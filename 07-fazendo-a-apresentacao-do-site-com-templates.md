Alatazan levantou cedinho, correu até a geladeira e buscou um doce de goiaba. Aquilo era muito bom e ele devorava cada pedacinho, como se fossem os deliciosos lagartos que vendem nas panificadoras de sua cidade natal.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_1.gif" align="left"/>

Ele ligou a TV. Sempre achou muito engraçada aquela forma primitiva de entretenimento.

No programa educativo daquela manhã, estavam apresentando como funciona a linha de fabricação e montagem de automóveis.

Qualquer um sabe - isso, aqui na Terra, claro - que um automóvel é movido pela força de seu motor, que faz um (ou ambos) de seus eixos girar, e isso faz as rodas levarem o carro à frente. Há diversas outras partes fundamentais em um veículo, e elas são compostas uma por uma, até que no fim da linha de montagem, passam a ser feitos ajustes, pinturas e encaixes com um propósito também muito importante: **o conforto do motorista e o visual do carro**.

Não há nada de agradável em dirigir um automóvel com cadeiras de ferro ou observar um carro sem os devidos acabamentos.

A situação do blog de Alatazan é mais ou menos essa: **funciona mas está longe de ser uma beleza**.

## Então vamos trabalhar nos templates!

Um dos elementos mais importantes do Django, é seu sistema de templates. Ele possui uma forma peculiar de trabalhar que facilita muito a criação da apresentação de suas páginas, mas os caras que criaram o Django fizeram questão que esse trabalho não se misture a outras coisas, ou seja, **não é interessante trazer para dentro do template aquilo que deveria ter sido feito no código da view**.

Mas chega de conversa e vamos ao que interessa!

Antes de mais nada, execute o seu projeto, clicando duas vezes no arquivo **executar.bat** da pasta do projeto (**meu\_blog**).

No navegador, carregue a página inicial:

    http://localhost:8000/
    

Observe por alguns segundos:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/blog-pagina-inicial-com-artigos/file/"/>
</p>

**Você levaria a sério um site assim?** Alatazan também não.

Então vamos criar um **CSS** para melhorar isso.

CSS significa **Cascading Style Sheet**.

Por alguma razão mórbida, Alatazan se lembrou da sociedade RSS de Katara, que se refere a "Rarams Sarabinis Sarados". O significado do nome dessa sociedade conservadora e risonha não era conhecido nem mesmo por seus seguidores, já que era conservada da mesma forma (e por sinal desorganizada) há tantos milênios que o significado se perdeu por lá.

Alatazan voltou sua atenção ao CSS e notou que isso não tinha nada a ver com aquilo. E prosseguiu.

Aproveitando o devaneio de Alatazan, a partir de agora, para facilitar a nossa comunicação, vamos usar o caminho de pastas de forma mais compacta, certo?

Partindo da pasta do projeto (**meu\_blog**), em **blog/templates/blog** abra o arquivo **artigo\_archive.html**, e o modifique de forma que fique assim:

    <html>
    <head>
    <title>Blog do Alatazan</title>
    <link rel="alternate" type="application/rss+xml" title="Ultimos artigos do Alatazan" href="/rss/ultimos/" />
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
    </head>
    
    <body>
    
    <h1>Blog do Alatazan</h1>
    
    {% for artigo in latest %}
      <div class="artigo">
        <h2>{{ artigo.titulo }}</h2>
    
        <div class="conteudo">
          Publicado {{ artigo.publicacao }}<br/>
          {{ artigo.conteudo }}
        </div>
      </div>
    {% endfor %}
    
    </body>
    </html>

No navegador, atualize a página com **F5** e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/blog-pagina-inicial-com-mudanca-no-visual/file/"/>
</p>

Opa, visual já melhorou! Mas aquela data de publicação... não ficou boa.

A maioria das modificações foram estéticas, usando **CSS e HTML**.

A exceção foi a data de publicação, que trata-se da referência **{{ artigo.publicacao }}**, que indica que naquele lugar do HTML, será exibido o conteúdo do atributo **publicacao**  da variável **artigo**. Mas ele ficou definitivamente desagradável.

Há uma forma de melhorar isso, usando um **template filter**, chamado **"date"**. Troque o **{{ artigo.publicacao }}** por **{{ artigo.publicacao|date:"d \d\e F \d\e Y" }}** e veja como fica:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/blog-pagina-inicial-com-template-filter/file/"/>
</p>

O trecho de código que você acrescentou ( **|date:"d \d\e F \d\e Y"** ), faz o seguinte: recebe uma data (ou data/hora, como é este caso) e faz o tratamento, exibindo no seguinte formato: **"DIA de MÊS de ANO"**, e retorna esse valor formato em seu lugar - sem modificar a varíavel original.

Acontece que o mês está em inglês. Então isso quer dizer que o todo o site está preparado para trabalhar na língua inglesa, o que não é o caso de Alatazan, que está vivendo no Brasil.

### Mudando o idioma padrão do projeto

Então vamos agora editar o arquivo **settings.py** da pasta do projeto para mudar isso. Pois então, com o arquivo **settings.py** aberto em seu editor, localize o seguinte trecho de código:

    LANGUAGE_CODE = 'en-us'
    

E modifique, para ficar assim:

    LANGUAGE_CODE = 'pt-br'
    

Como deve ter notado, a mudança foi de "**en**glish-**u**nited**s**tates" para "**p**or**t**uguese-**br**azil".

Salve o arquivo. Feche o arquivo. Volte ao navegador, pressione a tecla **F5** e:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/blog-pagina-inicial-com-lingua-portuguesa/file/"/>
</p>

Pronto! Agora estamos falando a mesma língua!

> **Nota:** dê uma olhadinha na interface de administração ( http://localhost:8000/admin/ ) para ver que agora tudo está em bom português brasileiro.

Mas ainda assim, aquela data ficou ruim, pois não possui horas, e ficou um tanto burocrática. Vamos fazer algo mais bacana?

Voltando ao arquivo do template ( **artigo\_archive.html** ) substitua o trecho **|date:"d \d\e F \d\e Y"** por **|timesince**, salve o arquivo e veja como fica:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/blog-pagina-inicial-com-timesince/file/"/>
</p>

### O problema com arquivos que não são UTF-8

Ahh, agora está muito mais humano - em Katara isso é chamado de "óbvio", mas enfim, não estamos em Katara.

Mas a linguagem ficou estranha, não? "Publicado em 2 dias, 5 horas" não é assim "tão" amigável. Pois então, no arquivo do template, substitua o **em** por **há**.

Salve o arquivo. Porém, é possível que você tenha um problema agora, veja:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/unicode-error/file/"/>
</p>

Se este mesmo erro for exibido para você, é porque você usou um editor que não salva em formato **Unicode** por padrão. Como é o caso do **Bloco de Notas**.

Dessa forma, você deve ir até o menu **Arquivo -> Salvar como** e deixar o mesmo nome do arquivo, mudando os dois campos abaixo do nome para **"Todos os arquivos"** e **"UTF-8"**, respectivamente, como está na imagem abaixo:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/resolvendo-unicode-error-no-bloco-de-notas/file/"/>
</p>

Agora, ao retornar ao navegador, veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/blog-pagina-inicial-com-acento/file/"/>
</p>

Pronto. Questão dos caracteres especiais resolvida. Da próxima vez que isso ocorrer, você já sabe como proceder.

Agora observe bem o arquivo de template, e você pode notar o seguinte trecho de código:

    {% for artigo in latest %}
      <div class="artigo">
        <h2>{{ artigo.titulo }}</h2>
    
        <div class="conteudo">
          Publicado {{ artigo.publicacao }}<br/>
          {{ artigo.conteudo }}
        </div>
      </div>
    {% endfor %}

Veja que o trecho é iniciado com **{% for artigo in latest %}** e finalizado com **{% endfor %}**.

Trata-se de um **template tag**.

Todos os **Template Tags** iniciam e terminam com **{%** e **%}** respectivamente, e são mais poderosos que qualquer outro elemento em um template, pois é possível trabalhar com blocos, como é o caso desse **laço de repetição** que é aberto pelo template tag **{% for %}** e fechado com seu template tag de fechamento **{% endfor %}**.

### Incluindo um template dentro de outro

Após aquele trecho de código citado acima, acrescente este outro trecho de código:

    {% include "rodape.html" %}
    

Salve o arquivo. Feche o arquivo. Vá ao navegador e atualize com **F5**. Veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/blog-template-include/file/"/>
</p>

Isso acontece, pois a template tag **{% include %}** inclui um template dentro de outro. Maneiro, não é? E como esse outro template incluído não existe, ele exibiu a mensagem de erro.

Vamos então criar esse novo arquivo. Mas pense comigo: **o rodapé de um template não deve ser restrito ao blog, mas deve ser do site como um todo**, concorda? Então dessa vez nós não vamos adicionar esse template à pasta de templates da aplicação **blog**.

Na pasta do projeto, crie uma nova pasta, chamada **"templates"** e dentro dela, crie o arquivo **"rodape.html"** com o seguinte código dentro:

    <div class="rodape">
    Por favor não faça contato. Obrigado pela atenção.
    </div>

Salve o arquivo. Feche o arquivo.

### Criando uma pasta geral para templates do projeto

Mas só isso não é suficiente, pois precisamos dizer ao projeto que ele agora possui uma pasta de templates própria, o que não é feito automaticamente como é feito para aplicações. Portanto, na pasta do projeto, abra o arquivo **settings.py** para edição e localize o seguinte trecho:

    TEMPLATE_DIRS = (
        # Put strings here, like "/home/html/django_templates" or "C:/www/django/templates".
        # Always use forward slashes, even on Windows.
        # Don't forget to use absolute paths, not relative paths.
    )

A tupla **TEMPLATE\_DIRS** indica as pastas onde podem ser encontrados templates para o projeto, além daquelas que já são automaticamente localizadas dentro das aplicações. O Django não incentiva o uso exagerado dessas pastas "extra" de templates, pois entendemos que a maior partes deles deve ser concentrada em suas devidas aplicações, mas em muitas vezes é inevitável possuir alguns arquivos-chave numa dessas pastas.

Modifique o trecho de código para ficar assim:

    TEMPLATE_DIRS = (
        'templates',
    )

Salve o arquivo.

Agora, ao atualizar seu navegador, o resultado será este:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/blog-pagina-inicial-com-rodape/file/"/>
</p>

No entanto, há uma coisa desagradável aqui. O caminho **'templates'** não é um caminho absoluto, e isso pode te levar a ter diversos problemas. Vamos portanto fazer uma boa prática já! No início do arquivo **settings.py**, acrescente as seguintes linhas:

    import os
    PROJECT_ROOT_PATH = os.path.dirname(os.path.abspath(__file__))

Agora, modifique a tupla **TEMPLATE\_DIRS** para ficar assim:

    TEMPLATE_DIRS = (
        os.path.join(PROJECT_ROOT_PATH,'templates'),
    )

E não somente isso. Localize esta linha:

    DATABASE_NAME = 'meu_blog.db'             # Or path to database file if using sqlite3.
    

E modifique para ficar assim:

    DATABASE_NAME = os.path.join(PROJECT_ROOT_PATH,'meu_blog.db')
    

Isso faz com que esses caminhos de pastas e arquivos sejam caminhos completos, e não somente o seu único nome simples.

### A template tag {% url %}

Pois agora vamos voltar ao template **"artigo_archive.html"** e localizar a seguinte linha:

    <h2>{{ artigo.titulo }}</h2>
    

Modifique para ficar assim:

    <a href="{{ artigo.get_absolute_url }}"><h2>{{ artigo.titulo }}</h2></a>
    

Isso faz com que o título do artigo passe a ser um link para sua própria página. Mas há ainda uma forma melhor de indicar um link, usando a template tag **{% url %}**.

Essa template tag é bacana, pois é flexível às mudanças. Usando a template tag **{% url %}** para indicar um link, você pode modificar o caminho da URL no arquivo **urls.py** e não precisa se preocupar com os vários links que existem espalhados pelos templates indicando àquela URL, pois eles são ajustados automaticamente. Portanto, modifique o mesmo trecho de código para ficar assim:

    <a href="{% url blog.views.artigo artigo_id=artigo.id %}"><h2>{{ artigo.titulo }}</h2></a>
    

Observando o que escrevemos, você vai notar que a template tag indica primeiramente o caminho da view ( aplicação **blog**, arquivo **views.py**, view **artigo** ) e o argumento **artigo\_id** recebendo o código do artigo ( **artigo.id** ).

Verificando a URL em questão, veja se reconhece as semelhanças:

    (r'^artigo/(?P<artigo_id>\d+)/$', 'blog.views.artigo'),
    

Notou o fator de ligação? Observe os elementos **blog.views.artigo** e **artigo\_id**.

Salve o arquivo. Feche o arquivo. E agora, ao carregar a página no navegador novamente, você vai notar o link para a página no artigo.

### A herança de templates

Pois bem, falando na página do artigo, vamos ver como ela está?

Localize em seu navegador a seguinte URL:

    http://localhost:8000/artigo/1/
    

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/pagina-do-artigo-com-conteudo/file/"/>
</p>

Como pode ver, a mudança visual que fizemos no template da página inicial não afetou em nada a página do artigo. Isso ocorre porque de fato os dois templates são distintos. Mas existe uma solução pra isso!

No conceito de **Programação Orientada a Objetos** há dois elementos importantes, e algumas vezes conflitantes, chamados **herança** e **composição**. No sistema de templates do Django, você pode trabalhar com ambos os conceitos, adotando o mais adequado para cada situação.

Nós já trabalhamos o conceito de **composição** lá atrás, quando você conheceu a template tag **{% include %}**, que trabalha na idéia de compôr um template acrescentando outros templates de fim específico (como apresentar tão somente um rodapé, por exemplo) como parte dele.

Agora então vamos fazer uso da **herança**.

O conceito de herança trata-se de concentrar uma parte **generalizada** em um template que será **herdado** por outros, ou seja, estes outros serão uma cópia daquele, modificando as partes que assim o desejarem. Isso é **muito útil** para se evitar re-trabalho na construção de layouts de páginas.

Pois então vamos ao trabalho.

Na pasta **"templates"** do **projeto** ( **meu\_blog/templates** ), crie um novo arquivo, chamado **"base.html"**, e escreva o seguinte código dentro:

    <html>
    <head>
    <title>{% block titulo %}Blog do Alatazan{% endblock %}</title>
    <link rel="alternate" type="application/rss+xml" title="Ultimos artigos do Alatazan" href="/rss/ultimos/" />
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
    </head>
    
    <body>
    
    <h1>{% block h1 %}Blog do Alatazan{% endblock %}</h1>
    
    {% block conteudo %}{% endblock %}
    
    {% include "rodape.html" %}
    
    </body>
    </html>

Você pode notar que se trata do mesmo código HTML que fizemos no template **"artigo\_archive.html"**, com algumas modificações. E essas modificações foram a remoção de partes do código que eram específicas daquela página (uma listagem de artigos), e a criação de áreas potencialmente modificáveis. Essas áreas são as template tags **{% block %}**.

A template tag **{% block %}** indica que naquele espaço aberto por **{% block %}** e fechado por **{% endblock %}**, os templates que herdarem este **podem fazer mudanças** adequadas à sua realidade.

Salve o arquivo. Vamos agora voltar a editar o template **"artigo\_archive.html"**, da pasta **blog/templates/blog** e modificar para todo o arquivo ficar assim:

    {% extends "base.html" %}
    
    {% block conteudo %}
    {% for artigo in latest %}
      <div class="artigo">
        <a href="{% url blog.views.artigo artigo_id=artigo.id %}"><h2>{{ artigo.titulo }}</h2></a>
    
        <div class="conteudo">
          Publicado há {{ artigo.publicacao|timesince }}<br/>
          {{ artigo.conteudo }}
        </div>
      </div>
    {% endfor %}
    {% endblock %}

Puxa! Diminuiu bastante, não foi?

Sim, e o que fizemos foi exatamente o oposto do template **base.html**: removemos as partes generalizadas e levadas para o template herdado e deixamos somente as partes específicas desse template.

Note a template tag **{% extends "base.html" %}** no início do template. É ela quem diz que este template será uma **extensão** daquele que criamos ( **base.html** ). Essa template tag deve sempre estar posicionada no início do template.

A partir do momento em que um template extende outro, **qualquer código que estiver fora** de uma template tag **{% block %}** será ignorado. Portanto, veja que o trecho de código específico desse template foi colocado dentro do bloco **{% block conteudo %}**.

Os demais blocos não foram declarados ou citados porque nesta página não precisamos modificá-los.

Salve o arquivo. Feche o arquivo. E ao atualizar o navegador com **F5**, verifique que o visual da página inicial permanece o mesmo.

Agora vamos fazer o mesmo trabalho no template **artigo.html** da pasta **blog/templates/blog**. Abra esse arquivo para edição e modifique para ficar assim:

    {% extends "base.html" %}
    
    {% block titulo %}{{ artigo.titulo }} - {{ block.super }}{% endblock %}
    
    {% block h1 %}{{ artigo.titulo }}{% endblock %}
    
    {% block conteudo %}
    {{ artigo.conteudo }}
    {% endblock %}

Veja que extendemos o mesmo template de herança ( **base.html** ). Mas desta vez declaramos os outros blocos, pois precisávamos mudar seus conteúdos.

O caso do bloco **{% block titulo %}** é especial, pois ele possui um elemento bastante útil aqui, o **{{ block.super }}**.

O **{{ block.super }}** é uma variável que carrega o conteúdo do bloco no template herdado, e quando ele é informado, isso significa que naquele lugar deve ser mantido o conteúdo do bloco original. Ou seja, nós mudamos o **título** da página, mas mantivemos o original, apenas acrescentando à esquerda o trecho adicional que queríamos.

Agora salve o arquivo. Feche o arquivo. Vá até o navegador e veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/pagina-do-artigo-com-template-extendido/file/"/>
</p>

Gostou do resultado?

Agora podemos representar a herança e composição dos nossos templates da seguinte forma:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/heranca-e-composicao-de-templates-diagrama/file/"/>
</p>

## Agora, partindo para separar as coisas...

- Se eu gostei? Mas claro, isso é fascinante, logo logo vou poder colocar meu blog no ar e...

Num rápido relance, Alatazan interrompeu seu discurso ao ver um vulto passar pela janela. Ele não identificou aquele movimento rápido, mas algo lhe lembrou daquela vez que havia conhecido Ballzer, na praça próxima de sua casa, em Katara.

Ele coçou a cabeça e percebeu que já era tarde. A empolgação o manteve ligado nas explicações de Cartola e Nena e agora era a hora de voltar rapidamente para casa. De outra forma, poderia perder a aula de xadrez daquele dia.

- Alatazan, leve esse livro com você, vai te ajudar a pegar alguns lances mais comuns de Xadrez - Nena pegou um livro na estante e entregou a ele. - você vai notar que cada peça tem uma dinâmica diferente, e algumas delas nem se pode chamar de dinâmica, de tão limitadas que são... mas cada uma cumpre o seu papel.

Alatazan agradeceu com um gesto típico de Katara e se despediu.

No próximo capítulo vamos separar algumas coisas do template, aplicar imagens e conhecer um pouco mais sobre como fazer isso.

**Próximo capítulo: [Trabalhando com arquivos estáticos](http://www.aprendendodjango.com/trabalhando-com-arquivos-estaticos/)**

