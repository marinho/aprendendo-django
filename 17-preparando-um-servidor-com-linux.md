<img src="http://www.aprendendodjango.com/media/img/ilustracao_5.gif" align="left"/>

Naquela noite ao chegar à sua casa, Nena abriu uma nova **máquina virtual com o Ubuntu Linux** em seu laptop. Não queria usar sua instalação oficial cheia de coisas já instaladas.

Alatazan por sua vez aproveitou a passagem de uma _nave-galé_ de Katara pelo nosso sistema solar para dar uma passadinha por lá (atrás de Júpiter) e buscar algumas coisas que sua mãe mandou.

A definição mais comum da palavra "mãe" em nossa galáxia é de **"aquela que serve, protege, regula e manda pra você um monte de coisas inúteis que um dia com febre e assaduras você descobre que não eram tão inúteis assim"**.

Claro que a mãe de Alatazan não foge à definição, e nas caixas que mandou estão alguns pacotinhos com... por exemplo: uma caixinha de cuecas refrescantes, um spray do pó que refresca as cuecas por algum tempo, uma chave-polvo para desentalar o spray de vez em quando e uma alga-aérea para alimentar a chave-polvo.

Ao sair de Katara, Alatazan se sentiu aliviado por não ser mais obrigado a usar as tais cuecas a guardou as poucas que trouxe em algum lugar no fundo da nave. Mas agora, cansado de desenhar silhuetas desconfortáveis para o ventilador, ele estava com uma assadura na virilha, o que fazia com que andasse desajeitado, parecendo um **E.T.** e não sabia mais onde as cuecas estavam. Por isso, os pacotes de sua mãe foram bastante tranquilizadores.

## Um servidor 100% livre

No capítulo anterior, trabalhamos com um conjunto bastante comum, com as ferramentas mais populares dentre as que estamos trabalhando: **Windows + MySQL + Apache + mod\_python**.

Mas agora o nosso foco é trabalhar com alternativas.

Não que Windows trabalhe melhor com MySQL ou Linux trabalhe melhor com PostgreSQL, não é bem por aí... o bom é conhecer sistemas operacionais diferentes, bancos de dados diferentes, servidores web diferentes e assim por diante.

Para colocar em prática o nosso servidor em Linux, não se esqueça de que é preciso ter o **controle total do sistema**, do contrário não será possível instalar os softwares que vamos instalar nem mesmo configurá-los. Portanto, a recomendação é que agora você use sua instalação do Ubuntu em **máquina local** ou em uma **máquina virtual**, como Alatazan o fez.

Neste capítulo vamos trabalhar no universo **Ubuntu/Debian**, que estão entre as distribuições mais populares do Linux.

Para fazer o download do Ubuntu, vá até esta página:

    http://www.ubuntu.com/
    

E para instalá-lo em uma máquina virtual, faça sua escolha favorita. Uma boa opção é o **VirtualBox**:

    http://www.virtualbox.org/
    

Ambos são **livres e gratuitos**, aliás uma coisa bem comum entre todas as ferramentas que vamos tratar aqui.

O Ubuntu Linux possui uma interface gráfica extremamente fácil e amigável, entretanto, como muitos servidores Linux não possuem ambiente gráfico, vamos trabalhar constantemente em **Console** - modo texto semelhante ao **Prompt do MS-DOS**.

Neste capítulo, assumimos que você possui conhecimentos básicos em Linux suficientes para abrir o Console como pode ver abaixo e ter um editor de sua preferência, como gVim, vim, gedit, emacs, nano ou outro qualquer.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/ubuntu-console-no-menu/file/"/>
</p>

### Instalando o Lighttpd

O Lighttpd é um dos servidores web mais leves e rápidos que existem, pois tem uma estrutura mais simples que o Apache e outros.

Para instalar o Lighttpd, abra uma janela do **Console** e digite o seguinte comando:

    sudo apt-get install lighttpd
    

O comando **"sudo"** solicita sua própria senha para efeitos de segurança, mas ele armazena essa senha em memória por algum tempo, portanto você precisa digitar sua senha novamente somente se ficar muito tempo em espera.

Confirme a instalação dos pacotes e espere pela conclusão da instalação.

Ao concluir, vá até seu navegador, e assumindo que esteja trabalhando em máquina local, carregue a seguinte URL:

    http://localhost/
    

Caso esteja trabalhando em uma máquina remota, digite a URL para o caso.

De qualquer forma, veja como aparece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/linux-lighttpd-rodando/file/"/>
</p>

Sim, é só isso mesmo para instalar o **Lighttpd**.

### Instalando o PostgreSQL

Vamos agora instalar o PostgreSQL, um dos bancos de dados livres mais robustos e populares.

No **Console**, digite a seguinte linha de comando:

    sudo apt-get install postgresql-8.3
    

Confirme a instalação dos pacotes e aguarde pela conclusão. Assim como aconteceu com o Lighttpd, ao concluir a instalação, o PostgreSQL já é iniciado.

Agora, precisamos definir uma senha para o usuário **"postgres"** para ter acesso ao banco de dados. Portanto, digite agora as seguintes linhas de comando:

    sudo su postgres
    psql

Será aberto um novo *shell* da seguinte forma:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/linux-psql/file/"/>
</p>

Este é o *shell* cliente do PostreSQL, onde é possível realizar qualquer operação SQL de **definição de modelo de dados, consulta a dados, persistência e tarefas de manutenção**, enfim, muito poderoso.

Para alterar a senha do usuário **"postgres"** do PostgreSQL, digite a seguinte linha de comando:

    alter user postgres with password '123456';
    

Não se esqueça de trocar a senha **'123456'** pela de sua preferência. O resultado na linha abaixo será este:

    ALTER ROLE
    

Pronto. Agora digite o comando abaixo para sair do shell:

    \q
    

E para sair da sessão do usuário **"postgres"** digite:

    exit
    

Observe que estamos trabalhando aqui com dois usuários **"postgres"** diferentes: ora trabalhamos no usuário "postgres" **do Linux**, ora com o usuário de mesmo nome **no PostgreSQL**. Um possui vínculo indireto ao outro, mas se tratam de coisas diferentes. Ao modificar a senha do usuário **no PostgreSQL**, a senha do usuário **no Linux** permanece como estava. Mas isso é irrelevante agora, pois esse usuário só será usado para acesso a bancos de dados.

Agora, de volta à sessão do seu usuário, digite a seguinte linha comando:

    psql -U postgres -W
    

Ao solicitar a senha, digite a senha informada no lugar de **'123456'** criada agora há pouco. Veja a mensagem de erro que será exibida:

    psql: FATAL: autenticação do tipo Ident falhou para o usuário "postgres"
    

Veja bem: quando acessamos o **psql** usando uma sessão do usuário **"postgres"** do Linux, a autenticação foi aceita sem nem mesmo requisitar senha, mas agora que usamos uma sessão do seu usuário, mesmo informando a senha correta a autenticação foi rejeitada. Porquê?

Porque existe uma configuração que determina isso. Vamos mudar essa configuração. Digite o seguinte comando:

    sudo nano /etc/postgresql/8.3/main/pg_hba.conf
    

Você pode usar outro editor ao invés do **nano**, como o **vim** ou **gedit** por exemplo.

Localize agora as seguintes linhas, já próximo ao fim do arquivo:

    # Database administrative login by UNIX sockets
    local   all         postgres                          ident sameuser

O que esta linha faz e determinar que o usuario **"postgres"** do PostgreSQL so pode ser autenticado caso esteja acessando do servidor **local**, com o usuario equivalente do sistema operacional (ou seja, o usuario **"postgres"** do Linux). Vamos portanto liberar a autenticacao deste usuario por outros (como o seu proprio usuario por exemplo). Para isso, modifique as linhas para ficar assim:

    # Database administrative login by UNIX sockets
    local   all         postgres                          password

Salve o arquivo. Feche o arquivo.

Agora de volta ao **shell**, reinicie o serviço do PostgreSQL com a seguinte linha de comando:

    sudo /etc/init.d/postgresql-8.3 restart
    

Agora ao tentar novamente o seguinte comando, você terá sucesso:

    psql -U postgres -W
    

Veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/linux-psql-autenticacao/file/"/>
</p>

Pronto! Agora temos o PostgreSQL funcionando em nosso servidor, da forma apropriada ao que precisamos.

### Python e Django

O **Ubuntu Linux** tem o **Python** como linguagem principal, portanto, você não precisa se preocupar em instalá-lo, pois ele já vem instalado. O mesmo vale para o **Debian**.

Para instalar o **Django** na última versão do Ubuntu Linux - **Intrepid Ibex**, versão **8.10** - basta digitar a seguinte linha de comando:








    sudo apt-get install python-django
    

Após confirmar e concluir a instalação, seu servidor já terá o Django na versão 1.0 - no mínimo - devidamente instalada.

Já nas demais versões, isso não é recomendável, pois a maioria delas ainda tem esse pacote em uma versão antiga do Django ( **0.96** ). Portanto neste caso, para instalar o Django, faça assim:

Primeiro instale o o **Subversion**, com a seguinte linha de comando:

    sudo apt-get install subversion
    

Depois de confirmar e concluir a instalação do **Subversion**, faça o download do Django 1.0.2:

    svn co http://code.djangoproject.com/svn/django/tags/releases/1.0.2/ django-1.0.2
    

Isso vai baixar o **código-fonte** da última revisão da versão 1.0.2 do Django, usando o **Subversion** - um software de controle de versões.

Após concluir o download, entre na nova pasta **"django-1.0.2"** e instale o Django:

    cd django-1.0.2
    sudo python setup.py install

Após concluir a instalação do Django, para confirmar que está tudo ok, digite as seguintes linhas de comando para voltar à sua pasta **HOME** e acessar o shell interativo do **Python**:

    cd ~
    python

E agora, fora da pasta de instalação do Django e dentro do **shell interativo**, digite estas linhas de comando:

    import django
    django.get_version()

O resultado deverá ser este (ou outro muito semelhante a este, mas **sem mensagem de erro**):

    '1.0.2 final'
    

Agora saia do **shell interativo**, pressionando **Control+D**.

Pronto! Agora temos o **Django** numa versão bacana funcionando em nosso servidor!

### Biblioteca de acesso ao PostgreSQL

A biblioteca de acesso ao PostgreSQL para o Python se chama **"psycopg2"**. Para instalar o **"psycopg2"** no Ubuntu, basta digitar a seguinte linha de comando no **shell** do Linux:

    sudo apt-get install python-psycopg2
    

Pronto! Após confirmar e concluir a instalação, já temos a biblioteca de acesso ao PostgreSQL funcionando no Python em nosso servidor!

### Instalando o flup para trabalhar com FastCGI no Python

Agora, como exploramos o **mod\_python** no capítulo anterior, desta vez vamos trabalhar com o **FastCGI** no Lighttpd. Então, será necessário instalar o **flup**, a biblioteca do Python responsável pela compatibilidade com o **FastCGI**.

Para isso, digite a seguinte linha de comando:

    sudo apt-get install python-flup
    

Agora o seu Python está preparado para trabalhar com FastCGI.

### Configurando seu projeto no servidor

Da mesma forma que no Windows, algumas coisas precisam ser feitas no projeto para que ele trabalhe adequadamente no servidor. Algumas dessas coisas são semelhantes, outras não...

Para começar, crie a nossa pasta de coisas em Django no servidor, assim:

    sudo mkdir /var/django
    sudo chown SEU_USUARIO /var/django
    sudo chgrp SEU_USUARIO /var/django
    cd /var/django

Lembre-se de trocar o **"SEU\_USUARIO"** nas duas linhas de comando pelo nome do seu usuário atual.

As linhas de comando vão:

1. Criar a pasta onde vamos colocar nossas coisas em Django, usando o superusuário do sistema operacional;
1. Assumir o seu próprio usuário como dono da nova pasta;
1. Assumir o seu próprio grupo como dono dela;
1. Acessar a pasta.

Agora cria a sequência de pastas:

    mkdir projects
    cd projects
    mkdir site_meu_blog

Feito isso, envie a pasta do seu projeto ( **"meu\_blog"** ) para esta pasta do servidor, em caminho completo:

    /var/django/projects/site_meu_blog
    

Para isso, utilize uma das formas de envio de arquivos abordadas no **"capítulo 15: Infinitas formas de se fazer deploy"** (software de controle de versões, FTP ou Fabric).

Agora dentro da pasta do projeto no servidor ( **"/var/django/projects/site\_meu\_blog/meu\_blog/"** ), abra o arquivo **"local\_settings.py"** para edição:

    cd meu_blog
    nano local_settings.py

Modifique o arquivo, para ficar desta maneira:

    # Django settings for meu_blog project.
    import os
    PROJECT_ROOT_PATH = os.path.dirname(os.path.abspath(__file__))
    
    LOCAL = False
    DEBUG = False
    TEMPLATE_DEBUG = DEBUG
    
    DATABASE_ENGINE = 'postgresql_psycopg2'
    DATABASE_NAME = 'meu_blog'
    DATABASE_HOST = 'localhost'
    DATABASE_USER = 'postgres'
    DATABASE_PASSWORD = '123456'
    
    FORCE_SCRIPT_NAME = ''

Da mesma forma que no capítulo anterior, **desabilitamos o modo de depuração**, mas desta vez preparamos o projeto para trabalhar com o **PostgreSQL**, ao invés do MySQL. Não vá se esquecer de ajustar a setting **"DATABASE\_PASSWORD"** com a senha que escolheu para o lugar de **"123456"**.

O principal ponto a ser observado nessa modificação trata-se desta linha:

    FORCE_SCRIPT_NAME = ''
    

Ela terá o papel de **ocultar** o nome interno do *script* do FastCGI, e caso não seja informada, o site muito provavelmente se comportará de forma instável em suas URLs.

Salve o arquivo. Feche o arquivo.

### Criando o novo banco de dados no PostgreSQL

E como definimos que o nome do banco de dados do projeto deve ser **"meu\_blog"**, então devemos criá-lo, certo? Pois é o que vamos fazer agora.

No **shell**, digite a seguinte linha de comando para acessar novamente o shell do PostgreSQL:

    psql -U postgres -W
    

Informe a senha correta e digite a seguinte expressão SQL:

    create database meu_blog encoding 'UTF-8';
    

Essa linha de comando vai **criar um novo banco de dados** e garantir que sua codificação de caracteres é a **'UTF-8'**, a mais compatível com o Django, por ele ser **Unicode**.

O resultado desse comando deve ser simplesmente:

    CREATE DATABASE
    

Pronto. Banco de dados criado! Digite esta outra linha de comando para sair do shell do PostgreSQL:

    \q
    

Agora, precisamos gerar as tabelas do banco de dados, lembra-se?

Só para nos certificar, digite a linha de comando abaixo para se posicionar na pasta do projeto:

    cd /var/django/projects/site_meu_blog/meu_blog
    

Agora, digite esta outra linha de comando:

    python manage.py syncdb
    

Depois de dadas as informações solicitadas (Username, E-mail, Password e Password again), o resultado deve ser este:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/linux-geracao-do-banco/file/"/>
</p>

Não se preocupe em comparar tudo. Se não houve mensagem de erro é porque deu certo.

Bacana! Estamos caminhando bem! Nosso banco de dados foi criado e agora já possui as tabelas e outras entidades devidamente geradas pelo Django!

### Configurando o Lighttpd para seu projeto

Agora precisamos trabalhar a configuração do Lighttpd para reconhecer o seu projeto, certo? Então vamos lá!

Digite as seguintes linhas de comando:

    cd /etc/lighttpd/conf-available
    ls

Veja o resultado:

    05-auth.conf     10-proxy.conf         10-ssi.conf     10-userdir.conf
    10-cgi.conf      10-rrdtool.conf       10-ssl.conf     README
    10-fastcgi.conf  10-simple-vhost.conf  10-status.conf

Esses arquivos são pré-configurados para determinadas utilidades. É uma dessas coisas feitas para ajudar quem está configurando o servidor. Mas nós vamos criar um novo, completamente vazio. Abra um novo arquivo **"10-meu\_blog.conf"** para edição, assim:

    sudo nano 10-meu_blog.conf
    

Escreva estas linhas de código:

    server.modules += ( "mod_fastcgi" )
    server.modules += ( "mod_rewrite" )
    
    fastcgi.server = (
        "/default.fcgi" => (
            "main" => (
                "socket" => "/tmp/lighttpd-default.sock",
                "check-local" => "disable",
                "bin-path" => "/var/django/projects/site_meu_blog/meu_blog/deploy/default.fcgi",
            )
        )
    )
    
    url.rewrite-once = (
        "^(/.*)$" => "/default.fcgi$1"
    )

Resumindo, o que fizemos foi:

Carregar os modulos de FastCGI e Rewriting do Lighttpd:

    server.modules += ( "mod_fastcgi" )
    server.modules += ( "mod_rewrite" )

Determinar um caminho interno do servidor apontando para um script do projeto (alem de determinar o caminho para o arquivo de socket):

    fastcgi.server = (
        "/default.fcgi" => (
            "main" => (
                "socket" => "/tmp/lighttpd-default.sock",
                "check-local" => "disable",
                "bin-path" => "/var/django/projects/site_meu_blog/meu_blog/deploy/default.fcgi",
            )
        )
    )

E por ultimo, definimos uma URL (em expressao regular que quer dizer: **"todas a partir da barra"**) para apontar para esse script:

    url.rewrite-once = (
        "^(/.*)$" => "/default.fcgi$1"
    )

Salve o arquivo. Feche o arquivo.

Agora precisamos ativar essa configuração. Para isso, vamos criar um **link simbólico** para representar o arquivo **"10-meu\_blog.conf"** na pasta que define as configurações ativadas.

    sudo ln -s /etc/lighttpd/conf-available/10-meu_blog.conf ../conf-enabled/
    

Feito.

Acontece, que o script indicado não existe:

    /var/django/projects/site_meu_blog/meu_blog/deploy/default.fcgi
    

Portanto, devemos criá-lo:

    cd /var/django/projects/site_meu_blog/meu_blog/
    mkdir deploy
    cd deploy
    nano default.fcgi

Lembre-se de trocar o **"nano"** pelo seu editor preferido, caso ele não for o **nano**.

Agora dentro do novo arquivo, digite as seguintes linhas de código:

    #!/usr/bin/python
    import sys, os
    
    sys.path.insert(0, '/var/django/projects/site_meu_blog/meu_blog/')
    sys.path.insert(0, '/var/django/projects/site_meu_blog/dependencies/')
    sys.path.insert(0, '/var/django/pluggables/')
    
    os.chdir('/var/django/projects/site_meu_blog/meu_blog/')
    os.environ['DJANGO_SETTINGS_MODULE'] = 'settings'
    
    from django.core.servers.fastcgi import runfastcgi
    runfastcgi(method='threaded', daemonize='false')

Resumindo, o que fizemos nesse outro arquivo de _script_ foi isso:

Possibilitamos que este arquivo seja executável, e dizemos ao Linux que ele deve ser interpretado pelo Python:

    #!/usr/bin/python
    

Importamos os pacotes de **sistema** do Python e do **sistema operacional**:

    import sys, os
    

Acrescentamos as pastas do projeto à PYTHONPATH:

    sys.path.insert(0, '/var/django/projects/site_meu_blog/meu_blog/')
    sys.path.insert(0, '/var/django/projects/site_meu_blog/dependencies/')
    sys.path.insert(0, '/var/django/pluggables/')

Agora um pequeno _trick_ necessário e a definição da variável de ambiente **"DJANGO\_SETTINGS\_MODULE"** apontando o módulo que contém as **settings** do projeto, que por sua vez será localizada na PYTHONPATH que acabamos de definir:

    os.chdir('/var/django/projects/site_meu_blog/meu_blog/')
    os.environ['DJANGO_SETTINGS_MODULE'] = 'settings'

Por fim, executa a função para **"escutar" o FastCGI**:

    from django.core.servers.fastcgi import runfastcgi
    runfastcgi(method='threaded', daemonize='false')

Entendido? Salve o arquivo. Feche o arquivo.

Agora vamos transformar esse arquivo que criamos em executável:

    chmod a+x default.fcgi
    

Feito isso, vamos **reiniciar** o Lighttpd para ver o efeito do que fizemos:

    sudo /etc/init.d/lighttpd restart
    

O resultado das linhas de comando deve ser este:

     * Stopping web server lighttpd                [ OK ]
     * Starting web server lighttpd                [ OK ]

Agora va ao navegador e carregue o endereço de seu site:

    http://localhost/
    

Veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/linux-navegador-funcionando-sem-estilos/file/"/>
</p>

Ótimo! Mas como deve ter percebido, nosso site está **tosco**. Sem imagens, sem CSS. Da mesma forma como aconteceu no servidor Windows, isso é porque precisamos agora definir as URLs para arquivos estáticos **"/media"** e **"/admin\_media"**.

Para fazer isso, edite novamente o arquivo de configurações do Lighttpd. Assim:

    sudo nano /etc/lighttpd/conf-enabled/10-meu_blog.conf
    

E agora com o arquivo aberto para edição (usando seu editor predileto), modifique o arquivo para ficar assim:

    server.modules += ( "mod_fastcgi" )
    server.modules += ( "mod_rewrite" )
    
    fastcgi.server = (
        "/default.fcgi" => (
            "main" => (
                "socket" => "/tmp/lighttpd-default.sock",
                "check-local" => "disable",
                "bin-path" => "/var/django/projects/site_meu_blog/meu_blog/deploy/default.fcgi",
            )
        )
    )
    
    alias.url = (
        "/media" => "/var/django/projects/site_meu_blog/meu_blog/media",
        "/admin_media" => "/usr/lib/python2.5/site-packages/django/contrib/admin/media/"
    )
    
    url.rewrite-once = (
        "^(/media/.*)$" => "$1",
        "^(/admin_media/.*)$" => "$1",
        "^(/.*)$" => "/default.fcgi$1"
    )

Observe que fizemos as seguintes modificações:

Primeiro, acrescentamos dois **"apelidos"**, dizendo ao Lighttpd que tais URLS (**"/media"** e **"/admin\_media"**) devem apontar para as respectivas pastas no HD. Ou seja: são URLs simples para pastas de arquivos estáticos e devem fazer exatamente isso: retornar arquivos estáticos do HD no caminho definido.

Observe com atenção a pasta **"/usr/lib/python2.5/site-packages/django/contrib/admin/media/"** pois ela pode ser diferente em seu sistema operacional se por acaso ele usar a versão **2,4** do Python como padrão.

    alias.url = (
        "/media" => "/var/django/projects/site_meu_blog/meu_blog/media",
        "/admin_media" => "/usr/lib/python2.5/site-packages/django/contrib/admin/media/"
    )

As demais linhas que acrescentamos criam URLs com suas respectivas expressões regulares para os "apelidos" de arquivos estáticos, veja:

        "^(/media/.*)$" => "$1",
        "^(/admin_media/.*)$" => "$1",

Salve o arquivo. Feche o arquivo.

Reinicie o Lighttpd novamente:

    sudo /etc/init.d/lighttpd restart
    

Volte ao navegador e... bingo!

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/linux-tudo-ok/file/"/>
</p>

Missão cumprida!

## Vamos para a próxima etapa?

- Definitivamente! Isso me lembra um velho ditado em Katara: **fazeu, tá fazido**!

Cartola trocou olhares com Nena, deu uma risadinha contida com o canto da boca... mas não quis corrigir o alienígena e quebrar sua empolgação. Mas foi Nena que continuou a conversa:

- Sim, Alatazan... veja:

 - Você instalou o Lighttpd;
 - e instalou também o PostgreSQL e **definiu uma senha** de acesso ao banco de dados;
 - depois você instalou o Django... em nosso caso usando o **apt-get** também;
 - instalou o **psycopg2** para PostgreSQL no Python;
 - instalou o **flup** para FastCGI no Python;
 - **enviou os arquivos** do projeto para a pasta correta no servidor;
 - configurou o **"local\_settings.py"** para tirar o modo de depuração e configurar o acesso ao **banco de dados**;

Alatazan deu uma pescada e um arranque com a cabeça, se refez e abriu bem os olhos. Nena continuava:

 - ... criou o banco de dados;
 - **gerou as tabelas** no banco de dados;
 - configurou o Lighttpd para acesso ao projeto;
 - criou um **script de FastCGI** no seu projeto;
 - e configurou as URLs para **arquivos estáticos**!

Depois dessa lista, emendou mais:

- Olha, vocô notou uma coisa interessante? Para quase tudo, usamos o **apt-get** para instalar as coisas. Ele faz isso: baixa o que precisamos, todas as suas dependências, configura tudo, move os arquivos nas pastas certas, faz tudo direitinho...

Estava claro que hoje Nena estava com mais tempo. Com bem mais tempo. Com muito tempo.

- Ahh, já sei! Vamos ver **WSGI e servidores compartilhados**?

E muito empolgadinha também.

- Err... vamos fazer isso amanhã... tenho minha aula de xadrez...

- Tudo bem rapaziada... mas enquanto vamos saindo, vou pedir uma coisa importante pra você fazer: volte ao assunto do servidor no Windows e **compare** com o servidor no Linux. Vai perceber que existem mais semelhanças do que parece! 

<span class="no_print">
**Próximo capítulo: [WSGI e Servidores compartilhados](/wsgi-e-servidores-compartilhados/)**
</span>

