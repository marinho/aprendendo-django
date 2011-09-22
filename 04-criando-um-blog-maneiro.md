# DESCRICAO CURTA

<img src="http://www.aprendendodjango.com/media/img/ilustracao_3.gif" alt="" align="right"/>

Alatazan cria seu primeiro projeto, conhece a interface automática de administração do Django e cria sua aplicação de blog.

Nessa brincadeira, ele aprende a criar **templates**, gerar um banco de dados, executar o projeto, utilizar **generic views**... enfim, uma série das coisas mais comuns do framework.

Mas ao final de tudo, ele percebe que precisa **compreender tudo aquilo melhor**. Não adianta nada fazer as coisas passo-a-passo e ninguém explicar o porquê de cada uma delas...

Paciência, Alatazan! Uma coisa de cada vez, logo logo chegaremos lá!

# TEXTO

Era uma manhã fria, o vento de outono balançava as folhas das poucas árvores na rua onde Alatazan morava, e alguém deixou ali um envelope engraçado, **triangular**. Para Alatazan, o envelope não era engraçado, era **exatamente igual** aos envelopes mais usados em Katara.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_3.gif" alt="" align="left"/>

Ele abriu o envelope e tirou o objeto prateado reluzente, com cheirinho de novo. Uma das pontas do envelope estava ligeiramente amassada, mas não tinha problema, pois o papel eletrônico era chamado de papel exatamente por ser **flexível**.

Na imagem animada do papel, sua mãe esbravejava por sua falta de consideração. Desde sua chegada, Alatazan não havia enviado sequer um "alô", mas mal sabia sua mãe que por aqui não havia uma agência do sistema interestelar de correios.

Mas Katara tem um jeito de acessar a nossa Internet, e ele teve então a idéia de criar um blog. 

Um blog é o jeito mais simples de escrever sobre sua vida. Toda vez que dá vontade de escrever, o sujeito vai lá, **cria um título e escreve** o que quiser, e a página mostra sempre os últimos ítens que ele escreveu.

## Porquê um blog?

Um blog é fácil e simples de se fazer. E também simples de criar novidades.

Hoje vamos deixar o **Blog** funcionando, a seguir vamos esclarecer umas coisas, melhorar outras, colocar os **RSS**, depois vamos aos **Comentários**, e assim por diante. Cada pequeno passo de uma vez, e bem resolvido.

Mas antes de tudo, precisamos resolver uma questão ainda em aberto...

## Escolhendo um editor

O Django não se prende a um editor específico. Existem centenas deles, alguns com mais recursos, outros mais simples. É como modelos de tênis, cada um usa o seu, e há aqueles que preferem sapatos, sandálias, ou andar descalços mesmo. Há ainda aqueles não preferem, mas ainda assim o fazem.

O editor que você escolhe hoje, pode ser descartado amanhã, e o próximo também. Não se trata de um casamento, mas sim de uma escolha do momento, até que você encontra aquele que se encaixa com o seu perfil.

Então vamos começar pelo **Bloco de Notas**, e depois vamos conhecer outros.

## Ok, agora vamos criar o projeto

Antes de mais nada, vamos criar uma pasta para colocar nossos projetos: **c:\Projetos** (no Linux ou Mac: **/Projetos**).

E agora abra o **Prompt de comando dos MS-DOS** (no Linux ou Mac é o **Console** ou **Terminal**) e localize a pasta criada com:

    cd c:\Projetos
    

Depois disso, digite o comando que cria projetos:

    python c:\Python25\Scripts\django-admin.py startproject meu_blog
    

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/criando-um-projeto-windows/file/" alt=""/>
</p>

No Linux ou Mac é um pouquinho diferente:

    $ cd /Projetos
    $ django-admin.py startproject meu_blog

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/criando-um-projeto-linux/file/" alt=""/>
</p>

Pronto, agora feche essa janela e vamos voltar à pasta do projeto, em **c:\Projetos\meu_blog**

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/pasta-do-projeto-meu_blog/file/" alt=""/>
</p>

> **Nota:** no **Linux** e no **MacOSX** o equivalente aos arquivos **.BAT** são os arquivos **.sh**. A diferença é que eles não suportam o comando **"pause"** e devem iniciar com a seguinte linha:
> 
>     #!/bin/bash
>     
> 
> Você deve também definir a permissão de execução ao arquivo usando o comando **chmod +x** ou usando a janela de Propriedades do arquivo.

Agora, vamos ver o site funcionando (mesmo que sem nada - ou com quase nada - dentro). Na pasta do projeto, crie um arquivo chamado **"executar.bat"** e o edite com o **Bloco de Notas**. Dentro dele, escreva o seguinte código:

    python manage.py runserver
    pause

Salve o arquivo. Feche o arquivo. Execute o arquivo clicando duas vezes sobre ele.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/executando-runserver/file/" alt=""/>
</p>

Abra seu **navegador** - o **Mozilla Firefox**, por exemplo - e localize o endereço:

    http://localhost:8000/

E é isso que você vê:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/pela-primeira-vez-no-navegador/file/" alt=""/>
</p>

Mas isso ainda não é nada. Você ainda não possui nada em seu projeto. Ou pelo menos, nada que alguém possa ver.

### O arquivo settings do projeto

Agora na pasta do projeto, edite o arquivo **settings.py** com o **Bloco de Notas** e faça as seguintes modificações:

1. A linha que começa com **DATABASE\_ENGINE**, deve ficar com **DATABASE\_ENGINE = 'sqlite3'**
1. A linha que começa com **DATABASE\_NAME**, deve ficar com **DATABASE\_NAME = 'meu_blog.db'**
1. E logo abaixo da linha que possui **'django.contrib.sites',** acrescente outra linha com **'django.contrib.admin',**

### O arquivo de URLs do projeto

Salve o arquivo. Feche o arquivo. Agora edite o arquivo **urls.py**, também com o **Bloco de Notas**, fazendo as seguintes modificações:

1. Na linha que possui **# from django.contrib import admin**, remova o "# " do início - **não se esqueça de remover o espaço em branco**
1. Na linha que possui **# admin.autodiscover()**, remova o "# " do início
1. Na linha que possui **# (r'^admin/(.*)', admin.site.root),**, remova o "# " do início

Salve o arquivo. Feche o arquivo. Agora vamos gerar o banco de dados (isso mesmo!).

### Geração do banco de dados

Na pasta do projeto (meu_blog), crie um arquivo chamado **gerar\_banco\_de\_dados.bat** e escreva o seguinte código dentro:

    python manage.py syncdb
    pause

Salve o arquivo. Feche o arquivo. Execute o arquivo, clicando duas vezes sobre ele.

A geração do banco de dados cria as tabelas e outros elementos do banco de dados e já **cria também um usuário de administração do sistema**. Isso acontece porque naquele arquivo **settings.py** havia uma linha indicando isso (calma, logo você vai saber qual).

Sendo assim, ele pergunta pelo nome desse usuário (**username**), seu **e-mail** (se você digitar um e-mail inválido, ele não vai aceitar) e a **senha**.

Assim, para ficar mais fácil, informe os dados assim:

* Username: **admin**
* E-mail: <seu e-mail>
* Password: **1**
* Password (again): **1**

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/geranco-o-banco-de-dados/file/" alt=""/>
</p>

Ao exibir a frase **Pressione qualquer tecla para continuar. . .**, feche a janela volte ao navegador, **pressione F5** pra ver como vai ficou.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/no-navegador-pagina-nao-encontrada/file/" alt=""/>
</p>

Isso aconteceu porque agora você possui alguma coisa em seu projeto. E quando você possui *"alguma coisa"*, qualquer outra coisa que você não possui é considerada como **Página não encontrada**. Então, que *coisa* é essa?

### A interface de administração

Na barra de endereço do navegador, localize o endereço:

    http://localhost:8000/admin/

E a seguinte página será carregada, para autenticação do usuário.

Você vai informar o usuário e senha criados no momento da geração do banco de dados:

* Username: **admin**
* Password: **1**

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/admin-autenticacao/file/" alt=""/>
</p>

E após clicar em **Log in**, a seguinte página será carregada:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/admin-primeira-vez/file/" alt=""/>
</p>

## Criando a primeira aplicação

Agora, vamos criar uma aplicação, chamada **"blog"**, ela vai ser responsável pelas funcionalidades do blog no site. Portanto, crie uma pasta chamada **"blog"** e dentro dela crie os arquivos abaixo:

* \_\_init\_\_.py
* models.py
* admin.py

Abra o arquivo **models.py** com o **Bloco de Notas** e escreva o seguinte código dentro:

    from django.db import models
    
    class Artigo(models.Model):
        titulo = models.CharField(max_length=100)
        conteudo = models.TextField()
        publicacao = models.DateTimeField()

Salve o arquivo. Feche o arquivo.

Agora abra o arquivo **admin.py** com o **Bloco de Notas** e escreva dentro:

    from django.contrib import admin
    from models import Artigo
    admin.site.register(Artigo)

Salve o arquivo. Feche o arquivo.

Vamos voltar à pasta do projeto (**meu_blog**), e editar novamente o arquivo **settings.py**, fazendo o seguinte:

1. Abaixo da linha que possui **'django.contrib.admin',** acrescente outra linha com **'blog',**

Salve o arquivo. Feche o arquivo.

E agora volte ao navegador, pressionando a tecla **F5**:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/admin-com-aplicacao-blog/file/" alt=""/>
</p>

Opa! Agora vemos ali uma coisa nova: uma caixa da nova aplicação **"blog"**.

Mas como nem tudo são flores, ao clicar no link **Artigos**, será exibido um erro, de banco de dados.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/admin-erro-de-tabela-inexistente/file/" alt=""/>
</p>

Isso acontece porque você não gerou o banco novamente, depois de criar a nova aplicação. Então vamos fazer isso.

Clique duas vezes sobre o arquivo **gerar\_banco\_de\_dados.bat** e veja o resultado a seguir:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/geranco-o-banco-de-dados-com-nova-aplicacao/file/" alt=""/>
</p>

Agora, ao voltar ao navegador e atualizar a página com **F5**, a página será carregada da maneira desejada:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/admin-blog-artigos/file/" alt=""/>
</p>

Vamos inserir o primeiro artigo?

Clique sobre o link **"Add artigo"** e preencha as informações, clicando por fim no botão **"Save"** para salvar.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/admin-blog-novo-artigo/file/" alt=""/>
</p>

Bom, já temos a interface de administração do blog funcionando, agora vamos ao mais interessante!

## Saltando da administração para a apresentação

Bom, temos a interface de administração do blog funcionando, mas isso ainda não é suficiente, porque os visitantes do blog precisam ter uma visão **mais agradável** e limitada dessas informações, e no caso da mãe de Alatazan, multiplique isso por dois.

Pois então vamos agora fazer uso do poder das **generic views** e dos **templates** do Django para criar a página onde vamos apresentar os artigos que o Alatazan escrever.

Vamos então começar criando **URLs** para isso.

URLs são os endereços das páginas do site. Um exemplo é a **http://localhost:8000/admin/**, apontada para a interface de administração.

Ao tentar carregar a URL **http://localhost:8000/** em seu navegador, será exibida aquela página amarela com **Página não encontrada**, se lembra? Pois então agora vamos criar uma página para responder quando aquela URL for chamada.

Na pasta do projeto (**meu\_blog**), abra o arquivo **urls.py** para editar, e modifique para ficar da seguinte maneira:

    from django.conf.urls.defaults import *
    
    # Uncomment the next two lines to enable the admin:
    from django.contrib import admin
    admin.autodiscover()
    
    from blog.models import Artigo
    
    urlpatterns = patterns('',
        (r'^$', 'django.views.generic.date_based.archive_index',
         {'queryset': Artigo.objects.all(), 'date_field': 'publicacao'}),
        (r'^admin/(.*)', admin.site.root),
    )    

Salve o arquivo. Feche o arquivo. Volte ao navegador, e pressione **F5** para atualizar a página, e o resultado será como este abaixo:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/template-do-artigo-nao-existente/file/" alt=""/>
</p>

Isso acontece porque ainda falta um **arquivo em HTML** para exibir as informações do **Blog**, que chamamos de **Template**.

Vamos criá-lo?

Ok, abra a pasta **blog**, contida na pasta da projeto (onde já estão os arquivos **\_\_init\_\_.py**, **models.py** e **admin.py**), crie uma pasta chamada **"templates"** e dentro dela crie outra pasta, chamada **"blog"**. Por fim, dentro da nova pasta criada, crie novo arquivo chamado **"artigo_archive.html"** e escreva o seguinte código dentro:

    <html>
    <body>
    
    <h1>Meu blog</h1>
    
    {% for artigo in latest %}
    <h2>{{ artigo.titulo }}</h2>
    
    {{ artigo.conteudo }}
    {% endfor %}
    
    </body>
    </html>

Este é um HTML simples, que percorre a lista de **artigos** do **blog** e exibe seu **Título** e **Conteúdo**.

**Atenção:** neste momento, é importante fechar a janela do **MS-DOS** (**Console** ou **Terminal**, no Linux ou Mac) onde o Django está rodando, e executar novamente o arquivo **executar.bat**. Isto se faz necessário, pois é a primeira vez que adicionamos um template no site, o que obriga que todo o Django seja iniciado novamente do zero.

Veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/blog-pagina-inicial-com-artigos/file/" alt=""/>
</p>

Bom, então, é isso!

## Finalizando por hoje...

Aos poucos Alatazan vai percebendo que a cada novo passo que ele dá, as explicações detalhadas de Cartola e, especialmente Nena, vão se tornando desnecessárias, pois ele vai formando o conhecimento necessário para fazer aquelas tarefas mais comuns.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_4.gif" align="right"/>

No entanto, é muito claro pra ele que há muitas coisas não explicadas (ou pouco explicadas) ali. Foi aí que ele se virou para Nena e disse:

- Err... fascinante, mas, eu ainda estou sem entender a maior parte das coisas... como por exemplo aquele...

- Calma meu amigo, deixa de ser fominha, uma coisa de cada vez - interferiu o sorridente Cartola, batendo em seu ombro e deixando cair um dos fones do ouvido.

- Vamos respirar um pouco, amanhã vamos explicar o porquê de cada coisa, e você vai compreender cada um desses arquivos criados, editados, modificados, executados, enfim. Mas isso só depois de um bom suco! - Nena se empolgou com a vontade de Alatazan, que se empolgou com a empolgação de Nena. Cartola já era empolgado por natureza.

**Próximo capítulo: [Entendendo como o Django trabalha](http://www.aprendendodjango.com/entendendo-como-o-django-trabalha/)**

