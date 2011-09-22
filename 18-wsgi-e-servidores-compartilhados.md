Houve uma época em que restavam poucos planetas e satélites próximos a Katara para serem habitados, e as pessoas perderam seu interesse por alguns deles em especial, talvez devido à falta de atmosfera, proximidade de Katara, falta de água ou qualquer outra desculpa que viesse à cabeça.

Isso não foi bom para os negócios de algumas empresas.

Foi por isso que a **Bangla Oportunidades Interplanetárias** lançou o slogan **"só para escolhidos a dedo"**. A parte do dedo não foi muito bem compreendida a princípio, mas ser um *escolhido* parecia ser bastante recompensador.

As sessões para escolha de novos clientes eram caracterizadas, dentre outras coisas, por avaliar eventos aleatoriamente e escolher aqueles que se esqueciam de parar de bater palmas ao final dos aplausos. Ou mesmo aqueles que batiam palmas sozinhos.

Atualmente a comunidade dos moradores do satélite **Katara-E** é o resultado disso, e é formada por uma população extremamente altruísta. Seus moradores compartilham suas casas, compartilham suas roupas, compartilham seus copos, suas mulheres, seus maridos, suas crianças e suas cuecas, especialmente as sujas.

Às vezes ocorrem algumas distrações como consequência disso, mas não interessa qual seja o desfecho, no final tudo é comemorado com uma boa cervejada e um enterro festivo.

## WSGI e servidores compartilhados

Sim, esta é a última bolacha do pacote do assunto de _deploy_.

O **WSGI** é um recurso recente do Python, criado para superar os demais. Ele é simples, muitos o chamam de *"cola"*, uma coisa que vai ficar entre outras duas, somente para dar liga entre elas, e trabalha bem tanto de forma dedicada quanto compartilhada. Em Katara chamariam isso de "recheio de sanduíche de rodoviária".

Vamos agora voltar ao servidor Windows e tratar de substituir o **mod\_python** pelo WSGI.

### Baixando e instalando o mod\_wsgi para Windows

O **mod\_wsgi** é o módulo de suporte a **WSGI** para Apache.

Para fazer o download do **mod\_wsgi** para Windows - sem a necessidade de compilar - vá a esta página da Web:

    http://adal.chiriliuc.com/mod_wsgi/revision_1018_2.3/
    

Escolha a opção mais adequada à sua versão.

No caso que temos trabalhado (**Apache 2.2** e **Python 2.5**), a versão a escolher é esta:

    mod_wsgi_py25_apache22
    

Faça o download do arquivo **"mod\_wsgi.so"**. Ele deve ser colocado na seguinte pasta do HD:

    C:\Arquivos de programas\Apache Software Foundation\Apache2.2\modules
    

Pronto. Instalado. É um processo rústico, mas simples.

### Configurando o Apache para usar WSGI com o seu projeto

Agora, vamos modificar o Apache para que ele acesse seu projeto usando o **WSGI**.

Abra o arquivo **"httpd.conf"** da pasta **"C:\Arquivos de programas\Apache Software Foundation\Apache2.2\conf"** para edição e localize a seguinte linha:

    LoadModule python_module modules/mod_python.so
    

Abaixo dela, acrescente mais esta:

    LoadModule wsgi_module modules/mod_wsgi.so
    

Isto vai habilitar o **mod\_wsgi** no seu Apache.

Agora localize este trecho de linhas:

    <Location "/">
        SetHandler python-program
        PythonPath "['c:/var/django/projects/site_meu_blog/meu_blog'] + sys.path"
        PythonHandler django.core.handlers.modpython
        SetEnv DJANGO_SETTINGS_MODULE settings
        PythonDebug On
    </Location>

**Remova-o**. Ou se preferir, copie para um bloco de notas para consultar depois se precisar.

Agora escreva o seguinte código no mesmo lugar de onde removeu o anterior:

    WSGIScriptAlias / "c:/var/django/projects/site_meu_blog/meu_blog/deploy/default.wsgi"
    
    <Directory "c:/var/django/projects/site_meu_blog/meu_blog/deploy/">
        Order deny,allow
        Allow from all
    </Directory>

Como pode ver, fizemos uma referência para dizer ao Apache que as URLs iniciadas com uma barra (ou seja: **todas, exceto às de arquivos estáticos**) devem ser direcionadas para o script WSGI no arquivo **"default.wsgi"** da pasta **"deploy"** do projeto. Veja:

    WSGIScriptAlias / "c:/var/django/projects/site_meu_blog/meu_blog/deploy/default.wsgi"

Já no segundo bloco, nós definimos a permissão para que o Apache acesse a pasta deste script:

    <Directory "c:/var/django/projects/site_meu_blog/meu_blog/deploy/">
        Order deny,allow
        Allow from all
    </Directory>

Salve o arquivo. Feche o arquivo.

**Reinicie o Apache** pelo ícone na bandeja do Windows, próximo ao relógio.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/mod_python-reiniciando-apache/file/"/>
</p>

Vá ao navegador e carregue a URL do servidor:

    http://localhost/
    

Mas veja o que acontece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/wsgi-not-found/file/"/>
</p>

É que... Acontece que este arquivo de _script_ ainda não existe. Portanto, vamos criá-lo!

### Script para o projeto trabalhar com WSGI

Abra a pasta do projeto no servidor:

    c:\var\django\projects\site_meu_blog\meu_blog
    

Agora crie a seguinte pasta - se ela ainda não existir:

    deploy
    

E agora dentro da pasta **"deploy"**, crie um arquivo chamado **"default.wsgi"**, com o seguinte código dentro:

    import os, sys
    
    PROJECT_ROOT_PATH = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
    sys.path.insert(0, PROJECT_ROOT_PATH)
    os.environ['DJANGO_SETTINGS_MODULE'] = 'settings'
    
    import django.core.handlers.wsgi
    application = django.core.handlers.wsgi.WSGIHandler()

Agora observe o que fizemos:

Na primeira linha simplesmente importamos os pacotes de **sistema operacional** e de **sistema** do Python.

    import os, sys
    

No bloco seguinte, nós encontramos a **pasta do projeto** (uma acima da pasta do script), inserimos seu caminho **no início da PYTHONPATH** e definimos o módulo de **settings** para o Django:

    PROJECT_ROOT_PATH = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
    sys.path.insert(0, PROJECT_ROOT_PATH)
    os.environ['DJANGO_SETTINGS_MODULE'] = 'settings'

Por fim, importamos o _handler_ do Django para trabalhar com **WSGI** e declaramos a variável **"application"** atribuída de uma instância do _handler_, veja:

    import django.core.handlers.wsgi
    application = django.core.handlers.wsgi.WSGIHandler()

O WSGI trabalha sempre procurando pela variável **"application"** no script em questão. Portanto, não mude este nome, ele é **fundamental**.

Salve o arquivo. Feche o arquivo.

**Reinicie o Apache** pelo ícone na bandeja do Windows, volte ao **navegador**, atualize com **F5** e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/wsgi-com-sucesso/file/"/>
</p>

É isso aí! Como pode ver, agora temos o servidor trabalhando em **WSGI**!

### Ajustando o servidor para trabalhar compartilhado

Um servidor compartilhado é aquele que permite **vários sites** usando os mesmos recursos. É uma alternativa de baixo custo e bastante acessível, onde a quantidade de sites ativos pode ser até de 500 sites, muitas vezes utilizando tecnologias variadas como PHP, Python, Ruby e Perl numa só máquina.

Muitas vezes isso não é uma boa escolha, mas claro que tudo depende de como as limitações técnicas são exploradas.

O funcionamento de um servidor compartilhado em geral segue o seguinte raciocínio:

1. Cada site possui um **arquivo de configuração** e uma **pasta de documentos**. Respectivamente semelhantes ao **"httpd.conf"** e à pasta "C:\Arquivos de programas\Apache Software Foundation\Apache2.2\htdocs".

1. O arquivo de configuração é acessível somente ao administrador do servidor. Portanto, ele deve suportar que alguns ajustes sejam feitos por você através de um arquivo chamado **".htaccess"** na pasta de documentos do seu site.

1. O arquivo **".htaccess"** por sua vez declara algumas regras para liberar alguns caminhos da pasta de documentos, e direcionar o restante para o Django. Para isso, ele precisa de um módulo do Apache: p **"mod\_rewrite"**.

Pois bem. Nós vamos agora ajustar o Apache para trabalhar com o mesmo comportamento de um site de servidor compartilhado.

Então, para começar, abra o arquivo **"httpd.conf"** da pasta **"C:\Arquivos de programas\Apache Software Foundation\Apache2.2\conf"** para edição. Localize **e remova** o seguinte bloco, já no final do arquivo:

    WSGIScriptAlias / "c:/var/django/projects/site_meu_blog/meu_blog/deploy/default.wsgi"
    
    <Directory "c:/var/django/projects/site_meu_blog/meu_blog/deploy/">
        Order deny,allow
        Allow from all
    </Directory>
    
    Alias /media "C:/var/django/projects/site_meu_blog/meu_blog/media"
    
    <Directory "C:/var/django/projects/site_meu_blog/meu_blog/media">
        Order allow,deny
        Allow from all
    </Directory>
    
    <Location "/media">
        SetHandler None
    </Location>

Salve o arquivo. **Reinicie** o Apache.

Veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/apache-working/file/"/>
</p>

Voltamos à estaca zero? Nem tanto. Mas agora localize a seguinte linha no arquivo:

    #LoadModule rewrite_module modules/mod_rewrite.so
    

E remova o caracter de comentário ( # ), para ficar assim:

    LoadModule rewrite_module modules/mod_rewrite.so
    

Isso vai ativar o **"mod\_rewrite"** no Apache.

Agora acrescente esse trecho ao final do arquivo:

    AddHandler wsgi-script .wsgi
    
    <Directory "C:/Arquivos de programas/Apache Software Foundation/Apache2.2/htdocs">
        AllowOverride FileInfo
        Options ExecCGI MultiViews FollowSymLinks
        MultiviewsMatch Handlers
        
        Order allow,deny
        Allow from all
    </Directory>

Veja só o que fizemos:

Aqui nós vinculamos uma **extensão de arquivo** ao formato de script de WSGI:

    AddHandler wsgi-script .wsgi
    

E aqui, modificamos a configuração da **pasta de documentos** para suportar a configuração do site usando **".htaccess"** e scripts **WSGI**:

    <Directory "C:/Arquivos de programas/Apache Software Foundation/Apache2.2/htdocs">
        AllowOverride FileInfo
        Options ExecCGI MultiViews FollowSymLinks
        MultiviewsMatch Handlers
        
        Order allow,deny
        Allow from all
    </Directory>

Salve o arquivo. Feche o arquivo. **Reinicie** o Apache.

### Trabalhando na sua pasta de documentos

O próximo passo agora é copiar a pasta de **arquivos estáticos** do seu projeto para a **pasta de documentos**. Isso é necessário pois o **Windows** não tem suporte a **links simbólicos**, um recurso do Linux, MacOS X e outros sistemas operacionais.

Copie a pasta **"media"** do projeto e vá ao navegador, carregue a URL abaixo e veja se o Apache a encontra:

    http://localhost/media/layout.css
    

Ele deve ser carregado assim:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/wsgi-pasta-media/file/"/>
</p>

Isso indica que a pasta foi carregada corretamente e que o Apache a encontra da forma esperada.

Feito isso, ainda na pasta de documentos ( **"C:\Arquivos de programas\Apache Software Foundation\Apache2.2\htdocs"** ), crie o arquivo **".htaccess"** com o seguinte código dentro:

    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(.*)$ /site.wsgi/$1 [QSA,PT,L]

Vamos detalhar o que essas linhas significam? Veja:

Ativamos o **rewriting** da pasta de documentos. A partir desse ponto as URLs são direcionadas de acordo com as regras abaixo desta linha:

    RewriteEngine On
    

Dizemos que se o caminho da URL requisitada existir na pasta de documentos, ela terá prioridade sobre o Django. É esta linha que dá vida à pasta **"media"** trabalhando com arquivos estáticos.

    RewriteCond %{REQUEST_FILENAME} !-f
    

Dizemos que todas as demais URLs (que não existirem enquanto arquivos da pasta) serão repassadas ao nosso projeto em Django. Representado por um *script* da pasta de documentos, chamado **"site.wsgi"**.

    RewriteRule ^(.*)$ /site.wsgi/$1 [QSA,PT,L]
    

Salve o arquivo. Feche o arquivo.

Como pode notar, o arquivo de *script* **"site.wsgi"** de fato não existe. Pois então, ainda na **pasta de documentos**, crie um novo arquivo chamado **"site.wsgi"** com o seguinte código dentro:

    import os, sys
    
    PROJECT_ROOT_PATH = 'C:/var/django/projects/site_meu_blog/meu_blog'
    sys.path.insert(0, PROJECT_ROOT_PATH)
    os.environ['DJANGO_SETTINGS_MODULE'] = 'settings'
    
    import django.core.handlers.wsgi
    
    application = django.core.handlers.wsgi.WSGIHandler()

Este script é muito semelhante ao que criamos no início do capítulo, mas o caminho do projeto ( **"C:/var/django/projects/site\_meu\_blog/meu\_blog"** ) é apontado manualmente, sem uso de recursos do Python. Isso porque agora estamos fora da pasta do projeto e não temos referência relativa a ela.

Salve o arquivo. Feche o arquivo. Volte ao navegador e carregue a URL do site:

    http://localhost/
    

E veja que tudo voltou ao normal:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/wsgi-funcionando/file/"/>
</p>

Pronto! Temos agora o site funcionando novamente, porém desta vez usando **WSGI com rewriting**, um recurso fundamental para sites compartilhados.

## Agora a ordem é: evoluir!

Após essas aulas sobre *deploy*, Alatazan decidiu por contratar um servidor compartilhado, com Linux, Apache e WSGI. Foi fácil colocar seu site no ar, pois como o servidor já estava devidamente configurado, seu trabalho se resumiu em criar um **link simbólico** para a pasta **"media"**, o arquivo **".htaccess"** e o script de **WSGI** apontando para seu projeto.

Ainda assim, manteve a configuração no Windows para testes eventuais.

Voltando para casa, um bêbado notou seus grandes olhos e tom peculiar de pele. Num ataque de ecologismo, cambaleante e vendo dois ou três Alatazans de uma só vez, o bêbado se pôs a gritar:

- Eu vi uma zebra! Eu vi uma zebra!

O que ele ainda não havia entendido era o que a cor da pele e os olhos de Alatazan tinham a ver com a **zebra**. Talvez fosse seu subconsciente - também muito tonto - se lembrando das notícias daquela manhã.

As notícias que vira na TV eram sobre sinais estranhos e inexplicáveis, deixados no meio de uma plantação de cana. Lá havia também uma toalha para limpar o suor e uma lâmina de um metal desconhecido. As expressões "apocalipse", "sinais dos tempos" e "prejuízo nesta safra" eram as mais comuns no vocabulário do noticiário, mas enfim, isso tabém não lembrava uma zebra. Exceto por aquela zebra comendo cana no canto superior direito da tela da TV.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_11.gif" align="right"/>

Alatazan deu de ombros e continuou seu caminho, lembrando do que aprendeu hoje, que resumidamente foi:

- Configurar um servidor Apache com WSGI, usando **mod\_wsgi**;
- Criar script de WSGI na pasta **"deploy"** do projeto;
- Habilitar o **mod\_rewrite** no Apache;
- Ativar recursos e sobrecarga na **pasta de documentos** do Apache, para suportar scripts WSGI e o arquivo **".htaccess"**;
- Copiar a pasta **"media"** do projeto para a **pasta de documentos**;
- Criar script de WSGI na **pasta de documentos** para acessar o projeto.

E foi só isso mesmo! No outro dia, seria a vez de usar **_signals_** e **URLs descomplicadas**.

<span class="no_print">
**Próximo capítulo: [Signals e URLs amigáveis com Slugs](/signals-e-urls-amigaveis-com-slugs/)**
</span>

