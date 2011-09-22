Cartola é obcecado por informação: lê notícias de política, esportes e economia, sites com artigos sobre robótica, receitas de comida senegaleza e blogs de nerds de renome.

Nena não deixa por menos, mas ela gostava mesmo é de entretenimento. Acessa blogs de celebridades, ao mesmo tempo que curte cada novidade sobre metodologias ágeis.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_4.gif" align="right"/>

Alatazan percebeu que seria complicado concorrer o precioso tempo de seus leitores com essa gente toda, então logo ele notou que precisa mudar seu blog para publicar seus textos em formato **RSS**.

RSS não se trata da sociedade conservadora mais risonha de Katara. RSS é somente um formato que segue os padrões do XML, mas que foi criado para carregar informações como **Título**, **Data de Publicação** e **Conteúdo** (dentre outras) de artigos. Qualquer site que lance esse tipo de informação, pode publicar seu conteúdo também em forma de RSS e assim seus leitores usam programas como o **Google Reader** para serem avisados sempre que houver novidades.

RSS também é muito útil para **Podcasts** e ainda para **exportar e importar** esse mesmo tipo de informação.

E claro, o Django já vem com isso pronto, sem dor e com bons recursos!

## Então vamos lá?

Para trabalhar com RSS no Django, vamos antes ajustar algumas coisas que serão necessárias daqui em diante. E o melhor momento para fazer esses ajustes, é agora.

### Ajustando settings fundamentais do projeto

No Django, um **site** é fruto de um **projeto**, que é composto de algumas (ou muitas) **aplicações**.

Uma aplicação é um conjunto - de preferência não extenso - de funcionalidades com foco em resolver uma questão específica. Um **Blog** por exemplo, é uma aplicação.

Todo projeto possui um arquivo chamado **settings.py**, onde são feitas configurações que determinam a sua essência.

Uma coisa importante para ajustar nas settings do projeto, é a **TIME\_ZONE**. É nesta setting que determinamos em que fuso horário o projeto deve estar adequado. Há uma lista desses fusos horários no seguinte site:

    http://en.wikipedia.org/wiki/List_of_tz_zones_by_name
    

Na pasta do projeto (**meu_blog**), abra o arquivo **settings.py** no editor e localize a seguinte linha:

    TIME_ZONE = 'America/Chicago'
    

Agora modifique para ficar assim:

    TIME_ZONE = 'America/Sao_Paulo'
    

Este é apenas um exemplo. Caso não esteja na zona de fuso horário da cidade de São Paulo, vá até o site citado acima, encontre o seu fuso horário, e ajuste a **TIME\_ZONE** para código respectivo.

Salve o arquivo. Feche o arquivo.

### Determinando a ordenação dos artigos do Blog

Agora, vamos à pasta da aplicação **blog**. Nesta pasta, abra o arquivo **models.py** com o editor e localize a seguinte linha:

    class Artigo(models.Model):
    

Modifique esse trecho do código de forma que fique assim:

    class Artigo(models.Model):
        class Meta:
            ordering = ('-publicacao',)

Essa modificação faz com que a listagem dos artigos seja sempre feita pelo campo **"publicacao"**. Ali há um detalhe importante: o **sinal de menos** ( - ) à esquerda de 'publicacao' indica que a **ordem deve ser decrescente**.

Localize esta outra linha:

    publicacao = models.DateTimeField()
    

E modifique para ficar assim:

    publicacao = models.DateTimeField(default=datetime.now, blank=True)

Isso faz com que não seja mais necessário informar o valor para o campo de publicação, pois ele assume a **data e hora atuais** automaticamente. O argumento **blank** indica que o campo **pode ser deixado em branco**, já que ele vai assumir a data e hora atuais nesse caso.

Porém, isso não basta.

O elemento **datetime** não está embutido automaticamente no seu arquivo, e você deve trazê-lo de onde ele está, para que o Python o use da forma adequada. Do contrário, o Python irá levantar um erro, indicando que o objeto **datetime** não existe.

Portanto, acrescente a linha a seguir no início do arquivo, na primeira linha:

    from datetime import datetime
    

Dessa forma, o arquivo **models.py** fica assim:

    from datetime import datetime
    from django.db import models
    
    class Artigo(models.Model):
        class Meta:
            ordering = ('-publicacao',)
    
        titulo = models.CharField(max_length=100)
        conteudo = models.TextField()
        publicacao = models.DateTimeField(default=datetime.now, blank=True)

Algumas coisas sobre Python serão esclarecidas daqui a alguns capítulos. Mas por hora, é importante ressaltar que você deve **sempre respeitar a identação**. O padrão mais indicado no Python é que seus blocos sejam sempre identados a cada **4 espaços**, portanto, muito cuidado com a tecla de tabulação. Não é a mesma coisa.

Muito bem. Feitos esses ajustes, vamos ao que realmente interessa.

## Aplicando RSS dos artigos do blog

A primeira coisa a compreender aqui, é que o Django é feito por pequenas partes. E a maior parte dessas pequenas partes não são ativadas quando você cria um projeto.

Esse é um cuidado essencial para que seu projeto fique **o mais leve e compacto possível**, carregando para a memória e processando somente aquilo que é necessário. E claro, quem sabe o que é necessário, **é você**.

As aplicações especiais que já vêm com o Django, são chamadas de **contribs**, e aquela que faz o trabalho pesado do RSS é chamada de **syndication**.

Vamos então editar novamente o arquivo **settings.py** da pasta do projeto, para adicionar a aplicação **syndication** às aplicações do projeto.

Localize a linha abaixo:

    INSTALLED_APPS = (
    

No Python, toda expressão que é divida entre sinais de vírgula ( , ) e contido entre parênteses, é chamada de **Tupla**. Trata-se de uma lista que não será modificada enquanto o projeto estiver em execução.

**INSTALLED\_APPS** é uma tupla, onde indicamos todas as aplicações do projeto. Todos os itens contidos ali terão um tratamento especial no seu projeto, indicando **classes de modelo de dados** (tabelas para o banco de dados), **views** e outras informações contidas em seus arquivos **models.py**, **views.py**, **admin.py**, dentre outros.

Vamos portanto, adicionar ao final da tupla a aplicação **'django.contrib.syndication'**, a nossa estrela principal aqui. Portanto, agora a tupla vai ficar assim:

    INSTALLED_APPS = (
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.sites',
        'django.contrib.admin',
        'django.contrib.syndication',
        
        'blog',
    )

Não é necessário que a ordem seja exatamente esta, mas é recomendável que se separe as aplicações **contribs** das demais aplicações, sempre que possível.

Salve o arquivo. Feche o arquivo. Agora vamos adicionar uma **URL** para este nosso **RSS**.

Localize o arquivo **urls.py** da pasta do projeto e abra-o para edição.

Neste arquivo, cada **tupla** informada dentro da função **patterns()** indica um padrão de URL. Veja como está abaixo:

    urlpatterns = patterns('',
        (r'^$', 'django.views.generic.date_based.archive_index',
            {'queryset': Artigo.objects.all(), 'date_field': 'publicacao'}),
        (r'^admin/(.*)', admin.site.root),
    )

Portanto, vamos adicionar a nossa nova URL após a última tupla, para ficar assim:

    urlpatterns = patterns('',
        (r'^$', 'django.views.generic.date_based.archive_index',
            {'queryset': Artigo.objects.all(), 'date_field': 'publicacao'}),
        (r'^admin/(.*)', admin.site.root),
        
        (r'^rss/(?P<url>.*)/$', 'django.contrib.syndication.views.feed',
            {'feed_dict': {'ultimos': UltimosArtigos}}),
    )

A tupla adicionada indica que a nova URL vai exibir os artigos em formato RSS na seguinte endereço:

    http://localhost:8000/rss/ultimos/
    

Não se esqueça da importância dessa palavra **ultimos**, pois é ela que identifica o nosso RSS (sim, podemos ter diversos RSS diferentes em um mesmo site).

Mas ainda não é suficiente, pois o **UltimosArtigos** é um estranho ali. Se você rodar o sistema agora, vai notar uma mensagem de erro, pois esse elemento **UltimosArtigos** realmente não existe.





Pois então, na linha superior à iniciada com **urlpatterns**, vamos adicionar a seguinte linha:

    from blog.feeds import UltimosArtigos
    

E agora todo o arquivo **urls.py**, depois das modificações, fica assim:

    from django.conf.urls.defaults import *
    
    # Uncomment the next two lines to enable the admin:
    from django.contrib import admin
    admin.autodiscover()
    
    from blog.models import Artigo
    from blog.feeds import UltimosArtigos
    
    urlpatterns = patterns('',
        (r'^$', 'django.views.generic.date_based.archive_index',
            {'queryset': Artigo.objects.all(), 'date_field': 'publicacao'}),
        (r'^admin/(.*)', admin.site.root),
        (r'^rss/(?P<url>.*)/$', 'django.contrib.syndication.views.feed',
            {'feed_dict': {'ultimos': UltimosArtigos}}),
    )

Pois bem. Salve o arquivo. Feche o arquivo.

Vamos seguir para a derradeira tarefa antes de ver o nosso RSS funcionando: na pasta da aplicação **blogs**, crie um novo arquivo chamado **feeds.py**, e escreva o seguinte código dentro:

    from django.contrib.syndication.feeds import Feed
    
    from models import Artigo
    
    class UltimosArtigos(Feed):
        title = 'Ultimos artigos do blog do Alatazan'
        link = '/'
    
        def items(self):
            return Artigo.objects.all()
        
        def item_link(self, artigo):
            return '/artigo/%d/'%artigo.id

Pronto! Antes de entender em detalhes o que fizemos no código acima, vamos ver o resultado disso!

Primeiro, execute o seu projeto, clicando duas vezes sobre o arquivo **executar.bat** da pasta do projeto.

Agora localize o seguinte endereço em seu navegador:

    http://localhost:8000/rss/ultimos/
    

E o resultado será este:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/rss-primeira-visao/file/"/>
</p>

Gostou do que viu? Não... eu ainda não. Veja que o **título** e a **descrição** do artigo estão como **"Artigo object"**. Não está como nós queremos. Vamos lá resolver isso?

Na pasta da aplicação **blog**, há uma outra pasta, chamada **templates**. Dentro dela, você vai criar mais uma pasta, chamada **feeds**.

Dentro da nova pasta, vamos criar um template que será usado para determinar o **título** de cada artigo. Ele será chamado **ultimos\_title.html**. Esse nome se deve à soma do nome desse RSS ( **ultimos** ) com **\_title.html**, o nome padrão para isso que queremos fazer. Escreva o seguinte código dentro:

    {{ obj.titulo }}
    

Salve o arquivo. Feche o arquivo. Crie um novo arquivo, chamado **ultimos\_description.html**, e escreva o seguinte código dentro:

    {{ obj.conteudo }}
    

Salve o arquivo. Feche o arquivo. Vá ao navegador e pressione a tecla **F5** para ver como ficou.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/rss-titulo-e-descricao/file/"/>
</p>

Que tal agora? Está melhor?

Ok, vamos conhecer um pouco mais do que fizemos.

O atributo **title** recebeu o valor **'Ultimos artigos do blog do Alatazan'**, que é título deste RSS.

O atributo **link** recebeu o valor **'/'**, indicando que o endereço na web equivalente a este RSS é o do **caminho** '/', que somado ao protocolo ('http'), domínio ('localhost') e porta ('8000'), fica assim:

    http://localhost:8000/
    

O método **def items(self)** carrega uma **lista de artigos**. A linha **return Artigo.objects.all()** indica que este método vai retornar no RSS **todos** os objetos da classe **Artigo** (em ordem decrescente pelo campo **publicacao**, lembra-se?).

E o método **def item_link(self, artigo)** retorna, para cada **artigo**, seu endereço na web, que definimos para ser o caminho '/artigo/' somado ao **id** do artigo, mais uma barra ( / ) ao final, que no fim da história vai ficar assim, por exemplo, para um artigo de id **57**:

    http://localhost:8000/artigo/57/
    
No entanto, nós podemos fazer isso de uma forma diferente. Faça assim: remova as seguintes linhas do arquivo **feeds.py**:

    def item_link(self, artigo):
        return '/artigo/%d/'%artigo.id

Salve o arquivo. Feche o arquivo. Após um **F5** no navegador, na URL do RSS, veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/rss-erro-com-get_absolute_url/file/"/>
</p>

A mensagem de erro completa é esta:

    Give your Artigo class a get_absolute_url() method, or define an item_link() method in your Feed class.
    

Veja que ele sugere a criação do método **item\_link** (que acabamos de remover) ou do método **get\_absolute\_url()** na classe **Artigo**. Vamos fazer isso?

Na pasta da aplicação **blog**, abra o arquivo **models.py** para edição e ao final da classe **Artigo** (o final do arquivo) adicione as seguintes linhas de código:

        def get_absolute_url(self):
            return '/artigo/%d/'%self.id

Ou seja, agora o arquivo **models.py** todo vai ficar da seguinte forma:

    from datetime import datetime
    from django.db import models
    
    class Artigo(models.Model):
        class Meta:
            ordering = ('-publicacao',)
        
        titulo = models.CharField(max_length=100)
        conteudo = models.TextField()
        publicacao = models.DateTimeField(default=datetime.now, blank=True)
    
        def get_absolute_url(self):
            return '/artigo/%d/'%self.id

Salve o arquivo. Feche o arquivo. De volta ao navegador, atualize a página com **F5** e veja que voltamos ao normal!

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/rss-titulo-e-descricao/file/"/>
</p>

O que fizemos agora foi definir um **endereço absoluto de URL** para **artigo**, ou seja, cada artigo tem o seu endereço, e esse endereço é útil não apenas para o RSS, mas também para outros fins, Veja por exemplo o seguinte endereço:

    http://localhost:8000/admin/blog/artigo/1/
    

O resultado é:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/admin-com-get_absolute_url/file/"/>
</p>

Notou o link **"View on site"** em destaque no canto superior direito? Pois bem, clique sobre ele, e o resultado será:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/erro-ao-carregar-exemple_com/file/"/>
</p>

Hummm... não saiu como queríamos... Mas isso aconteceu pelo seguinte motivo:

O Django possui suporte a **múltiplos sites em um único projeto**. Isso quer dizer que se você tem um projeto de blog, você pode ter os blogs do Cartola, do Alatazan e da Nena, **todos eles usando o mesmo projeto**, ainda que trabalhem com informações distintas. Isso às vezes é muito útil.

Quando um projeto é criado, ele recebe um **"site"** padrão, que tem o endereço **http://example.com**, justamente para você modificar e deixar como quiser. Então vamos mudar isso para **http://localhost:8000/**, certo?

Carregue o endereço do **admin** no navegador:

    http://localhost:8000/admin/
    

Na caixa **'Sites'**, clique no item **'Sites'**. Na página carregada, clique sobre **'example.com'**, e modifique, substituindo **'example.com'** para **'localhost:8000'**, assim:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/admin-modificando-exemple_com/file/"/>
</p>

Clique no botão **Save**.

Feche a janela de execução do Django e execute novamente o arquivo **executar.bat** da **pasta do projeto**.

Volte à URL anterior ( http://localhost:8000/admin/blog/artigo/1/ ), clique em **"View on site"** e o resultado será este:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/pagina-do-artigo-nao-encontrada/file/"/>
</p>

Bom, já conhecemos esta página, certo? Isso quer dizer que essa página não existe.

Esta página será a **página de um artigo específico**. Ou seja, Alatazan tem um blog, e na página principal do blog são exibidos todos os artigos, porém, cada artigo possui sua própria URL - sua própria página. Então vamos criá-la?

Na pasta do projeto, abra o arquivo **urls.py** para edição e adicione a seguinte URL:

    (r'^artigo/(?P<artigo_id>\d+)/$', 'blog.views.artigo'),
    

Ou seja, o arquivo **urls.py** vai ficar assim:

    from django.conf.urls.defaults import *
    
    # Uncomment the next two lines to enable the admin:
    from django.contrib import admin
    admin.autodiscover()
    
    from blog.models import Artigo
    from blog.feeds import UltimosArtigos
    
    urlpatterns = patterns('',
        (r'^$', 'django.views.generic.date_based.archive_index',
            {'queryset': Artigo.objects.all(), 'date_field': 'publicacao'}),
        (r'^admin/(.*)', admin.site.root),
        (r'^rss/(?P<url>.*)/$', 'django.contrib.syndication.views.feed',
            {'feed_dict': {'ultimos': UltimosArtigos}}),
        (r'^artigo/(?P<artigo_id>\d+)/$', 'blog.views.artigo'),
    )

Salve o arquivo. Feche o arquivo.

Volte ao navegador, pressione a tecla **F5** e observe que a mensagem mudou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/pagina-do-artigo-nao-existe-view/file/"/>
</p>

Essa mensagem indica que a URL informada ( http://localhost:8000/artigo/1/ ) realmente existe, mas que não foi encontrada a view **'blog.views.artigo'**. Isso acontece porque a view realmente não existe. Ainda!

Agora, na pasta da aplicação **blog**, crie um novo arquivo, chamado **views.py** e escreva o seguinte código dentro:

    from django.shortcuts import render_to_response
    
    def artigo(request, artigo_id):
        return render_to_response('blog/artigo.html')

Salve o arquivo. Feche o arquivo. Volte ao navegador, pressione **F5** e o resultado já mudou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/pagina-do-artigo-nao-existe-template/file/"/>
</p>

A mensagem diz: **"O template 'blog/artigo.html' não existe"**. Sim, não existe, mas vamos criá-lo!

Na pasta da aplicação **blog**, abra a pasta **templates**, e dentro dela, a pasta **blog**. Crie um novo arquivo chamado **'artigo.html'** e escreva o seguinte código dentro:

    <html>
    <body>
    
    Blog do Alatazan
    
    <h1>{{ artigo.titulo }}</h1>
    
    {{ artigo.conteudo }}
    
    </body>
    </html>

Agora volte ao navegador, atualize a página com um **F5** veja o que tem:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/pagina-do-artigo-vazia/file/"/>
</p>

Bom, aconteceu algo errado aqui, não? Veja que no arquivo do template, nós indicamos as variáveis **{{ artigo.titulo }}** e **{{ artigo.conteudo }}** para exibirem respectivamente o **título** e o **conteúdo** do artigo. Mas tudo o que apareceu foi 'Blog do Alatazan'. Porquê?

Porque para as variáveis informadas serem válidas, é preciso que alguém indique que elas existem, e de onde elas vêm.

As variáveis válidas para os templates são indicadas a uma parte "oculta" do processo, chamada **Contexto**. Para indicar a variável **artigo** para o contexto deste template, vamos modificar a **view**. Portanto, na pasta da aplicação **blog**, abra o arquivo **views.py** para edição e o modifique, para ficar da seguinte forma:

    from django.shortcuts import render_to_response
    
    from models import Artigo
    
    def artigo(request, artigo_id):
        artigo = Artigo.objects.get(id=artigo_id)
        return render_to_response('blog/artigo.html', locals())

Salve o arquivo. Feche o arquivo. Volte ao navegador, e... **shazan!**

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/pagina-do-artigo-com-conteudo/file/"/>
</p>

Pronto. Temos uma página para o artigo.

Mas antes de terminarmos o capítulo, falta fazer uma coisa importante: **deixar o navegador saber que o nosso blog fornece RSS**.

Para fazer isso, na pasta da aplicação **blog**, carregue a pasta **templates** e dali a pasta **blog**. Abra o arquivo **artigo_archive.html** para edição, e modifique, para ficar assim:

    <html>
    <head>
    <link rel="alternate" type="application/rss+xml" title="Ultimos artigos do Alatazan" href="/rss/ultimos/" />
    </head>
    
    <body>
    
    <h1>Meu blog</h1>
    
    {% for artigo in latest %}
    <h2>{{ artigo.titulo }}</h2>
    
    {{ artigo.conteudo }}
    {% endfor %}
    
    </body>
    </html>

Salve o arquivo. Feche o arquivo. Vá ao navegador e carregue a página principal de nosso site:

    http://localhost:8000/
    

O resultado será este:

<p align="center">

<img src="http://www.aprendendodjango.com/gallery/pagina-principal-com-icone-do-rss/file/"/>
</p>

Observe o **ícone do RSS** na barra de endereço.

## Fechando o dia com sabor de dever cumprido

- Cara, dá pra ver sua empolgação... pegou tudo? - disse um Cartola também agitado.

- É fascinante! Com essa aulinha de hoje, olha só o que eu aprendi:

 - Mudar o fuso horário do projeto
 - Mudar a ordenação dos artigos
 - Dar um valor automático para a data do artigo
 - Adicionar uma aplicação de contrib
 - Criar a URL do RSS
 - Criar a classe de Feed do RSS
 - Criar as templates para o título e a descrição dos artigos no RSS
 - Criar **a URL e a view do artigo** e sua template
 - Mudar a URL padrão do site
 - Informar varíaveis ao contexto do template
 - Fazer o ícone do RSS aparecer no navegador
 - e...

- Calma! Você vai ter um ataque aqui hein... - interferiu **Nena** - só não vá se esquecer de que **no Python, identação correta é obrigação** viu... não vá esquecer aquele espaço vazio à esquerda, senão *é pau na certa*. Trocar espaço por TAB também é fria...

Os dois corações de Alatazan pulsavam forte e os três caíram na gaitada, rindo da situação.

- Olha, vamos cada um pra sua casa que agora eu vou dar uma volta por aí. Esse blog está feínho que dói, amanhã vamos **dar um jeito no visual dele**.

Cartola disse isso e já foi ajeitando a roupa pra seguir com a sua vida.

**Próximo capítulo: [Fazendo a apresentação do site com templates](http://www.aprendendodjango.com/fazendo-a-apresentacao-do-site-com-templates/)**

