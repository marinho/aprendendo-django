**Atenção:** antes de realizar este capítulo, verifique se você já aplicou a modificação na setting **"ROOT\_URLCONF"** atualizada no capítulo 14: <a href="http://www.aprendendodjango.com/ajustando-as-coisas-para-colocar-no-ar/">Ajustando as coisas para colocar no ar</a>.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_1.gif" align="left"/>

Um índio passou em seu cavalo a galopes entre duas rochas. Havia poeira no ar, e também havia o cheiro da poeira, o Sol castigava a poeira, e a terra vermelha socada por séculos de pouca chuva e muito vento, fazia desenhos através do deserto, com muita poeira.

Os desenhos serpenteavam por aqui e ali, e pelas serpentes, ora passavam cavalos, ora passavam rios, ora passava uma criança chorando no colo de sua mãe, segurando firme seu iPod.

O índio de pele vermelha - não se sabe se era devido ao Sol, devido à terra avermelhada da região ou devido à sua timidez naquele momento - galopou novamente até uma lagoa, entregou peixes à boca de um golfinho, montou seu cavalo novamente e rumou a um celeiro, atravessando por uma janela larga e alta, e...

Opa! Voltando os olhos novamente àquele menino que não para de chorar, agora sentado com sua mãe, assistindo aos colegas terminarem a peça, morrendo de inveja porque não ia ganhar os aplausos do teatro e nem o novíssimo MelecaBoy verde, uma nova mania gosmenta da sessão de desenhos, e... enfim, isso nem interessa...

Ao final, **Cadu** correu até **Cartola**, tirou uma onda, curtiu seu momento de ator-mirim e voltou a rodopiar por ali.

Mas Alatazan ainda estava um pouco ligado no assunto do servidor para o dia seguinte...

## Mas porquê construir um servidor no Windows?

A maior parte dos servidores web são **Linux**. Por alguns motivos, em grande parte dos casos os sistemas de padrão Unix, como Linux, Solaris e MacOS X, são mais adequados para servidores quando se trata de Django.

Mas o mundo é _multi_, e vamos falar agora de como se fazer as coisas no Windows. Os procedimentos são semelhantes nos dois sistemas operacionais, e a parte mais importante - a configuração para ter seu projeto funcionando - é realmente muito parecida entre ambos, portanto seu aprendizado será útil em qualquer caso, mas agora vamos ao trabalho!

Você pode usar a mesma máquina que tem usado para estudar o Django, pois não se tratam de recursos conflitantes.

Esta é uma situação bastante normal para **intranets** e sistemas em **redes locais**.

### Instalando o Apache

A primeira coisa a se fazer é instalar o **Apache** - de preferência na **versão 2.2**. Para isso vá ao site oficial e faça o download do arquivo de instalação:

    http://httpd.apache.org/download.cgi
    

Localize agora um bloco que inicia com **"Apache HTTP Server 2.2.10 is the best available version"** ou algo bem semelhante a isso. Nesse bloco há uma lista das opções mais comuns de download do Apache. Escolha o **instalador para Windows, sem criptografia**, que em inglês está assim:

    Win32 Binary without crypto (no mod_ssl) (MSI Installer):
    

Ou quase isso:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/apache-pagina-de-download/file/"/>
</p>

Clique no link para fazer o download.

Enquanto o download é feito, sua mente deve estar se perguntando **"porquê há um ar de indefinição aqui?"**.

Software é um produto **dinâmico**, e **software livre** é quase **orgânico** e evolui rapidamente. É provável que ao final deste livro a versão **mais recente** do Apache será outra, e até mesmo do Django. Ou seja, é impossível indicar um caminho exato para isso hoje, se você vai usá-lo amanhã. Portanto fique ligado e observe que os números da **versão** podem mudar rapidamente.

Ao final do download, clique duas vezes sobre o arquivo baixado, que será executado e a primeira janela será esta:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/apache-instalacao/file/"/>
</p>

Siga clicando no botão **"Next"** e veja a janela seguinte:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/apache-instalacao2/file/"/>
</p>

Marque na primeira opção, indicando que aceita a licença do Apache, e siga clicando em **"Next"** até chegar a esta outra janela:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/apache-instalacao3/file/"/>
</p>

Preencha os respectivos valores para os campos. Caso não saiba o que preencher ali, preencha da seguinte forma:

* Network Domain: **"localhost"**
* Server Name: **"localhost"**
* Administrator's E-mail Address: **"seu_email@qualquercoisa.com"**

Modifique o campo **"Administrator's E-mail Address"** de acordo com seu próprio e-mail e siga adianta clicando em **"Next"** até finalizar.

Ao finalizar a instalação, verifique o **ícone do Apache na bandeja** do Windows, próximo ao relógio:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/apache-tray-icon/file/"/>
</p>

Agora vá ao navegador e carregue o seguinte endereço:

    http://localhost/
    

Observe que desta vez não informamos a porta, pois estamos indicando que ela deve ser a porta **HTTP** padrão: **a 80**. Veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/apache-working/file/"/>
</p>

Viu como é fácil ter o Apache funcionando em sua máquina Windows?

### Instalando o MySQL

Agora vamos instalar o MySQL, o banco de dados que vamos usar neste servidor.

Para isso, vá ao site oficial de download do MySQL:

    http://dev.mysql.com/downloads/mysql/5.1.html#win32
    

A versão mais indicada é a **versão 5.1**. Na opção **"Windows Essentials (x86)"**, clique no link **"Pick a mirror"**.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/mysql-download/file/"/>
</p>

Será requisitado um cadastro gratuito, apenas para fins de *propaganda* mesmo. Faça-o (não há outra forma, *sorry*) e siga para selecionar um dos servidores **"mirrors"** para download.

Após o download, clique duas vezes sobre o arquivo baixado e inicie a instalação do MySQL no Windows.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/mysql-instalacao/file/"/>
</p>

Siga clicando no botão **"Next"** até finalizar a instalação.

Ao final será exibida uma nova janela para configuração do MySQL (que por sinal é muito semelhante às janelas de instalação).

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/mysql-configuracao/file/"/>
</p>

Prossiga clicando sobre **"Next"**, modificando as opções padrão somente se estiver certo de tal mudança. Até chegar a essa tela:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/mysql-configuracao2/file/"/>
</p>

Escolha a segunda opção ( **"Best Support for Multilingualism"** ). Isso vai tornar a codificação de caracteres padrão em **"UTF-8"**, a que melhor se relaciona com o Django, e evitar alguma eventual chatice alguns dias à frente.

Prossiga clicando em **"Next"** até chegar a esta tela:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/mysql-configuracao2-5/file/"/>
</p>

Marque a caixa **"Include Bin Directory to Windows PATH"**.

Prossiga clicando em **"Next"** até esta outra tela:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/mysql-configuracao3/file/"/>
</p>

Determine uma senha difícil de ser quebrada, mas caso tenha dúvidas, simplesmente informe:

* Password: **"123456"**
* Confirm: **"123456"**

Porquê eu sugiro uma senha tão fácil de ser quebrada? Simples: **para que você não o faça!**

Prossiga clicando em **"Next"** até **executar** a configuração e concluí-la.

Pronto! Agora temos o MySQL rodando em seu servidor Windows!

### Python e Django

Caso esta máquina não possua Python ou Django, faça a instalação seguindo os mesmos passos do capítulo 3 - **Baixando e Instalando o Django** - com a diferença que desta vez não é preciso instalar a biblioteca de acesso ao banco de dados SQLite (pySQLite), pois vamos usar o MySQL.

### Biblioteca de acesso ao MySQL

Desta vez, a biblioteca de acesso ao banco de dados será a **"MySQL for Python"**, a biblioteca responsável por isso no Python.

Para fazer o download, vá à seguinte página da web:

    http://sourceforge.net/projects/mysql-python
    

Faça o **download** do **mysql-python** na opção **"MySQL-python-1.2.2.win32-py2.5.exe"**. Da mesma forma que as instalações anteriores, a **versão atual pode variar** para cima (1.2.3, 1.3 ou 2.0 por exemplo):

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/mysqldb-download/file/"/>
</p>

Após o download, clique duas vezes sobre o arquivo baixado para instalar o **MySQL for Python** no Windows e siga a instalação clicando em **"Avançar"** até finalizar.

### Instalando o mod\_python

Como já foi dito no capítulo anterior, o **mod\_python** ainda é a forma mais comum de instalar projetos em Django no Apache em servidores dedicados, mesmo que a opção mais recomendada seja o **WSGI**.

Neste capítulo, vamos trabalhar com o **mod\_python** e em um próximo capítulo será a vez do **WSGI**, isso porque é importante você conhecer os dois. Mas não se esqueça que é possível ter qualquer um deles (mod\_python, WSGI ou FastCGI) instalado em Windows ou Linux.

Para fazer o download do **mod\_python** para Windows, carregue a seguinte página em seu navegador:

    http://httpd.apache.org/modules/python-download.cgi
    

Localize o bloco na página que começa desta forma (lembrando que a versão pode variar):

    Apache Mod_python 3.3.1 is now available
    

E abaixo dele, clique sobre o link **"Win32 Binaries"**. Uma lista nada amigável será exibida. Você deve localizar dentre as opções, o **mod\_python** para a sua versão do Python ( **2.5** ) e a sua versão do Apache ( **2.2** ), que atualmente é esta:

    mod_python-3.3.1.win32-py2.5-Apache2.2.exe 
    

Após o final do download, clique duas vezes sobre o arquivo baixado e prossiga clicando em **"Avançar"** até esta janela ser exibida:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/mod_python-instalacao/file/"/>
</p>

Escolha o **caminho de instalação** do Apache, que é este:

    c:\Arquivos de programas\Apache Software Foundation\Apache2.2
    

Prossiga para finalizar a instalação.

Agora abra o seguinte arquivo para edição, usando o **Bloco de Notas**:

    C:\Arquivos de programas\Apache Software Foundation\Apache2.2\conf\httpd.conf
    

Localize uma linha que inicia com **"LoadModule"** e acrescente a seguinte linha abaixo dela:

    LoadModule python_module modules/mod_python.so
    

Salve o arquivo. Feche o arquivo.

Na verdade você vai encontrar muitas linhas assim, mas não se preocupe. Se acrescentar a nova linha abaixo de qualquer uma delas, estará no lugar correto.

E agora você tem o **mod\_python** instalado em seu servidor Windows.

### Configurando seu projeto no servidor

Bom, agora estamos caminhando para o final.

Crie na raiz da unidade **"C:"** do servidor a seguinte sequência de pastas:

    C:\var\django\projects\site_meu_blog
    

O caminho acima apenas segue a nossa proposta do capítulo 15, mas você pode trabalhar essa configuração como preferir.

Envie (usando seu meio escolhido) a pasta do projeto ( **"meu\_blog"** ) para dentro da pasta criada acima.

Agora na pasta do projeto ( **"C:\var\django\projects\site\_meu\_blog\meu\_blog"** ) abra o arquivo **"local\_settings.py"** para edição e o modifique para ficar assim:

    # Django settings for meu_blog project.
    import os
    PROJECT_ROOT_PATH = os.path.dirname(os.path.abspath(__file__))
    
    LOCAL = False
    DEBUG = False
    TEMPLATE_DEBUG = DEBUG
    
    DATABASE_ENGINE = 'mysql'
    DATABASE_NAME = 'meu_blog'
    DATABASE_HOST = 'localhost'
    DATABASE_USER = 'root'
    DATABASE_PASSWORD = '123456'

Isso vai fazer seu projeto trabalhar como se deve no servidor: sem **modo de depuração** ( DEBUG ) e acessando ao banco de dados **MySQL**. Lembre-se de informar a setting **DATABASE\_PASSWORD** com a senha correta que você informou ao **configurar** o MySQL.

### Criando o novo banco de dados no MySQL

Mas agora precisamos criar um novo banco de dados no MySQL para que seu projeto trabalhe. Como está escrito na setting **DATABASE\_NAME**, o banco de dados deverá ser chamado **"meu\_blog"**.

No menu **"Iniciar"** do Windows, escolha a opção **"Executar programa"** e digite o seguinte comando:

    mysql -u root -p
    

Uma janela do MS-DOS será aberta para informar a senha do usuário **"root"** no MySQL. Informe a senha ( a que você escolheu para o lugar de **"123456"** ), pressione ENTER, e o **shell** do MySQL será aberto:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/mysql-cliente/file/"/>
</p>

Agora digite a seguinte linha de comando:

    create database meu_blog;
    

Este será o resultado do comando:

    mysql> create database meu_blog;
    Query OK, 1 row affected (0.01 sec)
    
    mysql>

Feito isso, seu banco de dados está criado.

Feche a janela do MS-DOS e vá à pasta do projeto. Clique duas vezes sobre o arquivo **"gerar\_banco\_de\_dados.bat"** para gerar as tabelas para o novo banco de dados em MySQL. É agora que vamos saber se a nossa configuração está correta:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/mysql-geracao-do-banco/file/"/>
</p>

Informe os dados solicitados ( Username, E-mail, Password e Password again ) da mesma forma que fizemos no capítulo 4 ( **Criando um blog maneiro** ), com username **"admin"** e password **"1"**. Após finalizar a geração do banco de dados, feche a janela e...

... pronto! Mais um passo dado! Temos o projeto e o MySQL conversando naturalmente até aqui!

### Configurando o Apache para seu projeto

Agora, para ter seu projeto funcionando no Apache, abra novamente o arquivo **"httpd.conf"** da pasta **"C:\Arquivos de programas\Apache Software Foundation\Apache2.2\conf\"** para edição e acrescente as linhas abaixo ao final do arquivo:

    <Location "/">
        SetHandler python-program
        PythonPath "['c:/var/django/projects/site_meu_blog/meu_blog'] + sys.path"
        PythonHandler django.core.handlers.modpython
        SetEnv DJANGO_SETTINGS_MODULE settings
        PythonDebug On
    </Location>

O que fizemos ali? Bom, ao abrir uma tag **"Location"**, indicamos que o Apache deve seguir uma configuração especial para a sua URL raiz.

    <Location "/">
    

A linha seguinte determina que para esta URL deve ser habilitado o suporte ao Python.

        SetHandler python-program
    

Esta linha abaixo indica a **PYTHONPATH** que o Python deve obedecer na máquina virtual que será aberta para esta URL. No caso, atribuímos a pasta do projeto ( **'c:/var/django/projects/site\_meu\_blog/meu\_blog'** ), acrescentada do que já estava antes na PYTHONPATH ( **sys.path** ), seguindo a sintaxe do próprio Python para isso.

        PythonPath "['c:/var/django/projects/site_meu_blog/meu_blog'] + sys.path"
    

Na linha seguinte determinamos que o Apache deve utilizar o *handler* do Django para atender às requisições através do Python neste caso.

        PythonHandler django.core.handlers.modpython
    

E aqui declaramos uma **variável de ambiente** que determina o módulo que contém as **settings** do projeto. Observe que se indicamos a pasta do projeto à PYTHONPATH, então todos os módulos ( arquivos com extensão **".py"** ) que estão na pasta do projeto podem agora ser informados com seu único nome, pois eles estão na PYTHONPATH sem nenhum pacote intermediário. Na prática, estamos passando o arquivo **"settings.py"** para o Django .

        SetEnv DJANGO_SETTINGS_MODULE settings
    

Por fim, definimos o **estado de depuração** para a **máquina virtual** do Python. Como estamos ainda em processo de colocar o site no ar, é prudente manter essa configuração. Quando o site estiver em pleno funcionamento, é recomendável que esta linha seja removida.

        PythonDebug On
    

Salve o arquivo.

### Reiniciando o Apache

Agora, observe o **ícone do Apache na bandeja** do Windows, próximo ao relógio, clique sobre ele e escolha a opção **"Restart"** para reiniciar o Apache e aplicar a mudança feita.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/mod_python-reiniciando-apache/file/"/>
</p>

Vá ao navegador e carregue o endereço do servidor:

    http://localhost/
    

A seguinte página será carregada:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/apache-projeto-no-servidor-sem-imagens/file/"/>
</p>

Olha só! Temos o nosso site funcionando no servidor! Mas onde foi parar tudo aquilo que fizemos de **imagens e estilos CSS**?

Simples. Lembra-se que a URL **'^media/'** só é reconhecida pelo Django em ambiente de desenvolvimento? Pois bem, agora temos que configurar o Apache para reconhecê-la. Isto é uma tarefa que não tem nenhum intermédio do Django. É apenas um endereço do Apache que aponta para uma pasta no HD. Vamos lá?

Volte ao arquivo de configuração do Apache ( **"httpd.conf"** ) e acrescente o seguinte trecho de código ao final dele:

    Alias /media "C:/var/django/projects/site_meu_blog/meu_blog/media"
    
    <Directory "C:/var/django/projects/site_meu_blog/meu_blog/media">
        Order allow,deny
        Allow from all
    </Directory>
    
    <Location "/media">
        SetHandler None
    </Location>

O bloco acima faz com que o Apache entenda que ao carregar a URL **"/media"** ( e URLs "filhas" ), a pasta **"C:/var/django/projects/site\_meu\_blog/meu\_blog/media"** deve ser tomada como base, ou seja, a pasta de arquivos estáticos do nosso projeto.

A linha que faz esse trabalho é esta:

    Alias /media "C:/var/django/projects/site_meu_blog/meu_blog/media"
    

Mas isso não é suficiente, pois é preciso dar permissão à pasta indicada, portanto, este bloco abaixo dá a permissão necessária:

    <Directory "C:/var/django/projects/site_meu_blog/meu_blog/media">
        Order allow,deny
        Allow from all
    </Directory>

Por fim, este último bloco desabilita o Python para a URL **/media**, para que ela trabalhe somente com arquivos estáticos, sem qualquer interferência do Django.
    
    <Location "/media">
        SetHandler None
    </Location>

Salve o arquivo. **Reinicie** o Apache, usando o **ícone na bandeja** do Windows na opção **"Restart"** para aplicar a mudança que fizemos.

Voltando ao navegador, pressione **F5** e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/deploy-com-estilos-css/file/"/>
</p>

Satisfeito? Mas como pode notar, não temos nenhum **artigo** publicado, devido a ser outro banco de dados. Portanto, vamos ao **Admin** do site para criar os novos artigos! No navegador, carregue a seguinte URL:

    http://localhost/admin/
    

Veja como ele será carregado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/deploy-admin-sem-estilos-css/file/"/>
</p>

Eta! Mas nós não havíamos resolvido o problema dos **arquivos estáticos**?

Sim, mas a URL que resolvemos foi a **"/media"** - a URL de arquivos estáticos do projeto. Porém, agora se trata do **Admin**, que é um caso especial, e possui seu próprio caminho para arquivos estáticos, com prefixo **"/admin\_media"**, informado na setting **"ADMIN\_MEDIA\_PREFIX"**.

Portanto, volte ao arquivo de configuração do Apache ( **"httpd.conf"** ) e acrescente mais estas linhas ao final:

    Alias /admin_media "C:/python25/lib/site-packages/django/contrib/admin/media"
    
    <Directory "C:/python25/lib/site-packages/django/contrib/admin/media">
        Order allow,deny
        Allow from all
    </Directory>
    
    <Location "/admin_media">
        SetHandler None
    </Location>

O que fizemos aí foi exatamente o mesmo que fizemos para a URL **/media**, mas desta vez trabalhamos a URL **/admin\_media**, que é direcionada para a pasta de arquivos estáticos da _contrib_ **Admin**.

Salve o arquivo. Feche o arquivo. **Reinicie** o Apache.

Agora atualize o navegador e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/deploy-admin-com-estilos-css/file/"/>
</p>

Pronto! Servidor funcionando, podemos partir para o próximo desafio!

### E aí, o quê mais?

<img src="http://www.aprendendodjango.com/media/img/ilustracao_4.gif" align="right"/>

- O quê mais? Meu amigo, estamos no meio da história, e nem terminamos o assunto do _deploy_! Amanhã vem a última bolacha do pacote.

Cartola encabulou com a energia de Alatazan. Ele não parecia exausto depois da extensa aula de hoje. Pelo contrário, Alatazan queria mais, queria saber quais outras formas de _deploy_ existem, porque essa foi barbada. 

Nena, que queria ir embora, puxou a cadeira que estava ao lado, coçou a bochecha, deu uma olhada no relógio e tomou sua vez:

- Alatazan, amanhã vai ser a minha vez de passar com você... quero te mostrar um servidor no **Linux** e outras coisas diferentes, vamos mudar isso aqui um pouco que esse ambiente de janelinhas de não é a minha praia não... - Nisso ela olhou irônica para o Cartola, que brigava com seu Mac pra pegar alguns arquivos do iPod. A resposta veio de bate-pronto.

- Hey, quem gosta de janelinha aqui é o Alatazan, não sou eu não hein, gosto de coisa com mais estilo.

Alatazan mexeu sua peruca desalinhado, fez de conta que não ouviu e tratou de voltar ao assunto:

- Bom, então hoje instalamos um monte de coisas:

 - Apache 2.2
 - MySQL 5.1
 - MySQL for Python 1.2.2
 - mod\_python 3.3.1

- Olha, não se prenda às **versões** secundárias, apenas atenha-se a pegar a versão estável mais recente. Geralmente funciona melhor.

- E só completando, meu camarada... quando ela fala em versão "secundária", isso quer dizer que se aparecer um Apache 3.0 ou MySQL 6.0, é bom fazer uns testes antes, pois as versões primárias geralmente mudam muitas coisas...

- Ok ok... valeu, já peguei. Por fim..

 - Configuramos o acesso ao banco de dados no arquivo **"local\_settings.py"** do servidor;
 - Criamos o banco de dados no MySQL;
 - Geramos as tabelas do banco de dados pelo Django...

Nena tomou a palavra, já que ela tinha pressa e o Alatazan estava falando lentamente, como se estivesse saboreando um daqueles churrascos de **lagartos de Katara**...

- e configuramos o Apache, criando a **Location** para o projeto e os **Alias** pra URLs de arquivos estáticos! Aí foi só **reiniciar** o Apache e a mágica está feita! Vamos embora pessoal?

- Bacana, amanhã será a vez do Linux! Vai lá meu camarada, vou só pegar essa música aqui e já vou descendo...

Dito isso, Cartola voltou sua atenção para o computador e os dois saíram, se despedindo do **Cadú** e da priminha que insistia em arrancar seu nariz.

<span class="no_print">
**Próximo capítulo: [Preparando um servidor com Linux](/preparando-um-servidor-com-linux/)**
</span>

