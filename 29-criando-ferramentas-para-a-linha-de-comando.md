Alatazan estava em êxtase!

Esse era o último dia que estudava Django na modalidade básica junto com seus amigos. Nos últimos dias, Cartola e Nena estiveram distantes, ora viajando, ora envolvidos em seus trabalhos de final de ano, mas Alatazan estava ali firme e queria resolver uma última coisa antes de entrar de férias: executar algumas coisas do site a partir de um _prompt_ de linhas de comando.

O desafio de hoje - o derradeiro desse estudo - mostra que é absurdamente simples criar esse tipo de _script_ no Django. Muito mais simples do que tentar reinventar a roda, com certeza.

Tenha um bom estudo e aproveite com a mesma paixão que Alatazan estudou nesses últimos meses!

## Ferramentas para a linha de comando

Às vezes é necessário criar uma ferramenta para ser chamada pela linha de comando. Algumas vezes é preciso criar esse tipo de ferramenta para automatizar tarefas, e outras vezes para facilitar a execução de certas rotinas que são feitas manualmente de vezes em quando.

A nossa aplicação **"mordomo"** foi criada para fazer algumas dessas tarefas, especialmente duas:

1. Verificar se a página principal do site está OK e enviar um e-mail caso algo esteja dando errado;
1. Enviar e-mails periódicos aos usuários que possuam contas a pagar em até 10 dias contando a partir da data atual.

### Criando um primeiro _script_

A reação natural a essa situação é criar um _script_ externo ao ambiente do Django e tentar resolver tudo por conta própria, mas o resultado geralmente é um conjunto de pequenas gambiarras, isso quando não se desiste antes da hora.

O que muita gente não sabe é que o Django tem recursos especificamente para isso.

Quando você executa um comando como este:

    python manage.py syncdb
    

Ou este:

    python manage.py dumpdata contas > contas.json
    

Mal sabe que os comandos **"syncdb"** e **"dumpdata"** são arquivos que seguem uma API simples do Django, criada para isso. Então, você pode ter seus próprios **"python manage.py verificar\_status"** ou **"python manage.py enviar\_contas\_por\_email"** sem nenhuma dificuldade!

A esse tipo de recurso, usamos o nome de **"Management Commands"**.

### Criando o primeiro comando

Na pasta da aplicação **"mordomo"**, crie uma pasta chamada **"management"** e dentro dela um arquivo vazio, chamado **"\_\_init\_\_.py"**. Dentro da nova pasta, crie outra, chamada **"commands"** e dentro dela outro arquivo, também vazio, também chamado **"\_\_init\_\_.py"**.

Agora dentro da pasta **"commands"** crie um arquivo chamado **"verificar\_status.py"**, com o seguinte código dentro:

    from django.core.management import BaseCommand
    
    class Command(BaseCommand):
        def handle(self, **kwargs):
            print 'Funciona!'

Salve o arquivo.

Agora na pasta do projeto, crie um novo arquivo chamado **"verificar\_status.bat"** com o seguinte código dentro:

    python manage.py verificar_status
    pause

Salve o arquivo. Feche o arquivo. Clique duas vezes sobre o novo arquivo para executá-lo e veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/comandos-primeira-versao/file/"/>
</p>

Viu como é fácil? Agora vamos dar mais um passo. Na pasta da aplicação **"mordomo"**, abra o arquivo **"models.py"** para edição e localize estas linhas de código:

    def verificar_pagina_inicial(funcao_status_code):
        status_code = funcao_status_code('http://localhost:8000/')

Nós temos aí dois problemas:

1. O argumento **"funcao\_status\_code"** não possui um valor padrão, o que faz esta função ser dependente de uma função ser atribuída externamente sempre que ela for chamada;
1. A função está presa à URL **"http://localhost:8000/"**, o que faz dela limitada.

Para resolver o primeiro problema, escreva a seguinte função acima das linhas de código encontradas:

    import urllib2
    
    def obter_status_code(url):
        try:
            urllib2.urlopen(url)
        except urllib2.HTTPError, e:
            return e.code
        except Exception:
            return 0
    
        return 200

Esta função faz nada mais do que retornar o código de status da URL requisitada, que é exatamente do que precisamos. Ela tenta a conexão normal à URL. Se a conexão levantar qualquer erro do tipo **"urllib2.HTTPError"**, a função retorna o código de status do erro. Se for outro erro qualquer, ela retorna **0** ( zero ). E se der tudo certo, ela retorna **200**: o código de status que significa **OK**.

Agora modifique as seguintes linhas:

    def verificar_pagina_inicial(funcao_status_code):
        status_code = funcao_status_code('http://localhost:8000/')

Para ficar assim:

    def verificar_pagina_inicial(
        funcao_status_code=obter_status_code,
        url='http://localhost:8000/',
        ):
        status_code = funcao_status_code(url)

Ou seja, indicamos a função que criamos como valor padrão para o argumento **"funcao\_status\_code"** e substituimos a URL pelo argumento **"url"**, que por sua vez possui como valor padrão a mesma URL que usávamos antes de forma fixa.

Salve o arquivo. Feche o arquivo. Agora volte a editar o arquivo **"verificar\_status.py"** da pasta **"mordomo/management/commands"**, partindo da pasta do projeto e o modifique para ficar assim:

    from django.core.management import BaseCommand
    from mordomo.models import verificar_pagina_inicial
    
    class Command(BaseCommand):
        def handle(self, **kwargs):
            verificar_pagina_inicial()
            print 'URL verificada!'

Salve o arquivo. Execute o arquivo **"verificar\_status.bat"** da pasta do projeto novamente, para que ele funcione corretamente:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/comandos-verificacao-de-status-simples/file/"/>
</p>

### Trabalhando com opções

Agora, é hora de melhorar um pouco mais o nosso comando, adicionando uma opção importante: a URL a ser verificada.

Para isso, volte ao arquivo que acabamos de modificar ( **"verificar\_status.py"** ) e o modifique novamente, para ficar assim:

    from optparse import make_option
    
    from django.core.management import BaseCommand
    from django.conf import settings
    
    from mordomo.models import verificar_pagina_inicial
    
    class Command(BaseCommand):
        option_list = BaseCommand.option_list + (
            make_option('--url',
                        default='/',
                        dest='url',
                        help='Informe a URL para verificar o status',
                ),
            )
        help = 'Verifica uma URL e envia e-mail ao admin se estiver com erro'
        requires_model_validation = True
        
        def handle(self, url, **kwargs):
            url = settings.PROJECT_ROOT_URL[:-1] + url
            verificar_pagina_inicial(url=url)
            print 'URL "%s" verificada!'%url

Uau! Vamos detalhar o que o nosso novo código faz!

Primeiro, nossas importações aumentaram, agora usamos a biblioteca **"optparse"** do Python para ter o recurso de opções em nosso _script_. Também usamos o módulo **"settings"** do projeto para carregar dali a URL padrão do site:

    from optparse import make_option
    
    from django.core.management import BaseCommand
    from django.conf import settings
    
    from mordomo.models import verificar_pagina_inicial

Em seguida, acrescentamos uma nova opção além daquelas que o próprio Django tem por padrão em todas elas, que é a opção **"--url"**, que possui um valor _default_: **"/"**. O valor desta opção será concatenado à URL raiz do projeto (que por padrão é **"http://localhost:8000/"**):

    class Command(BaseCommand):
        option_list = BaseCommand.option_list + (
            make_option('--url',
                        default='/',
                        dest='url',
                        help='Informe a URL para verificar o status',
                ),
            )

O atributo a seguir tem a função de exibir uma mensagem de ajuda sobre o comando:

        help = 'Verifica uma URL e envia e-mail ao admin se estiver com erro'
    

E este outro exije que o Django faça uma validação nos arquivos de modelo antes de permitir a execução do comando:

        requires_model_validation = True
    

Por fim, o método que executa a ação do comando foi modificado para receber o argumento **"url"** e concatená-lo à URL raiz do site:

        def handle(self, url, **kwargs):
            url = settings.PROJECT_ROOT_URL[:-1] + url
            verificar_pagina_inicial(url=url)
            print 'URL "%s" verificada!'%url

Salve o arquivo. Feche o arquivo. Agora, para finalizar a nossa mudança, abra o arquivo **"settings.py"** da pasta do projeto para edição e localize esta linha de código:

    PROJECT_ROOT_PATH = os.path.dirname(os.path.abspath(__file__))
    

Acrescente esta abaixo dela:

    PROJECT_ROOT_URL = 'http://localhost:8000/'
    

Salve o arquivo. Feche o arquivo. Agora para usar as mudanças que fizemos abra o arquivo **"verificar\_status.bat"** da pasta do projeto para edição e o modifique para ficar assim:

    python manage.py verificar_status --url=/contas/
    pause

Salve o arquivo. Feche o arquivo. Execute esse arquivo e veja como ele se sai:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/comandos-com-opcao-url/file/"/>
</p>

Fantástico! Temos uma ferramenta de comandos e foi mole, mole pra fazer!

## Terminando o curso e partindo para novos desafios

Alatazan falou em voz alta, como que estivesse fazendo um gol:

- Em contagem regressiva, vamos ver o que vimos hoje... é tão pouco que vou ser breve:

 - O primeiro passo foi criar a pasta **"management/commands"** dentro da pasta da aplicação, com seus respectivos **"\_\_init\_\_.py"**;
 - Dentro da pasta, criamos um arquivo com o mesmo nome do comando que usei: **"verificar\_status.py"**. É preciso um pouco de cuidado pra evitar criar comandos que já existem em outras aplicações com o mesmo nome;
 - O novo _script_ possui uma classe chamada **"Command"**, um nome padrão, que extende da classe **"django.core.management.BaseCommand"** e declara um método **"handle"** para executar a ação do comando;
 - Para criar uma opção, usamos a biblioteca **"optparse"** e dela usamos a função **"make\_option"**.
 - O atributo **"help"** é bacana para ajudar ao usuário a usar aquele comando e a opção em questão;
 - O atributo **"requires\_model\_validation"** determina que este comando depende de uma validação dos arquivos de modelo antes, isso porque os arquivos de modelo são importantes neste caso.

- E é só isso!

Alatazan recebeu um telefonema. Para ele foi um alívio danado saber que agora teria um emprego. O que ele não sabia ainda era que aquela oportunidade tinha um lado negro... e Nena sabe muito bem do que se trata um **emprego tradicional**.

