Alatazan aceitou o convite de Nena para assistir à estréia de sua irmã mais nova em um desfile de moda. Estava um pouco frio, mas ainda era cedo, chegaram ao ginásio cerca de uma hora e meia antes da abertura, pois foi Nena que dirigiu para levar a irmã.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_4.gif" align="left"/>

Como estavam por ali, entraram no vestiário com a menina, e acabaram vendo como a coisa acontece nos bastidores.

É engraçado como essas coisas parecem uma completa bagunça para quem não é do ramo. Panos, fitas, faixas, coisas coloridas, arames e toda sorte de agulhas e linhas eram só uma fração da variedade de coisas ali, e no começo as modelos se pareciam mais com atrizes de picadeiro.

O cheiro de cola imperava no ar, e havia também um calorzinho com cheiro de ferro de passar ligado, do lado direito, próximo à cortina.

"Neninha", como Alatazan chama carinhosamente a irmã de Nena, seria uma das últimas a participar e eles ficaram lá dentro o suficiente para ver as primeiras saírem do vestiário e caminharem para a passarela.

O interessante é que depois de toda a arrumação, em questão de segundos a coisa parece sair de uma situação incompreensível para uma apresentação louvável. A moça que antes parecia um papagaio depenado, já estava agora refinada, delicada, cheia de glamour.

Ahh, já ia esquecendo... foi nesse dia que Nena e Alatazan estudaram XHTML e CSS e o resultado foi muito interessante, acompanhe...

## Tempo para o (X)HTML e o CSS

No capítulo 7, trabalhamos com templates, e tivemos um contato próximo com o HTML.

No capítulo 8, trabalhamos com arquivos estáticos, e lá usamos um pouco de CSS.

Mas agora, o que vamos tratar não é o conhecimento do sistema de templates, e muito menos a disposição de arquivos estáticos. Estamos falando de apresentação visual, a roupagem estética que o site precisa receber antes de ser apresentado ao usuário.

Não é a nossa intenção fazer desse um trabalho visual para ganhar algum prêmio, e sim, conhecer um pouco do assunto.

Antes de mais nada, execute seu projeto, clicando duas vezes no arquivo **"executar.bat"** da pasta do projeto.

Pois bem, vamos começar pelo template mais básico de nosso site. Aquele que todos extendem: o **"base.html"**, da pasta **"templates"** do projeto. Abra esse arquivo para edição e o modifique para ficar assim:

    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
    	"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
    <html xmlns="http://www.w3.org/1999/xhtml" xml:lang="pt-br" lang="pt-br">
     <head>
      <title>{% block titulo %}Blog do Alatazan{% endblock %}</title>
    
      {% block meta %}
      <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
      <meta http-equiv="Content-Language" content="pt-br" />
      <meta name="keywords" content="Python, Django, Alatazan, Katara, blog" />
      <meta name="description" content="Este é o blog do Alatazan, um katarense aprendendo Django." />
      {% endblock meta %}
    
      {% block feeds %}
      <link rel="alternate" type="application/rss+xml" title="Ultimos artigos do Alatazan" href="/rss/ultimos/" />
      {% endblock feeds %}
    
      {% block style %}
      <link rel="stylesheet" type="text/css" href="{{ MEDIA_URL }}layout.css"/>
      {% endblock style %}
    
      {% block scripts %}
      {% endblock scripts %}
     </head>
    
     <body>
      <div id="tudo">
       <div id="topo">
        {% block topo %}
        <div id="foto">&nbsp;</div>
        Blog do Alatazan
        {% endblock topo %}
       </div>
    
       <div id="menu">
        {% block menu %}
        {% spaceless %}
        <ul>
         <li><a href="/">Pagina inicial</a></li>
         <li><a href="/sobre-mim/">Sobre mim</a></li>
         <li><a href="{% url views.contato %}">Contato</a></li>
        </ul>
        {% endspaceless %}
        {% endblock menu %}
       </div>
    
       <h1>{% block h1 %}{% endblock %}</h1>
    
       <div class="corpo">
        {% block conteudo %}{% endblock %}
       </div>
    
       {% include "rodape.html" %}
    
      </div>
     </body>
    </html>

Salve o arquivo. Assegure-se de que está salvando o arquivo no padrão **UTF-8**. Mas quanta coisa hein?

Como pode notar, modificamos o template em grande parte, portanto, vamos falar das mudanças por partes.

### HTML ou XHTML?

A principal mudança estrutural que fizemos ali foi que saímos do **HTML para o XHTML**. Isso aconteceu por causa dessas duas linhas:

    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
    	"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
    <html xmlns="http://www.w3.org/1999/xhtml" xml:lang="pt-br" lang="pt-br">

O XHTML é uma **evolução** do HTML, que obedece a regras melhor definidas. Isso a torna potencialmente mais rápida e melhor compreendida por navegadores e mecanismos de busca.

As principais novidades que o XHTML oferece são:

* A página deve iniciar com uma declaração de **DOCTYPE**;
* A tag **&lt;html&gt;** deve possuir um **namespace** e definição de idioma;
* Ela é **_case sensitive_**, ou seja, letras maiúsculas e minúsculas são consideradas diferentes;
* Todas as tags devem ser declaradas em letras minúsculas (ex.: **&lt;body&gt;** ao invés de **&lt;BODY&gt;**);
* Todas as tags vazias devem ser fechadas com uma barra ao final (ex.: **&lt;br/&gt;** ao invés de **&lt;br&gt;**);
* Todas as tags preenchidas devem ser fechadas (ex.: **&lt;option&gt;teste&lt;/option&gt;** ao invés de **&lt;option&gt;teste**);
* Todos os atributos das tags devem ser abertos e fechados com aspas duplas (ex.: **&lt;p align="center"&gt;** ao invés de **&lt;p align=center&gt;**).

#### DOCTYPE

O **DOCTYPE** indica qual é o tipo da página. Cada tipo implica em um conjunto de regras diferentes que serão aplicadas à página quando esta for renderizada.

Os tipos existentes são:

* **Transitional** - é o tipo mais flexível. Permite tags HTML que são marcadas como *deprecated* (marcadas como descontinuadas) e permite também páginas que não usem CSS da maneira devida;

* **Strict** - é o tipo mais rígido. Aplica todas as regras "ao pé da letra";

* **Frameset** - aplica regras específicas para *frames* e também possui a mesma flexibilidade da **Transitional**;

#### Namespace e idioma

O padrão que definimos aponta o *XML namespace* "http://www.w3.org/1999/xhtml", que define o documento como compatível com o padrão de XHTML definido pelo W3C em 1999. Além disso, definimos o idioma como **"pt-br"**

### Bloco "meta"

Você viu que definimos um **{% block meta %}**? Pois bem, é ali que vamos descrever tags HTML de meta-definições. As que definimos agora são:

    <meta http-equiv="Content-type" content="text/html; charset=utf-8" />

Esta acima define o padrão de caracteres **utf-8**, ou seja, **Unicode**, o conjunto de caracteres que suporta os mais importantes alfabetos do mundo.

    <meta http-equiv="Content-Language" content="pt-br" />

Esta outra acima define o idioma do conteúdo como **"português brasileiro"**.

    <meta name="keywords" content="Python, Django, Alatazan, Katara, blog" />

Esta outra acima define as palavras-chave para busca. O ideal é informar entre **5 e 10 palavras-chave**, pois isso torna sua página mais consistente no momento de ser indexada por mecanismos de busca como o Google.

Não invente de colocar ali 20 ou 30 palavras pensando que isso vai fazer sua página ficar "mais importante". Uma atitude dessa faz as palavras contidas perderem seu "valor" no momento da busca, e o tiro sai pela culatra.

    <meta name="description" content="Este é o blog do Alatazan, um katarense aprendendo Django." />

Esta acima define a descrição da página. Os mecanismos de busca (como o Google) a utilizam para sua descrição quando sua página é listada nos resultados de uma busca.

### Blocos "feeds", "style" e "scripts"

Cada um desses três será usado para um fim específico: Feeds, Estilos e JavaScripts, respectivamente.

### Divisões do layout

#### div "tudo"

Com esta **div**, criamos um contêiner capaz de abranger toda a página, assim podemos delimitar a página para ficar mais amigável a resoluções de tela menores, e também com uma aparência mais agradável.

#### div "topo"

É o novo contêiner para o topo da página, onde estará a identificação do site, presente em todas as páginas.

#### div "menu"

É o novo contêiner para o menu geral do site. dentro dela há uma **lista não-numerada** ( **&lt;ul&gt;** ), que tem cada um de seus itens ( **&lt;li&gt;** ) como sendo um item do menu.

#### div "corpo"

É o novo contêiner para o conteúdo da página.

Bom, agora que tudo foi esclarecido, vá ao navegador no seguinte endereço:

    http://localhost:8000/
    

E veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/html-e-css-layout-modificado-css-antigo/file/"/>
</p>

Bom, está na cara que não mudou muita coisa né? Mas agora vamos à mágica, e é aí que você precisa observar com maior atenção.

Da forma que estruturamos o **HTML**, podemos ajustar nosso layout para ficar assim:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/html-e-css-layout-da-pagina/file/"/>
</p>

Para fazê-lo, vá à pasta do projeto e abra a pasta **"media"**. Dali, abra o arquivo **"layout.css"** para edição.

### Trabalhando no estilo da página

Vamos adotar uma atitude radical: limpe o arquivo, **delete todo seu conteúdo**. Salve o arquivo, volte ao navegador e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/html-e-css-sem-css/file/"/>
</p>

Completamente sem estilo CSS, certo? Ok, agora acrescente o seguinte trecho de código ao arquivo de CSS:

    body {
      font-family: arial;
      background-color: #eee;
      color: #333;
    }
    
    h1 {
      margin: 10px 20px 10px 20px;
    }
    
    h2 {
      margin: 0;
      font-size: 1.2em;
    }

Salve o arquivo, atualize o navegador e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/html-e-css-css-parcial-1/file/"/>
</p>

Mudança pequena? Agora acrescente este trecho de código:

    /* Layout */
    
    #tudo {
      position: absolute;
      background-color: white;
      margin: 20px 0 20px -390px;
      width: 760px;
      left: 50%;
      border: 10px solid #aaa;
    }

Salve o arquivo, atualize o navegador e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/html-e-css-css-parcial-2/file/"/>
</p>

Agora já temos uma caixa fixa no centro, certo? Vamos agora acrescentar este trecho de código:

    #topo {
      font-size: 3em;
      margin: 20px;
    }
    
    #topo #foto {
      float: right;
      background-image: url(/media/foto.jpg);
      background-repeat: no-repeat;
      background-position: -20px -30px;
      width: 56px;
      height: 56px;
      border: 1px solid #47a;
    }

Salve o arquivo, atualize o navegador e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/html-e-css-css-parcial-3/file/"/>
</p>

Agora já temos o topo da página! Agora acrescente mais este:

    .corpo {
      margin: 20px 20px 0 20px;
    }
    
    .rodape {
      clear: both;
      overflow: hidden;
      background-color: #89a;
      margin: 20px;
      font-size: 0.8em;
      color: white;
      padding: 10px;
      text-align: center;
    }
    
    .rodape a {
      color: white;
    }

Salve o arquivo, atualize o navegador e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/html-e-css-css-parcial-4/file/"/>
</p>

Pronto, nosso **conteúdo** e **rodapé** também estão resolvidos! Agora acrescente mais este trecho de código ao CSS:

    /* Menu principal */
    
    #menu {
      clear: both;
      overflow: hidden;
      background-color: #47a;
      margin-top: 10px;
      height: 26px;
      font-size: 0.8em;
      font-weight: bold;
      color: white;
    }
    
    #menu ul {
      margin: 0;
      padding: 0;
    }
    
    #menu ul li {
      float: left;
      list-style: none;
      padding: 5px;
    }
    
    #menu ul li a:link, #menu ul li a:visited, #menu ul li a:active {
      padding: 5px;
      text-decoration: none;
      color: white;
    }
    
    #menu ul li a:hover {
      background-color: #79b;
      color: white;
    }

Salve o arquivo, atualize o navegador e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/html-e-css-css-parcial-5/file/"/>
</p>

Puxa, o nosso menu já virou outro! Agora acrescente mais esse trecho de código:

    /* Artigo do blog */
    
    .artigo h2 {
      text-decoration: underline;
    }
    
    .artigo {
      margin: 10px 0 10px 0;
    }
    
    .artigo a {
      color: black;
    }
    
    .conteudo {
      padding: 10px;
    }

Salve o arquivo, atualize o navegador e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/html-e-css-css-parcial-6/file/"/>
</p>

Notou a mudança no artigo?

Por fim, acrescente mais esse trecho:

    
    /* Formularios */
    
    form label {
      display: block;
    }
    
    textarea {
      height: 100px;
    }
    
    th {
      text-align: right;
      padding-right: 5px;
    }
    
    /* Comentarios */
    
    .comentarios {
      border-top: 1px solid silver;
      margin-top: 20px;
    }
    
    .comentarios hr {
      border-width: 0;
      height: 1px;
      border-top: 1px solid #ddd;
    }

Salve o arquivo. Feche o arquivo. Carregue no navegador o endereço do artigo:

    http://localhost:8000/artigo/1/
    

E veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/html-e-css-css-parcial-7/file/"/>
</p>

Pois bem, agora veja como está o nosso CSS depois de tudo:

    body {
      font-family: arial;
      background-color: #eee;
      color: #333;
    }
    
    h1 {
      margin: 10px 20px 10px 20px;
    }
    
    h2 {
      margin: 0;
      font-size: 1.2em;
    }
    
    /* Layout */
    
    #tudo {
      position: absolute;
      background-color: white;
      margin: 20px 0 20px -390px;
      width: 760px;
      left: 50%;
      border: 10px solid #aaa;
    }
    
    #topo {
      font-size: 3em;
      margin: 20px;
    }
    
    #topo #foto {
      float: right;
      background-image: url(/media/foto.jpg);
      background-repeat: no-repeat;
      background-position: -20px -30px;
      width: 56px;
      height: 56px;
      border: 1px solid #47a;
    }
    
    .corpo {
      margin: 20px 20px 0 20px;
    }
    
    .rodape {
      clear: both;
      overflow: hidden;
      background-color: #89a;
      margin: 20px;
      font-size: 0.8em;
      color: white;
      padding: 10px;
      text-align: center;
    }
    
    .rodape a {
      color: white;
    }
    
    /* Menu principal */
    
    #menu {
      clear: both;
      overflow: hidden;
      background-color: #47a;
      margin-top: 10px;
      height: 26px;
      font-size: 0.8em;
      font-weight: bold;
      color: white;
    }
    
    #menu ul {
      margin: 0;
      padding: 0;
    }
    
    #menu ul li {
      float: left;
      list-style: none;
      padding: 5px;
    }
    
    #menu ul li a:link, #menu ul li a:visited, #menu ul li a:active {
      padding: 5px;
      text-decoration: none;
      color: white;
    }
    
    #menu ul li a:hover {
      background-color: #79b;
      color: white;
    }
    
    /* Artigo do blog */
    
    .artigo h2 {
      text-decoration: underline;
    }
    
    .artigo {
      margin: 10px 0 10px 0;
    }
    
    .artigo a {
      color: black;
    }
    
    .conteudo {
      padding: 10px;
    }
    
    /* Formularios */
    
    form label {
      display: block;
    }
    
    textarea {
      height: 100px;
    }
    
    th {
      text-align: right;
      padding-right: 5px;
    }
    
    /* Comentarios */
    
    .comentarios {
      border-top: 1px solid silver;
      margin-top: 20px;
    }
    
    .comentarios hr {
      border-width: 0;
      height: 1px;
      border-top: 1px solid #ddd;
    }

### Os macetes mais importantes do CSS

Resumindo a história, os macetes mais importantes do CSS para quem está começando são:

* **Evite usar tabelas**, com todas as suas forças, para o que não é *de fato* uma matriz de dados tabulares. Abra mão disso somente quando não houver mais alternativas;

* Evite *padding* quando há um elemento dentro de outro. É melhor que o *padding* do elemento pai seja **zero** e a margem do elemento filho faça o trabalho do espaçamento;

* Use **overflow: hidden** para garantir que aquele elemento não ficará deformado quando algo dentro dele fizer aquela força danada para sair;

* Use **clear: both** para garantir que aquele elemento vai ficar sozinho na linha onde ele está posicionado.

* Use **float: left** ou **float: right** para posicionar um elemento à esquerda ou à direita respectivamente, ainda que ele não seja *inline*. E faça isso com ele antes dos demais elementos no código HTML. Esse é o segredo do menu horizontal em lista;

* Use unidade **"em"** para tamanhos de fonte;

* Caso queira exibir somente uma parte retangular de uma imagem, crie uma **div** com a imagem em *background*, depois posicione a imagem onde quiser.

### Ajustando o rodapé

Bom, agora que temos um CSS bem elaborado, vamos fazer um último ajuste. Na pasta **"templates"** da pasta do projeto, abra o arquivo **"rodape.html"** para edição e o modifique para ficar assim:

    <div class="rodape">
    Obrigado por passar por aqui. Volte sempre.
    </div>

Salve o arquivo. Feche o arquivo. Agora nosso rodapé tem uma cara um pouco mais de... **rodapé**!

## Satisfeito com o visual, mas de olho no Ballzer

Chegando em casa, Alatazan abria a porta quando uma bola metálica surgiu à sua frente, do nada!

A bola metálica tinha uma fileira de dentes, braços compridos, olhos esbugalhados e aquele sorriso habitual.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_6.gif" align="right"/>

- Tcharam! Poc poc! Ziiig poc poc!

Alatazan por fim confirmou o que vinha suspeitando há alguns dias. **Ballzer** estava ali, mais metálico do que nunca, e provavelmente aprontando.

Depois de dar um abraço destrambelhado em Alatazan, Ballzer foi ao fundo do quarto e trouxe um robozinho pequeno, desses feitos por pesquisadores do Planeta Terra. O robozinho não fazia muito mais do que girar para lá e para cá, e parecia que Ballzer havia adotado-o como mascote, boneca ou algo do gênero.

- Ballzer, onde você conseguiu isso?

No outro dia de manhã, desiludido e sem seu sorriso habitual, Ballzer levou Alatazan à casa de Cartola, de onde havia tirado o robô.

Depois da explicação constrangedora e sem-graça de Alatazan, Cartola parecia estar doido pra ele terminar com aquilo logo pra lhe mostrar algo mais interesse.

- Tá bom cara, mas olha, sabe em quê esse robô foi programado? Python, cara! **Python**! Eu consegui isso lá no laboratório da faculdade, com um cara meio maluco, e trouxe pra dar uma olhada. Vamos tirar um tempo hoje pra fuçar nisso e ver o que a gente aprende aqui!

<span class="no_print">
**Próximo capítulo: [Um pouco de Python agora](/um-pouco-de-python-agora/)**
</span>

