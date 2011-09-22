# DESCRICAO CURTA

<img src="http://www.aprendendodjango.com/media/img/ilustracao_2.gif" align="right"/>

Neste capítulo, Alatazan aprende a configurar o projeto para trabalhar com **arquivos estáticos** e define uma URL para isso.

Arquivos estáticos são arquivos que são enviados ao navegador exatamente como estão no HD do servidor. Ou seja, toda vez que são usados, eles são apresentados do mesmo jeito. Isso significa que a maioria das **imagens, CSS, JavaScript e Shockwaves Flash** devem ser tratados dessa forma.

Para isso, Alatazan vai fazer alguns ajustes nas **settings**, nas **URLs** e nas pastas, para mostrar sua foto e separar o arquivo CSS do HTML.

# TEXTO

<img src="http://www.aprendendodjango.com/media/img/ilustracao_2.gif" align="right"/>

O que sensibiliza Alatazan desde que chegou à Terra é que nós humanos possuímos uma virtude que também é importante em Katara. Seres orgânicos possuem **individualidade**.

- Cada flor, mesmo sendo da mesma espécie e da mesma planta, possui um tamanho diferente, um tom de cor particular. - dizia ele consigo mesmo enquanto tomava um sorvete na praça.

O sorvete caiu no chão e balançou a cabeça. Mas continuou:

- Cada pessoa tem um jeito diferente de agir, uma importância particular para o todo, e tratar a todos como se fossem iguais seria desperdiçar o que cada um tem de melhor...

Agora um ou dois pombos mexiam no sorvete espalhado pelo chão.

Filosofias à parte, Alatazan se levantou e seguiu para a casa de Cartola.

- Vamos lá meu amigo, hoje é dia de estudar outra peça importante na engrenagem do site

## Os arquivos estáticos

Um site não se resume a páginas em HTML. Ele também possui imagens, arquivos CSS, JavaScript, Flash e outros formatos diversos.

Mas há uma diferença fundamental entre umas e outras partes do site, e isso não é restrito ao tipo de conteúdo que será enviado para o navegador, e sim a como aquele conteúdo se comporta no site.

Ele é **dinâmico** ou **estático**?

A página inicial do blog apresenta um resultado diferente a cada vez que é criado um novo artigo. Seu conteúdo poderia variar também dependendo do usuário que está autenticado naquela hora. Essa é uma página **dinâmica**.

Já a foto sorridente de Alatazan no canto superior esquerdo do site, será a mesma até que alguém substitua o arquivo por outro. Então, ela é **estática**.

Mas que foto? Não há nenhuma foto por lá. Então é isso que vamos fazer.

Antes de mais nada, não se esqueça de executar o projeto no Django, clicando duas vezes sobre o arquivo **executar.bat** da pasta do projeto.

Agora abra o arquivo de template **artigo\_archive.html, da pasta **blog/templates/blog** para edição, e localize a seguinte linha:

    {% block conteudo %}
    

Agora acrescente a seguinte linha abaixo dela:

    <img src="{{ MEDIA_URL }}foto.jpg"/>
    

Salve o arquivo. Feche o arquivo. Essa tag HTML vai exibir uma imagem chamada **foto.jpg** no caminho apontado pela varíavel **{{ MEDIA\_URL }}**.

A varíavel **{{ MEDIA\_URL }}** contém o mesmo valor contido na **setting MEDIA\_URL**.

Então vamos agora abrir o arquivo **setings.py** da pasta do projeto para edição, e localizar esta setting:

    MEDIA_URL = ''
    

A setting **MEDIA\_URL** tem a função de indicar o endereço que contém **arquivos estáticos** do projeto. 

Isso não indica que seja possível ter apenas um. Em grandes projetos é necessário haver mais caminhos para arquivos estáticos, então são tomados outras práticas para isso. Mas sempre que possível, é recomendável ter apenas um lugar que forneça esses arquivos. E o caminho mais indicado é o **/media/**. Portanto, modifique essa linha para ficar assim:

    MEDIA_URL = '/media/'
    

Mas só isso não é suficiente. Localize a setting **MEDIA\_ROOT** e veja como está:

    MEDIA_ROOT = ''
    

Esta setting indica o caminho no HD onde estão os arquivos estáticos. Modifique esta setting para ficar assim:

    MEDIA_ROOT = os.path.join(PROJECT_ROOT_PATH, 'media')
    

Você deve ser lembrar de que já usamos essa função **os.path.join()** com a setting **PROJECT\_ROOT\_PATH** não é mesmo? E ela retorna para a setting **MEDIA\_ROOT** o caminho completo para a pasta do projeto ( **meu\_blog** ) somada com a subpasta **media**.

E por fim, ainda temos uma coisa para fazer. Localize a setting **ADMIN\_MEDIA\_PREFIX** e veja como está:

    ADMIN_MEDIA_PREFIX = '/media/'
    

Esta setting indica o caminho para os arquivos estáticos do **Admin** - a interface automática de administração do Django. Acontece que ela está apontando exatamente para o mesmo caminho que a setting **MEDIA\_URL** e isso vai dar em **conflito**. Então a modifique para ficar assim:

    ADMIN_MEDIA_PREFIX = '/admin_media/'
    

Você deve estar se perguntando **"mas se estas são as melhores práticas, então porquê essas settings já não vêm dessa forma?"**. Ahn... **melhores práticas** é uma expressão muito forte. E este assunto trata de variações diversas em projetos distintos. Digamos que estas sejam as melhores práticas para sites pequenos ou sites em desenvolvimento. Mas ainda assim não são as únicas boas práticas para isso. Portanto, o Django tenta ser o mais discreto possível, deixando essa decisão para você.

Salve o arquivo. Feche o arquivo.

Acha que é só isso? Não é. O caminho que indicamos à setting **MEDIA\_URL** não existe no arquivo **urls.py** e como este é um **ambiente de desenvolvimento**, temos isso a fazer ali: abrir o arquivo **urls.py** para edição e adicionar a seguinte URL:

        (r'^media/(.*)$', 'django.views.static.serve',
         {'document_root': settings.MEDIA_ROOT}),

Notou que fizemos a ligação entre as settings **MEDIA\_URL** e **MEDIA\_ROOT**? A segunda dá suporte à primeira.

Mas o elemento **settings.MEDIA\_ROOT** ali não vai funcionar, pois é preciso dizer quem é o **"settings"** em questão. Portanto, localize a seguinte linha no arquivo:

    from django.conf.urls.defaults import *

E adicione abaixo dela:

    from django.conf import settings

Agora o arquivo **urls.py** ficou assim:

    from django.conf.urls.defaults import *
    from django.conf import settings
    
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
        (r'^media/(.*)$', 'django.views.static.serve',
         {'document_root': settings.MEDIA_ROOT}),
    )

Salve o arquivo. Feche o arquivo.

Pronto, já temos a URL funcionando. Mas agora precisamos ter uma pasta **"media"** na pasta do projeto, com o arquivo **foto.jpg** dentro, certo?

Então vá até a pasta do projeto e crie a pasta **media**.

A foto do Alatazan é esta:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/foto-de-alatazan/file/"/>
</p>

Você pode usá-la, baixando da seguinte URL:

    http://www.aprendendodjango.com/gallery/foto-de-alatazan/file/
    

Ou pode usar uma foto sua. Como preferir. O que importa é que essa imagem deve ser do formato **JPEG**, o nome do arquivo deve ser **"foto.jpg"**, e deve ser colocada na pasta **media**. Isso porque este é o endereço indicado pela tag HTML desta imagem, lá no template.

Feito isso, agora temos a imagem da foto do Alatazan, devidamente salva na pasta correta e com as todas referências finalizadas. Vamos ao navegador? Pois bem, localize no seu navegador a URL principal do blog:

    http://localhost:8000/
    

E veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/pagina-principal-com-imagem/file/"/>
</p>

Bacana! Agora temos uma imagem posicionada em nosso layout!

É possível que haja uma dúvida em sua cabeça agora: **"mas o Django só suporta imagens JPEG?"**

Não. isso não está sob poder do Django. O Django suporta qualquer imagem ou outro formato de arquivo estático, seja ele Shockwave Flash (.swf), imagens (.gif, .jpg, .png e outras), mídia digital (.mp3, .wma, .ogg e outras), JavaScript (.js), CSS (.css) ou qualquer outro formato de arquivo **estático**. Essa é uma definição que não depende do Django, mas sim de seu servidor e navegador que vão trabalhar com esses arquivos.

Agora vamos fazer outra modificação: separar o **CSS** do template, e criar um arquivo estático para conter somente o CSS. Vamos?

Na pasta do projeto, abra a pasta **templates**. Nesta pasta, abra o arquivo **base.html** para edição, e substitua o seguinte trecho de código:

    <style type="text/css">
    body {
      font-family: arial;
      background-color: green;
      color: white;
    }
    
    h1 {
      margin-bottom: 10px;
    }
    
    h2 {
      margin: 0;
      background-color: #eee;
      padding: 5px;
      font-size: 1.2em;
    }
    
    .artigo {
      border: 1px solid black;
      background-color: white;
      color: black;
    }
    
    .conteudo {
      padding: 10px;
    }
    </style>

Por este:

    <link rel="stylesheet" type="text/css" href="{{ MEDIA_URL }}layout.css"/>
    

Agora o nosso template **base.html** ficou assim:

    <html>
    <head>
    <title>{% block titulo %}Blog do Alatazan{% endblock %}</title>
    <link rel="alternate" type="application/rss+xml" title="Ultimos artigos do Alatazan" href="/rss/ultimos/" />
    <link rel="stylesheet" type="text/css" href="{{ MEDIA_URL }}layout.css"/>
    </head>
    
    <body>
    
    <h1>{% block h1 %}Blog do Alatazan{% endblock %}</h1>
    
    {% block conteudo %}{% endblock %}
    
    {% include "rodape.html" %}
    
    </body>
    </html>

Salve o arquivo. Feche o arquivo. E veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/pagina-principal-sem-css/file/"/>
</p>

Opa! Parece que perdemos o nosso visual verde bandeira! Sim, mas vamos agora criar o arquivo de CSS para voltar ao normal.

Na pasta **"media"** do projeto, crie um arquivo chamado **"layout.css"** e escreva o seguinte código dentro:

    body {
      font-family: arial;
      background-color: green;
      color: white;
    }
    
    h1 {
      margin-bottom: 10px;
    }
    
    h2 {
      margin: 0;
      background-color: #eee;
      padding: 5px;
      font-size: 1.2em;
    }
    
    .artigo {
      border: 1px solid black;
      background-color: white;
      color: black;
    }
    
    .conteudo {
      padding: 10px;
    }

Notou que é exatamente o mesmo que removemos do template **base.html**, exceto pela ausência das tags **&lt;style&gt;**?

Pois agora salve o arquivo. Feche o arquivo. Volte ao navegador e veja que o visual do site voltou ao normal:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/pagina-principal-com-imagem/file/"/>
</p>

( **Atualização de Correção** )

Agora, vá novamente à página do artigo, no navegador, na seguinte URL:

    http://localhost:8000/artigo/1/
    

O resultado será o seguinte:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/template-do-artigo-sem-estilo/file/"/>
</p>

Opa! Onde foi parar o estilo CSS desta página?

Acontece que esta página vem que uma **view** que foi feita manualmente por você, e esta não possui um **contexto** que contenha um valor para a variável **{{ MEDIA\_URL }}**, e isso fez com que sua referência ficasse vazia, perdendo seu estilo.

Mais pra frente vamos ver isso em detalhes, mas agora, vamos nos atentar e resolver o problema.

Na pasta da aplicação **"blog"**, abra o arquivo **views.py** para edição, e modifique o arquivo para ficar da seguinte forma:

    from django.shortcuts import render_to_response
    from django.template import RequestContext
    
    from models import Artigo
    
    def artigo(request, artigo_id):
        artigo = Artigo.objects.get(id=artigo_id)
        return render_to_response('blog/artigo.html', locals(),
            context_instance=RequestContext(request))

O que nós fizemos? Nós acrescentamos a seguinte linha:

    from django.template import RequestContext
    

E modificamos esta outra:

        return render_to_response('blog/artigo.html', locals(),
            context_instance=RequestContext(request))

Essas modificações fazem com que algumas variáveis especiais (incluindo a **{{ MEDIA\_URL }}**) sejam válidas nos templates.

Salve o arquivo. Feche o arquivo. Vá ao navegador, atualize com **F5** e veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/template-do-artigo-com-estilo/file/"/>
</p>

Pronto! Agora sim, o problema foi resolvido.

## De um extremo ao outro: do estático para as páginas planas!

O tempo passou voando e Alatazan já estava curioso em conhecer ferramentas para a criação de imagens: o **Inkscape** e o **Gimp**. Essas duas ferramentas são fantásticas para a criação de imagens para sites. Nena já havia comentado sobre elas.

Mas ele não quis perder o foco, e comentou com o dono da casa:

- Bom, hoje foi bem diferente...

- Mas é uma parte muito, muito importante do site, Alatazan. Não existem sites sem CSS e imagens, ou pelo menos não existem **bons** sites sem eles - interrompeu Nena, muito enfática - e este é um ponto bem confuso, às vezes...

- Sim, é mesmo... é um pouco confuso. Mas depois que você aprende, é simples entender que:

 - É preciso ajustar as settings **MEDIA\_URL**, **MEDIA\_ROOT** e **ADMIN\_MEDIA\_PREFIX**
 - A setting **MEDIA\_URL** é uma URL para a pasta **MEDIA\_ROOT**
 - É preciso criar a pasta **media** na pasta do projeto
 - É preciso criar a URL **^media/**
 - Toda referência a arquivos estáticos nos templates devem iniciar com **{{ MEDIA\_URL }}**

- Muito bom, muito bom! - foi a vez de Cartola.

- Agora que tenho a minha foto na página principal, eu estou sentindo falta de um lugar para falar quem sou eu, e alguma informação a mais...

- Sim. Vamos fazer assim: amanhã vamos estudar sobre **Páginas Planas**, é pra isso que elas servem! Agora vamos embora que o calor está nos *convidando* pra um passeio lá na rua.

**Próximo capítulo: [Páginas de conteúdo são FlatPages](http://www.aprendendodjango.com/paginas-de-conteudo-sao-flatpages/)**

