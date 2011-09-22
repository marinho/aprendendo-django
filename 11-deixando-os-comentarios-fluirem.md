Alatazan estava bebendo um copo d'água no intervalo da aula, quando uma garota chegou até ele. Ela segurava um copo com refrigerante e comia um pastel.

- Oi, desculpe, mas seu braço está sujo aqui oh.

Alatazan deu uma olhada em seu cotovelo, mas a única coisa que havia ali eram as listras habituais dos katarenses. Um pouco constrangido e sem saber como explicar à moça, ele gagueijou, antes que outra garota, que parecia ser amiga dela, se aproximou rindo.

- Não acredito... não sua tonta, é mancha, de nascença, você está fazendo o menino ficar com vergonha aqui... presta atenção!

E riu um pouco mais...

Alatazan deu uma risadinha meio de meia boca e deu um jeito de sair dali... não queria entrar naquele "debate".

Depois de pensar um pouco, ele percebeu que isso tinha seu lado bom. No final das contas, essa liberdade de expressão acaba sendo positiva e bem-vinda.

## Habilitando comentários no blog

Às vezes você quer que as pessoas tenham um espaço para **emitir suas opiniões** sobre aquilo que escreve em seu blog. Muitas pessoas gostam disso. E o Django oferece esse recurso de maneira muito acessível.

A coisa se resume a habilitar a aplicação *contrib* de **comentários**, adicionar uma URL e depois ajustar alguns templates. Vamos lá?

Antes de mais nada, execute seu projeto clicando duas vezes sobre o arquivo **"executar.bat"** da pasta do projeto.

Na pasta do projeto, abra o arquivo **"settings.py"** para edição e localize a setting **INSTALLED\_APPS**. Adicione a ela a seguinte linha:

        'django.contrib.comments',
    

Ou seja, o trecho de código da setting **INSTALLED\_APPS** fica assim:

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
    )

Temos agora uma nova aplicação no projeto: **comments**.

Salve o arquivo. Feche o arquivo.

Agora atualize o banco de dados, clicando duas vezes sobre o arquivo **"gerar\_banco\_de\_dados.bat"** da pasta do projeto.

Uma janela do MS-DOS será exibida mostrando o seguinte resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/comentarios-geracao-do-banco/file/"/>
</p>

Ao carregar o navegador na URL do **Admin**:

    http://localhost:8000/admin/
    

Você pode ver que há ali uma nova aplicação chamada **"Comments"**:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/comentarios-admin/file/"/>
</p>

Devido a uma série de ajustes que ela sofreu para a **versão 1.0**, é possível que em sua versão ela ainda não esteja traduzida por completo para português brasileiro.

Mais à frente, você vai utilizar essa seção do **Admin** para administrar os comentários recebidos, pois tem hora que a paciência não é suficiente para lidar com algumas *liberdades* de certos indivíduos.

Pois agora vamos ao segundo passo: **adicionar uma URL** para a aplicação **"comments"**.

Na pasta do projeto, abra o arquivo **"urls.py"** para edição e adicione a seguinte URL ao final da função **patterns()**:

        (r'^comments/', include('django.contrib.comments.urls')),
    

A função **include()** inclui as URLs de outro arquivo na URL em questão. Neste caso, as URLs do módulo **'django.contrib.comments.urls'** são incluídas na URL **'^comments/'**. Na prática, se no módulo **'django.contrib.comments.urls'** houver uma URL **'^post/$'**, o Django junta uma coisa à outra e entende que possui uma URL **'^comments/post/$'** apontando para tal. Isso é feito para todas as URLs encontradas no módulo incluído (**'django.contrib.comments.urls'**).

É uma solução prática para dividir as URLs quando o arquivo se torna muito extenso, e também é muito prático quando você quer separar as coisas para que suas aplicações se tornem mais flexíveis.

Agora, o arquivo **"urls.py"** ficou assim:

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
        (r'^contato/$', 'views.contato'),
        (r'^comments/', include('django.contrib.comments.urls')),
    )

Salve o arquivo. Feche o arquivo.

### Trabalhando as template tags no template do artigo

Agora, na pasta do projeto, abra a pasta **"blog/templates/blog"**. Dentro dela, abra o arquivo **"artigo.html"** para edição e localize a seguinte linha:

    {% extends "base.html" %}
    

Acrescente abaixo dela a seguinte linha:

    {% load comments %}
    

Isso vai carregar as template tags da aplicação **comments**. Agora localize esta outra linha:

    {{ artigo.conteudo }}
    
    
Acrescente esse trecho de código abaixo dela:

    <div class="comentarios">
      <h3>Envie um comentario</h3>
    
      {% render_comment_form for artigo %}
    </div>

Salve o arquivo. Agora vá ao navegador e localize a URL do artigo:

    http://localhost:8000/artigo/1/
    

E veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/comentarios-com-form/file/"/>
</p>

Opa, aí está um formulário de contato completo! Mas o que nós fizemos ali? A template tag **{% render\_comment\_form for artigo %}** gera um formulário dinâmico de comentários para o objeto **"artigo"**. Lembra-se do **Form** que trabalhamos no último capítulo?

Mas ele ficou meio deformando, não? Um pouco de **CSS** vai fazer bem pra esse layout. Da pasta do projeto, entre em **"media"**, abra o arquivo **"layout.css"** para edição e acrescente o seguinte trecho de código ao final do arquivo:

    form label {
      display: block;
    }
    
    textarea {
      height: 100px;
    }

Salve o arquivo. Feche o arquivo. Volte ao navegador, atualize e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/comentarios-com-form-ajustado-css/file/"/>
</p>

Agora vamos preencher o formulário de comentário para ver o que acontece? Preencha assim:

- Nome: **Garota da Escola**
- E-mail: **garota@escola.com**
- URL: **http://escola.com/garota/**
- Comentário: **Parabéns pelo blog!**

Agora clique no botão **"Post"** e veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/comentarios-thanks-for-your-comment/file/"/>
</p>

Bacana! Então o envio do comentário já funcionou! Não se preocupe, vamos mudar o visual dessa página aí já-já. Agora volte à página anterior no navegador ( **ALT+Esquerda** ).

Vamos agora exibir os comentários do artigo?

Volte ao template **"artigo.html"** que estávamos editando e localize a seguinte linha:

    <div class="comentarios">
    

Acrescente abaixo da linha o trecho de código abaixo:

      <h3>Comentarios</h3>
    
      {% get_comment_list for artigo as comentarios %}
      {% for comentario in comentarios %}
      <div class="comentario">
        Nome: {{ comentario.name }}<br/>
        {% if comentario.url %}URL: {{ comentario.url }}{% endif %}<br/>
        {{ comentario.comment|linebreaks }}
        <hr/>
      </div>
      {% endfor %}

Salve o arquivo. Volte ao navegador, atualize a página do artigo e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/comentarios-lista-de-comentarios/file/"/>
</p>

Já melhorou, não é? Pois olha o que fizemos: nós declaramos a template tag **{% get\_comment\_list for artigo as comentarios %}** que obtém todos os comentários que existem para o objeto **"artigo"** e os coloca na variável **"comentarios"**.

Depois, com a variável em mãos, nós fazemos um *laço* nela, usando a template tag **{% for comentario in comentarios %}**, ou seja, todo o trecho de código que está entre as template tags **{% for %}** e **{% endfor %}** será repetido a cada comentário. Se houverem 5 comentários na lista, então será repetido 5 vezes, uma para cada comentário, que é representado pela variável **"comentario"**.

Por fim, a template tag **{% if comentario.url %}** sigfinica que o trecho de código que está entre ela e a template tag **{% endif %}** só deve ser mostrado se o comentário possuir o campo **"url"** preenchido.

Agora, o template **"artigo.html"** completo está assim:

    {% extends "base.html" %}
    
    {% load comments %}
    
    {% block titulo %}{{ artigo.titulo }} - {{ block.super }}{% endblock %}
    
    {% block h1 %}{{ artigo.titulo }}{% endblock %}
    
    {% block conteudo %}
    {{ artigo.conteudo }}
    
    <div class="comentarios">
      <h3>Comentarios</h3>
    
      {% get_comment_list for artigo as comentarios %}
      {% for comentario in comentarios %}
      <div class="comentario">
        Nome: {{ comentario.name }}<br/>
        {% if comentario.url %}URL: {{ comentario.url }}{% endif %}<br/>
        {{ comentario.comment|linebreaks }}
        <hr/>
      </div>
      {% endfor %}
    
      <h3>Envie um comentario</h3>
    
      {% render_comment_form for artigo %}
    
    </div>
    {% endblock %}


Agora observe que no formulário do template, existe também um botão **"Preview"**. Ele faz o seguinte: ao invés de enviar o comentário, ele apenas exibe o resultado para que o usuário veja como vai ficar antes de enviar. Isso é útil para que ele tenha uma segunda chance de ajustar algo no texto.

Mas chega de explicações!

### A função de pré-visualização do comentário

Escreva um novo comentário, clique sobre o botão **"Preview"** e veja o que acontece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/comentarios-visualizacao/file/"/>
</p>

Primeiro, observe a caixa **destacada de vermelho**. Ali será exibida a **pré-visualização** do comentário, antes de confirmar o seu envio. Esta opção não é obrigatória, você pode escolher entre trabalhar com pré-visualização ou não, e vamos ver como fazer isso em breve.

Mas o que precisamos fazer agora é dar um jeito no visual dessa página. Ela não está nada boa.

Na pasta **"templates"** da pasta do projeto, crie uma nova pasta **"comments"**. Dentro da nova pasta crie um arquivo chamado **"base.html"** e escreva dentro o seguinte código:

    {% extends "base.html" %}
    
    {% block titulo %}
      {% block title %}{% endblock %}
    {% endblock %}
    
    {% block conteudo %}
      {% block content %}{% endblock %}
    {% endblock %}

Salve o arquivo.

Mas que confusão com tantos *blocks*! Bom, o que o código acima está fazendo é levar todos os templates da contrib **"comments"** para extenderem o template do seu blog, ou seja, eles serão encaixados no seu visual.

Volte ao navegador, pressione **F5** - será feita uma pergunta sobre os dados enviados, apenas confirme - e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/comentarios-com-template-base/file/"/>
</p>

Já melhorou, né? Agora, clique no botão **"Post"** para publicar esse comentário. Veja:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/comentarios-mensagem-de-sucesso/file/"/>
</p>

Veja que nesta página a mudança no template também surtiu efeito. Mas precisamos ajustar algo aqui. Ao menos um **link** para a página principal é necessária, certo? Não podemos deixar uma página sem meios para dar continuidade.

### Depois de enviado o comentário...

Pois então vá à pasta do projeto, e abra a pasta **"templates/comments"**. Dentro dela, crie o arquivo **"posted.html"** com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% block titulo %}Comentário enviado! - {{ block.super }}{% endblock %}
    
    {% block conteudo %}
    <p>Obrigado por enviar seu comentário, logo vou dar uma olhada!</p>
    
    <p><a href="/">Agora clique aqui para voltar para o blog!</a></p>
    {% endblock %}

Salve o arquivo. Feche o arquivo. Volte ao navegador, atualize e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/comentarios-template-depois-de-publicado/file/"/>
</p>

Agora, é só lançar vários artigos e comentários para ver como a coisa funciona bem!

## A caminho de colocar o site no ar...

Alatazan foi enfático, ao final:

<img src="http://www.aprendendodjango.com/media/img/ilustracao_4.gif" align="right"/>

- Bom, vamos ver o que eu fiz hoje:

 - Instalei a aplicação **'django.contrib.comments'**;
 - **Gerei** o banco de dados;
 - Adicionei a URL que inclui todas as URLs da aplicação **'comments'**;
 - Nos templates que usam template tags de comentários, tive que carregá-las com **{% load comments %}**;
 - Usei as template tags **{% render\_comment\_form %}** e **{% get\_comment\_list %}**, para gerar um formulário e obter os comentários de um artigo, respectivamente;
 - E defini um template **"base.html"** para ajustar as páginas de comentários ao **layout do site**;

- Exatamente! E você ainda fez um ajuste no CSS para dar uma quebra de linha nas tags &lt;label&gt;. - Cartola completou.

- Jóia! Agora posso colocar esse blog no ar!

- Mesmo? Você acha que já está bom pra fazer o *deploy*? - Nena perguntou, ela também está ansiosa para ver o blog de Alatazan estreiar no servidor.

- Bom, na verdade ainda quero melhorar o visual do site, mas...

- Já sei, amanhã vamos tirar o dia pra ver **HTML e CSS** o suficiente para transformar seu blog com um **bom layout**!

- Então combinado.

No próximo capítulo, vamos trabalhar novamente no layout do site, mas agora com um foco mais profissional: vamos dar um visual típico de um blog bem elaborado ao site do Alatazan, afinal, com tanto estudo, ele merece algo bem apresentável e o principal: **no ar**.

<span class="no_print">
**Próximo capítulo: [Um pouco de HTML e CSS só faz bem](/um-pouco-de-html-e-css-so-faz-bem/)**
</span>

