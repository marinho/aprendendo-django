<img src="http://www.aprendendodjango.com/media/img/ilustracao_3.gif" align="left"/>

Folheando o livro de Xadrez que Nena lhe emprestou, Alatazan notou aquelas seções de informações do livro, como uma breve biografia do autor, na contracapa, e a página onde haviam seus agradecimentos. Na última página do livro havia também um texto promocional de outros livros do mesmo autor, sua fotografia e um cartão da loja onde o livro havia sido comprado.

É comum haver esse tipo de páginas na maior parte dos sites.

Isso é porquê você precisa dizer quem é, do que se trata e outros assuntos relacionados para dar solidez e demonstrar transparência ao seu usuário. Seu legado é importante.

## Como uma luva: as Páginas Planas

Para páginas de conteúdo simples assim, as **FlatPages** - ou traduzindo - as **"Páginas Planas"**, encaixam-se como luvas, pois não é necessário saber programar em Django para criá-las, basta que o programador tenham habilitado esse recurso e criado um template para isso e todo o resto pode ser feito pelo **Admin** - a interface de administração do Django.

Vamos logo colocar a mão na massa, pois **não é preciso muito esforço** para chegar ao final da lição de hoje.

Antes de tudo, inicie seu projeto clicando duas vezes no arquivo **executar.bat**.

Agora abra a pasta do projeto, e edite o arquivo **settings.py** para adicionar a aplicação **flatpages** ao projeto e seu middleware.

Faça assim: vá até o final do arquivo, na setting **INSTALLED\_APPS** e adicione a seguinte linha:

    'django.contrib.flatpages',
    

Com isso, o trecho de código dessa setting ficou assim:

    INSTALLED_APPS = (
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.sites',
        'django.contrib.admin',
        'django.contrib.syndication',
        'django.contrib.flatpages',
        
        'blog',
    )

Agora vamos adicionar **o middleware que faz todo o trabalho mágico** das FlatPages. Localize a setting **MIDDLEWARE\_CLASSES** ainda no arquivo **settings.py** e adicione a seguinte linha:

    'django.contrib.flatpages.middleware.FlatpageFallbackMiddleware',
    

Agora o trecho de código da setting **MIDDLEWARE\_CLASSES** ficou assim:

    MIDDLEWARE_CLASSES = (
        'django.middleware.common.CommonMiddleware',
        'django.contrib.sessions.middleware.SessionMiddleware',
        'django.contrib.auth.middleware.AuthenticationMiddleware',
        'django.contrib.flatpages.middleware.FlatpageFallbackMiddleware',
    )

Salve o arquivo. Feche o arquivo.

Agora vamos gerar o banco de dados para ver o efeito que isso causou em nosso projeto. Na pasta do projeto, clique duas vezes sobre o arquivo **gerar\_banco\_de\_dados.bat** que deve executar da seguinte forma:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/flatpages-gerando-banco-de-dados/file/"/>
</p>

Com o banco de dados gerado, podemos abrir o navegador na URL do admin:

    http://localhost:8000/admin/
    

E temos agora uma nova seção para **Páginas Planas**:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/flatpages-admin/file/"/>
</p>

Clique no link **"Adicionar"** à direita da palavra **"Páginas Planas"** e adicione uma nova URL da seguinte forma:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/flatpages-nova-url/file/"/>
</p>

... e ao rolar a **barra de rolagem**, ainda há este outro campo para informar:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/flatpages-nova-url-2/file/"/>
</p>

Ou seja, informe os seguintes valores para os respectivos campos:

* URL: **/sobre-mim/**
* Título: **Sobre mim**
* Conteúdo: **esteja livre para informar o texto como quiser, mas faça questão que esse texto tenha mais de uma linha**
* Sites: **localhost:8000**

Uma informação importante aqui: **não se esqueça** da barra ( / ) **no início e no final** do **campo URL**. Isso é importante para que o middleware consiga casar a URL com a FlatPage, ok?

Pois bem, agora clique sobre o botão **"Salvar"** e carregue no navegador o seguinte endereço:

    http://localhost:8000/sobre-mim/
    

E veja o resultado a seguir:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/flatpages-template-nao-encontrado/file/"/>
</p>

Opa! Temos um erro aqui!

Mas é simples: **para uma FlatPage funcionar, é necessário haver um template onde ela será "encaixada"**.

Portanto, vamos agora à pasta do projeto e abrir dali a pasta **"templates"**. Dentro da pasta **"templates"**, crie uma nova pasta, chamada **"flatpages"**. Dentro da nova pasta, crie o arquivo **"default.html"**. Abra-o para edição e escreva o seguinte código dentro:

    {% extends "base.html" %}
    
    {% block titulo %}{{ flatpage.title }} - {{ block.super }}{% endblock %}
    
    {% block h1 %}{{ flatpage.title }}{% endblock %}
    
    {% block conteudo %}
    {{ flatpage.content }}
    {% endblock %}

Salve o arquivo. Volte ao navegador e pressione a tecla **F5** para atualizar. Veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/flatpages-sem-saltos-de-linha/file/"/>
</p>

Difícil? Uma baba! Mas há uma coisa feia ali. Lembra-se de que escrevemos um texto com **duas linhas**? Sim, mas elas se transformaram em uma só... mas o que aconteceu?

A resposta é simples: o HTML gerado para textos com salto de linha ignora esses saltos de linha a menos que haja a tag HTML **&lt;br/&gt;** entre elas. Mas o Django é muito legal, então há uma solução mais amigável para isso.

Volte ao arquivo de template que acabamos de criar e localize a seguinte linha:

    {{ flatpage.content }}
    

Agora a modifique para ficar assim:

    {{ flatpage.content|linebreaks }}
    

Salve o arquivo. Feche o arquivo. Vá ao navegador, atualize a página e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/flatpages-template-com-linebreaks/file/"/>
</p>

Pronto! **FlatPages** são isso que você acabou de ver!

## Chega de mau humor, Alatazan, claro que queremos fazer contato com você!

- Tranquilo, tranquilo hein gente... - falou um Alatazan relaxado.

- Tranquilo? Isso é até sem graça de tão tranquilo - foi a resposta empolgada do Cartola

- É, bastou **adicionar a aplicação** ao projeto, **adicionar o middleware** e **criar o template**. Só isso!

- E aí é só ir até o **Admin** e criar quantas páginas quiser, usando HTML e nunca se esquecendo de que a URL da Flatpage **sempre inicia e termina com a barra** ( / ). - completou Nena.

- Agora tem uma coisa esquisita nesse seu blog, cara. Você deixou um rodapé escrito "Por favor, não faça contato..." não é assim que se faz. Porque você não cria uma **página de Contato**?

Alatazan olhou para ele como se fosse um daqueles índios pelados da baía de Guanabara vendo navios portuguêses pela primeira vez.

- É! É isso, vamos fazer uma página de contato amanhã, então!

Pois bem, então é isso pessoal!

**Próximo capítulo: [Permitindo contato do outro lado do Universo](http://www.aprendendodjango.com/permitindo-contato-do-outro-lado-do-universo/)**

