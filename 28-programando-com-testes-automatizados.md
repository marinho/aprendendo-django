A **Biblioteca de Explicações Inquestionáveis** de Katara fica um tanto bom ao sul das **Montanhas Fusoé**. Nesta biblioteca foram encontradas algumas das explicações mais profundas para perguntas não muito rasas ao longo dos últimos milênios.

Próximo dali está um igualmente milenar mosteiro, onde ficaram reclusos alguns dos maiores questionadores da História, por sua própria vontade ou não. Deste mosteiro surgiram as perguntas mais surpreendentes e improváveis dos últimos milênios.

À medida que os últimos milênios se passaram, tornara-se cada vez mais irrefutável a reputação da sabedoria inquestionável exalada deste complexo.

Entre o mosteiro e a biblioteca há uma plantação de um arbusto verde-bandeira, com folhas que em geral possuem 5 pontas grandes e outras pontas menores, chamadas de **"folha das 5 sabedorias e outras coisas que são quase isso"**.

O **Ritual da Apresentação das Respostas**, é um disputado evento anual que acontece na biblioteca, algumas semanas após o **Ritual do Desdobramento das Perguntas** do mosteiro. O restante do ano é preenchido com atividades comuns.

O Ritual da Apresentação das Respostas é precedido por um momento de pensamento e alegria profundos, quando são oferecidos chás, bolos e cigarros produzidos com as plantas da região. Após a apresentação das respostas, todos os presentes saem dali com a certeza de que mais algumas perguntas foram esclarecidas, de maneira enfática e inquestionável.

Uma das primeiras perguntas resolvidas e devidamente registrada na biblioteca fala da origem do universo conhecido e de seus planetas:

<img src="http://www.aprendendodjango.com/media/img/ilustracao_12.gif" align="right"/>

_"Há milhares de anos, milhões de anos, milhares de milhões de anos, não haviam planetas, mas apenas uma fábrica de bolas de tênis. **Ogros** trabalhavam ali, fazendo as mais criativas bolas de tênis do universo conhecido._

_Uma bola de tênis, para atingir o grau divino de qualidade, necessitava ser construída com cera de ouvido e água, tornando-se uma mistura lamacenta ao redor de uma estrutura consistente e resistente às mais diversas raquetadas. Elas devem resistir ao calor, ao frio, ao movimento circular sem ficarem tontas e a colisões com outras bolas também. Para dar uma emoção um pouco maior ao esporte, é preciso que as bolas possuam uma superfície irregular e seja costurada com linhas de tal modo que entre uma raquetada e outra nunca se sabe com certeza para qual direção a bola será arremessada._

_A costura deve ser feita também de forma aleatória, para dificultar a infração dos direitos autorais e patentes da fabricação de bolas._

_Os **ogros fazedores de bolas** trabalhavam dia por dia, tempo por tempo e após cada bola concluída, davam-lhe uma raquetada e ela sumia para os confins do universo e passava a girar aleatoriamente em torno de alguma estrela, e por ali ficava."_

Dizem que isso também inspirou o surgimento dos primeiros protótipos e dos jogos com bolas.

## Mais testes e menos debug

Por mais bizarro que seja um caso, é possível descrever um cenário de testes para ele. Por mais complexo que seja, é possível separar parte por parte em testes automatizados até que haja um conjunto satisfatório.

**TDD** - Test Driven Development (ou _Design_) - é uma metodologia de desenvolvimento de software que se aplica com essa filosofia: **um software deve ser escrito para atender a um conjunto de testes automatizados, que devem ser melhorados e detalhados à medida que este software evolui**.

Isso quer dizer que você deve escrever alguns testes automatizados, codificar para atender a esses testes, escrever mais testes (ou detalhar os já existentes) e mais software, e seguir com esse ciclo de vida.

Mas como assim? Como testar algo que ainda não existe?

Algumas das motivações mais relevantes para esse paradigma são:

1. A documentação de regras de negócio de um software nunca é clara o suficiente até atingir uma dimensão impossível de se acompanhar. Há diversos casos onde se escreve 300 ou 500 páginas para detalhar a análise de um software e o resultado é quem ninguém é louco o suficiente para tentar ler essa quantidade de documentação a ponto de resolver ou compreender algo no software;

1. Quando um software recebe melhorias e evolui naturalmente, a cada versão lançada seria necessário o seu teste por completo, isso se torna inviável quando as versões são lançadas semanal ou mensalmente. A consequência é que não se testa por completo e ocorrem muitas ocasiões do tipo da _colcha curta_: **conserta de um lado e estraga do outro**;

1. Enquanto se escreve um conjunto de testes, muitas questões são esclarecidas ali, e a programação se torna mais limpa e o retrabalho diminui. Isso é porque você precisa pensar para escrever os testes, e isso o ajuda por exemplo a perceber que uma idéia _"brilhante"_ na verdade era uma **bomba relógio**;

1. **Depurar sofware** (especialmente para a Web) **é um _saco_**, e nem sempre a depuração será feita com a perfeição e o roteiro ideais.

E assim por diante, enfim: depois que você pega o jeito, escrever testes se torna um investimento, e quanto melhor testado é seu software, maiores as chances de sucesso.

### Como os testes se aplicam no Django

Há diversas formas de se escrever testes automatizados. No Django há pelo menos 4:

1. **Testes de unidade** - são usados para testar procedimentos isolados do software (sabe aquela fórmula de cálculo de um certo imposto?);

1. **DocTests textuais** - são usados para se testar **comportamentos** ou **roteiros** mais elaborados;

1. **DocTests em _docstrings_** - permitem que testes localizados sejam escritos nos próprios comentários de classes de modelo e seus métodos;

1. **Testes externos** - há frameworks como **Selenium** e **Twill** para testes de JavaScript e (X)HTML. Há também como se criar _scripts_ para fazer **testes de performance** e _stress_, dentre outros meios para testar um software.

Para colocar alguns desses modos em prática, vamos criar uma nova aplicação, chamada **"mordomo"**. Portanto, crie a pasta **"mordomo"** dentro da pasta do projeto e dentro dela crie dois arquivos vazios, chamados **"\_\_init\_\_.py"** e **"models.py"**.

Abra o arquivo **"settings.py"** e localize a _setting_ **INSTALLED\_APPS**, acrescentando a nova aplicação a ela, para ficar assim:

    INSTALLED_APPS = (
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.sites',
        'django.contrib.admin',
        'django.contrib.syndication',
        'django.contrib.flatpages',
        'django.contrib.comments',
    
        'blog',
        'galeria',
        'tags',
        'contas',
        'mordomo',
    )

Salve o arquivo. Feche o arquivo.

### Programando testes unitários

Testes unitários são testes de procedimentos que podem ser testados isoladamente. O Django possui um bom suporte para esse tipo de teste, através da classe **"django.test.TestCase"**.

Antes de mais nada, é preciso saber qual é a função dessa nova aplicação que estamos criando. A aplicação **"mordomo"** vai ter o papel de enviar notificações sobre o **estado do site** para os administradores do site e enviar também os **avisos de contas** que estão prestes a vencer.

Então, o que vamos fazer agora é criar um novo arquivo na pasta da aplicação **"mordomo"**, chamado **"tests.py"**, com o seguinte código dentro:

    from django.test import TestCase
    
    class TesteMordomo(TestCase):
        def testUmMaisUm(self):
            self.assertEquals(1 + 1, 3)

Salve o arquivo.

Agora na pasta do projeto, crie um novo arquivo chamado **"testar\_mordomo.bat"** com o seguinte código dentro:

    python manage.py test mordomo
    pause

Salve o arquivo. Feche o arquivo. Agora clique duas vezes sobre ele para executar o primeiro teste na nova aplicação:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/testes-primeiro-teste/file/"/>
</p>

Veja que o teste acusou um erro: **1 + 1 não é igual a 3** nem na Terra nem em lugar nenhum do universo que conhecemos.

Você pode notar que antes de executar o teste que escrevemos, todo o banco de dados foi gerado novamente. Isso ocorre porque de fato, ao executar os testes em um projeto, um banco de dados temporário é gerado para que os testes sejam feitos ali. Dessa forma, esse banco de dados é iniciado limpo e sem registros, exceto aqueles que foram definidos para serem gerados junto com a criação do banco.

Que tal melhorar um pouco mais os nossos testes?

Volte a editar o arquivo **"tests.py"** da pasta da aplicação **"mordomo"** e o modifique para ficar assim:

    import datetime
    
    from django.test import TestCase
    from django.contrib.auth.models import User
    
    from contas.models import Pessoa, Historico, ContaPagar, CONTA_STATUS_APAGAR,\
         CONTA_STATUS_PAGO
    
    class TesteMordomo(TestCase):
        def setUp(self):
            self.usuario, novo = User.objects.get_or_create(
                username='admin'
                )
            
            self.pessoa, novo = Pessoa.objects.get_or_create(
                usuario=self.usuario, nome='Maria Anita',
                )
    
            self.historico, novo = Historico.objects.get_or_create(
                usuario=self.usuario, descricao='Agua',
                )
    
        def tearDown(self):
            pass
        
        def testUmMaisUm(self):
            self.assertEquals(1 + 1, 2)
    
        def testObjetosCriados(self):
            self.assertEquals(User.objects.count(), 1)
            self.assertEquals(Pessoa.objects.count(), 1)
            self.assertEquals(Historico.objects.count(), 1)

Puxa, mas quantas mudanças! Então, vamos entender o que fizemos ali...

A classe que definimos possui 2 métodos de teste:

- testUmMaisUm
- testObjetosCriados

**Antes** de executar cada um dos métodos de teste, o método **"setUp"** é chamado, para preparar o terreno em que os testes irão trabalhar, após cada um deles, o método **"tearDown"** é executado para finalizar a bagunça feita, se necessário.

No caso acima, o teste **"testUmMaisUm"** não faz nada demais, e logo será removido, mas o teste **"testObjetosCriados"** verifica se os objetos básicos que criamos foram gravados com segurança no banco de dados, o que já é um passo na garantia de que algo está funcionando.

Salve o arquivo. Feche o arquivo. Agora clique duas vezes sobre o arquivo **"testar\_mordomo.bat"** da pasta do projeto e veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/testes-teste-com-2-testes/file/"/>
</p>

Observe que uma parte destacada ali diz:

    Ran 2 tests in 0.460s
    

E logo em seguida há outra linha que o teste fechou OK:

    OK
    Destroying test database...

### Escrevendo testes para depois programar

E agora que já temos uma visão geral de como funcionam os testes, vamos colocar a idéia da programação orientada a testes. Abra novamente o arquivo **"tests.py"** da pasta da aplicação **"mordomo"** para edição e acrescente este novo método de teste ao final:

        def testContasPendentes(self):
            from mordomo.models import obter_contas_pendentes
        
            for numero in range(-50,50):
                if numero % 2:
                    novo_status = CONTA_STATUS_APAGAR
                else:
                    novo_status = CONTA_STATUS_PAGO
                    
                nova_data = datetime.date.today() + datetime.timedelta(days=numero)
    
                novo_valor = numero + 70
                
                ContaPagar.objects.create(
                    usuario=self.usuario,
                    pessoa=self.pessoa,
                    historico=self.historico,
                    data_vencimento=nova_data,
                    valor=novo_valor,
                    status=novo_status,
                    )
            
            self.assertEquals(ContaPagar.objects.count(), 100)
    
            contas_pendentes = obter_contas_pendentes(
                usuario=self.usuario,
                prazo_dias=10,
                )
    
            self.assertEquals(contas_pendentes.count(), 30)

Novamente, temos um código longo. Vamos ver detalhe por detalhe?

A primeira coisa que será verificada será esta importação:

            from mordomo.models import obter_contas_pendentes
        

Se não existe a função **"obter\_contas\_pendentes"**, seguramente uma exceção do tipo **"ImportError"** será levantada antes que todo o restante seja chamado.

A próxima parte do código inicia um laço de **100 posições** entre os números -50 e 50 (que de fato pelos padrões do Python será de -50 a 49) para criar 100 contas a pagar:

            for numero in range(-50,50):
    

Os números vão de -50 a 49 pois cada uma das contas terá um **status**, **data de vencimento** e **valor** diferentes, entre contas vencidas e por vencer.

O próximo trecho do código escolhe um dos **status** que indicam se a conta já está paga ou ainda está pendente:

                if numero % 2:
                    novo_status = CONTA_STATUS_APAGAR
                else:
                    novo_status = CONTA_STATUS_PAGO

Na prática, o código acima indica o status **"CONTA\_STATUS\_APAGAR"** para quando **"numero"** for um valor ímpar e o status **"CONTA\_STATUS\_PAGO"** para quando ele for par. A operação **"numero % 2"** calcula o módulo **2** sobre o valor de **"numero"** que será sempre **1 (ímpar) ou 0 (par)**.

A próxima linha calcula uma data que para cada conta será incrementada em um dia:

                nova_data = datetime.date.today() + datetime.timedelta(days=numero)
    
Na prática, estamos calculando a data atual somada à quantidade de dias apontada pela variável **"numero"**, que vai de **-50** (50 dias atrás) a **49** (daqui a 49 dias), ou seja: metade das contas estarão vencidas e outra metade ainda a vencer.

A próxima linha calcula um valor que também é incrementado para cada conta:

                novo_valor = numero + 70
                
Na primeira conta, o valor será **"-50 + 70"**, ou seja: **20**. Na segunda ela será **21** e assim por diante.

O próximo trecho de código cria a conta a pagar:

                ContaPagar.objects.create(
                    usuario=self.usuario,
                    pessoa=self.pessoa,
                    historico=self.historico,
                    data_vencimento=nova_data,
                    valor=novo_valor,
                    status=novo_status,
                    )

Ela é criada usando os valores que calculamos e os objetos **"self.usuario"**, **"self.pessoa"** e **"self.historico"**.

Após as 100 contas serem criadas, fazemos uma pequena verificação:

            self.assertEquals(ContaPagar.objects.count(), 100)
    

A comparação acima serve apenas para confirmar que as 100 contas foram criadas com sucesso.

A próxima etapa faz uso da função que importamos no início do método:

            contas_pendentes = obter_contas_pendentes(
                usuario=self.usuario,
                prazo_dias=10,
                )

Ela será chamada para um usuário específico e com um prazo máximo em dias após a data atual. Lembre-se de que o mordomo deve avisar ao usuário das contas a vencer, e não somente daquelas que já venceram. **10 dias** é um prazo bacana.

Por fim, a lista de contas pendentes resultantes da chamada à função também é verificada:

            self.assertEquals(contas_pendentes.count(), 30)
    

Veja que temos 100 contas, das quais **60 delas** venceram antes da data atual ou vencerão até os próximos **10 dias**. Como somente a metade possui status **"a pagar"**, a quantidade de contas pendentes encontradas deve ser **igual a 30**.

Salve o arquivo. Feche o arquivo. Que tal agora executar nosso teste? Clique duas vezes sobre o arquivo **"testar\_mordomo.bat"** da pasta do projeto e veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/testes-erro-de-importacao/file/"/>
</p>

Como você já sabia, uma exceção de importação foi levantada, já que a função não existe. Pois então o primeiro passo na nossa nova forma de programar é resolver esse primeiro erro **criando a função**.

Na pasta da aplicação **"mordomo"**, abra o arquivo **"models.py"** para edição e escreva o seguinte código dentro:

    import datetime
    
    from django.contrib.auth.models import User
    
    from contas.models import Conta, CONTA_STATUS_APAGAR
    
    def obter_contas_pendentes(usuario, prazo_dias):
        delta = datetime.timedelta(days=prazo_dias)
        
        return Conta.objects.filter(
                usuario=usuario,
                status=CONTA_STATUS_APAGAR,
                data_vencimento__lte=datetime.date.today() + delta
                )

Nas primeiras linhas do arquivo, importamos os módulos necessários e declaramos a função:

    import datetime
    
    from django.contrib.auth.models import User
    
    from contas.models import Conta, CONTA_STATUS_APAGAR
    
    def obter_contas_pendentes(usuario, prazo_dias):

Logo em seguida definimos uma variável chamada **"delta"** para armazenar o objeto que representa uma faixa de data para a quantidade de dias informada no argumento **"prazo\_dias"**:

        delta = datetime.timedelta(days=prazo_dias)
        

E finalmente a função retorna as contas do **usuário informado**, com status **"a pagar"** e data de vencimento **menor ou igual** à data atual, somada do prazo de dias representado na variável **"delta"**:

        return Conta.objects.filter(
                usuario=usuario,
                status=CONTA_STATUS_APAGAR,
                data_vencimento__lte=datetime.date.today() + delta
                )

Salve o arquivo. Feche o arquivo. Execute o teste novamente e veja agora o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/testes-3-testes-com-sucesso/file/"/>
</p>

Bacana, agora está tudo certo!

### Testando e-mails enviados

Agora que temos uma função para carregar as contas pendentes para um usuário específico, precisamos criar outra para enviar a ele uma lista delas por e-mail.

Para isso, vamos novamente começar criando testes. Abra o arquivo **"tests.py"** da pasta da aplicação **"mordomo"** para edição e localize esta linha:

            from mordomo.models import obter_contas_pendentes
        

Modifique-a para ficar assim:

            from mordomo.models import obter_contas_pendentes, enviar_contas_pendentes
        

Agora acrescente o seguite trecho de código ao final do arquivo, mas ainda dentro do método **"testContasPendentes"**:

            enviar_contas_pendentes(
                usuario=self.usuario,
                prazo_dias=10,
                )
    
            from django.core import mail
    
            self.assertEquals(len(mail.outbox), 1)
            self.assertEquals(mail.outbox[0].subject, 'Contas a pagar e receber')

Simplificando o que as novas linhas de código fazem...

A primeira parte chama uma nova função, que envia ao usuário a lista das contas pendentes para seu e-mail:

            enviar_contas_pendentes(
                usuario=self.usuario,
                prazo_dias=10,
                )

Já na segunda parte, usamos o pacote **"django.core.mail"** para verificar se a caixa de mensagens ( **"mail.outbox"** ) possui **uma mensagem** e em seguida, verificamos se o título dela é **"Contas a pagar e receber"**:

            from django.core import mail
    
            self.assertEquals(len(mail.outbox), 1)
            self.assertEquals(mail.outbox[0].subject, 'Contas a pagar e receber')

Salve o arquivo. Feche o arquivo. Você pode executar os testes, mas o que vai verificar é que deve ser escrito o código para atender às novas especificações que ele aponta.

Para, isso, abra novamente o arquivo **"models.py"** da aplicação **"mordomo"** para edição e acrescente as seguintes linhas de código ao final:

    def enviar_contas_pendentes(usuario, prazo_dias):
        contas_pendentes = obter_contas_pendentes(usuario, prazo_dias)
    
        if not contas_pendentes.count():
            return False
        
        texto = """
        Voce tem as seguintes contas vencidas ou a vencer nos proximos %(dias)d dias:
    
        %(lista_de_contas)s
        """%{
            'dias': prazo_dias,
            'lista_de_contas': "\n".join([unicode(conta) for conta in contas_pendentes]),
            }
    
        return usuario.email_user(
            'Contas a pagar e receber',
            texto,
            )

Veja que nas primeiras duas linhas, nós declaramos a nova função e fazemos uso da função **"contas\_pendentes"**:

    def enviar_contas_pendentes(usuario, prazo_dias):
        contas_pendentes = obter_contas_pendentes(usuario, prazo_dias)

Em seguida, saímos da função caso não existam contas pendentes:

        if not contas_pendentes.count():
            return False

Definimos o **texto da mensagem**, formatando a _string_ indicando os dias e a lista de contas separadas por saltos de linha ( **"\n"** ):

        texto = """
        Voce tem as seguintes contas vencidas ou a vencer nos proximos %(dias)d dias:
    
        %(lista_de_contas)s
        """%{
            'dias': prazo_dias,
            'lista_de_contas': "\n".join([unicode(conta) for conta in contas_pendentes]),
            }

E por fim, a função retorna a quantidade de e-mails enviados pelo método **"email\_user"** do usuário, que envia um mensagem para o endereço de e-mail do usuário em questão:

        return usuario.email_user(
            'Contas a pagar e receber',
            texto,
            )

O primeiro argumento indica o título ( ou **_subject_** ) da mensagem e o segundo argumento indica seu **texto**.

Salve o arquivo. Feche o arquivo.

Agora, a resposta para sua pergunta mais profunda é: e-mails enviados durante a execução de testes são falsos. É... é verdade! Tudo funciona direitinho como se fosse de verdade, mas o que acontece é que eles são armazenados na variável **"outbox"** do pacote **"django.core.mail"**. As mesmas rotinas de e-mail funcionam corretamente quando o sistema estiver executando normalmente, fora do ambiente de testes.

### Trabalhando com DocTests

Testes unitários são legais, divertidos até, possuem muitos recursos... mas, são de fato, limitados.

Cada um daqueles métodos de testes deve ser encarado e pensado isoladamente, pois sua execução não segue uma ordem certa... como seus próprios nomes dizem: são **testes unitários**.

Para testar cenários mais complexos e compostos de várias etapas ou elementos - geralmente chamados de **testes de comportamento** - existem os **DocTests**. Os testes de comportamento, em um formato um pouco mais elaborado são conhecidos como **BDD** - **"Programação Orientada a Comportamentos"**.

Para conhecer um pouquinho de **DocTests**, abra o arquivo **"tests.py"** da aplicação **"mordomo"** para edição e acrescente as seguintes linhas de código ao início do arquivo, empurrando todo o restante para baixo:

    """
    Criar um comando para verificar se estah tudo ok na pagina principal do
    site e enviar um e-mail caso algum erro foi encontrado.
    
        >>> from mordomo.models import verificar_pagina_inicial
    
    Simulando status 200 - estado ok
    
        >>> def status_code_ok(url):
        ...     return 200
    
        >>> verificar_pagina_inicial(funcao_status_code=status_code_ok)
        True
    """

Veja que estamos trabalhando numa grande _string_. Logo de primeira já dá pra notar o quanto o DocTest é mais amigável, pois ele permite textos livres explicando detalhadamente o que está acontecendo ali, como se fosse uma história sendo contada.

As linhas iniciadas com **quatro espaços**, seguidas de três sinais de **"maior-quê"** e mais um espaço, iniciam um procedimento em Python. Caso esses procedimentos necessitem de mais linhas (como é um caso de uma função ou classe, por exemplo), então as linhas secundárias devem estar exatamente abaixo das anteriores, iniciadas sempre com **quatro espaços**, **três pontos** e mais um espaço, seguidas da identação comum à linguagem.

Já as linhas iniciadas com **quatro espaços** e seguidas de qualquer outra coisa, indicam que aquele é o valor esperado como resposta da linha de comando anterior, veja este caso por exemplo:

        >>> verificar_pagina_inicial(funcao_status_code=status_code_ok)
        True

A linha com **"    True"** indica que a função chamada na linha anterior deve retornar o valor **"True"**.

Viu como é simples?

O que fizemos aí foi uma função, chamada **"status\_code\_ok"**, que retorna forçadamente o código **200**, que é o código de status HTTP que significa que tudo está OK. Outros códigos são o **404** - que indica **"página não encontrada"** -, **500** - que indica **"erro no servidor"**-, e há muitos outros além desses.

Agora acrescente ao final do DocTest (antes da linha que finaliza com três aspas duplas, assim: **"""**) mais este trecho de código:

        >>> from django.core import mail
        >>> len(mail.outbox)
        0
    
    Simulando status 500 - estado de erro que envia e-mail
    
        >>> def status_code_500(url):
        ...     return 500
    
        >>> verificar_pagina_inicial(funcao_status_code=status_code_500)
        True
    
        >>> len(mail.outbox)
        1

O primeiro trecho de código verifica a quantidade de mensagens na caixa de e-mails enviados, que deve ser zero:

        >>> from django.core import mail
        >>> len(mail.outbox)
        0

Lembre-se de que, como diz o próprio comentário no DocTest que escrevemos, a função **"verificar\_pagina\_inicial"** deve enviar um e-mail de aviso somente se algo aconteceu de errado com a página inicial.

No segundo trecho de código, nós criamos outra função para forçar o código de status **500**, que indica erro na página:

        >>> def status_code_500(url):
        ...     return 500
    
        >>> verificar_pagina_inicial(funcao_status_code=status_code_500)
        True

E por fim o último trecho espera que após este erro simulado haja um e-mail enviado na caixa:

        >>> len(mail.outbox)
        1

Salve o arquivo. Feche o arquivo. Execute os testes, clicando duas vezes sobre o arquivo **"testar\_mordomo.bat"** para ver o resultado das nossas mudanças:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/testes-erros-com-doctests/file/"/>
</p>

Como pode ver, o nosso DocTest levantou uma fila de erros, mas todos eles foram resumidos como se fossem apenas um, por estas linhas, veja:

    Ran 4 tests in 1.162s
    
    FAILED (failures=1)

Note que se antes nós tínhamos 3 testes e agora temos 4, é porque o DocTest é interpretado pelo Django com um único teste, e é por isso que o sumário final aponta apenas **"1 falha"** ( "_failures=1_" ).

Que tal agora escrever o código que vai acalmar esses testes e voltá-los ao estado de **"tudo OK"**?

Abra o arquivo **"models.py"** da aplicação **"mordomo"** para edição e acrescente as seguintes linhas de código ao final:

    from django.core.mail import send_mail
    
    def verificar_pagina_inicial(funcao_status_code):
        status_code = funcao_status_code('http://localhost:8000/')
    
        if status_code != 200:
            send_mail(
                'Erro na pagina inicial do site!',
                'Ocorreu um erro no site, verifique!',
                'alatazan@gmail.com',
                ['alatazan@gmail.com'],
                )
            
        return True

Veja que em primeiro de tudo nós importamos a função de envio de e-mails do Django: **"send\_mail"**:

    from django.core.mail import send_mail
    

Depois disso a declaramos, com o argumento que vai receber a função que verifica o código de status da URL, e essa função é executada, recebendo a URL da página inicial:

    def verificar_pagina_inicial(funcao_status_code):
        status_code = funcao_status_code('http://localhost:8000/')

A seguir, o código de status é comparado com **200**, o único valor válido esperado. Se ele for diferente, temos um erro na página, e então o e-mail é enviado ao endereço de e-mail **"alatazan@gmail.com"**:

        if status_code != 200:
            send_mail(
                subject='Erro na pagina inicial do site!',
                message='Ocorreu um erro no site, verifique!',
                from_email='site_do_alatazan@gmail.com',
                recipient_list=['alatazan@gmail.com'],
                )

Por fim, o retorno esperado **"True"** é retornado.

Observando bem, você vai notar que usando DocTests nós criamos um teste mais humano e amigável, que segue uma sequência de ações para representar um cenário esperado de um comportamento do sistema.

Criando as funções que forçam o código do status da URL nós criamos situações falsas para que seja possível testar as diversas situações pelas quais o sistema pode passar. Com um pouco de prática e imaginação, os DocTests permitem a você uma variedade de testes quase infinita, mas isso, só depende de você!

Salve o arquivo. Feche o arquivo.

Execute o teste da aplicação, clicando duas vezes sobre o arquivo **"testar\_mordomo.bat"** e veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/testes-doctests-from-resultado-ok/file/"/>
</p>

Você deve estar se perguntando quando essas funções serão usadas fora do ambiente de testes... mas isso é para o próximo capítulo!

## Usando as funções do mordomo em uma ferramenta para linha de comando

Naquela noite, Alatazan refletiu sobre isso tudo, e viu que escrever os testes antes da codificação faz todo o sentido:

- Com testes bem escritos, diminui a necessidade de descrever documentação sobre o código, também diminui as fases de análise e arquitetura. Um teste bem escrito vale por tudo isso e ainda garante que o software continue funcionando...

Basicamente o que Alatazan fez hoje foi:

- Criar uma nova aplicação vazia e iniciar um arquivo chamado **"tests.py"** dentro dela;
- O arquivo **"tests.py"** também poderia ser uma pasta **"tests"**, com um arquivo **"\_\_init\_\_.py"** dentro dela;
- Testes unitários são testes para calcular procedimentos isolados e independentes;
- Usando os métodos de **assertação** - todos aqueles que começam com **"assert..."** - é possível verificar os valores da forma como se espera que eles sejam. Há métodos de assertação também para verificar valores semelhantes (ainda que não sejam iguais), valores verdadeiros ou falsos, exceções levantadas, validação de formulários, estados de URLs e outras coisas mais;
- Uma classe de testes deve herdar da classe **"django.test.TestCase"**;
- O método **"setUp"** é executado antes de cada um dos métodos de teste daquela classe de testes;
- O método **"tearDown"** é o contrário do **"setUp"**, pois é executado depois de cada um dos métodos de teste da classe;
- Nunca escreva testes longos antes de programar. Escreva de meia-dúzia a dez testes, codifique até cada um deles ser atendido, depois escreva mais de meia-dúdia a dez, e aos poucos vá quebrando esses testes em outros menores, cada vez mais detalhados e claros, e codificando para se ajustar a isso;
- Faça alguns testes com maiores quantidades de dados para verificar como a aplicação se comporta;
- **DocTests** são mais "rústicos" que testes unitários, mas têm mais poder, pois é possível testar cenários e comportamentos complexos;
- Os e-mails enviados durante a execução dos testes são falsos: eles são apenas armazenados na variável **"outbox"** do pacote **"django.core.mail"**;
- Escreva seu código sempre de forma flexível, aceitando funções para fazer determinadas rotinas de forma que possam ser substituídas por **funções falsas**, assim você pode testar com segurança e ainda mantém seu código fácil de se adaptar a futuras novidades;
- A _string_ de um DocTest deve ser escrita nas primeiras linhas do arquivo.

E pensando nisso, Alatazan dormiu... no dia seguinte o desafio seria criar os chamados **"management commands"**, que são recursos para se criar ferramentas executáveis na linha de comando.

