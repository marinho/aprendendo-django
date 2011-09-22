O **Instituto de Previsões de Longuíssimo Prazo Mesmo** de Katara, uma instituição respeitadíssima na galáxia, tem representantes em diversos planetas e usa seu imenso banco de dados para indicar tendências **econômicas**, **políticas** e **meteorológicas** para toda parte.

As áreas de previsão econômica e política são um sucesso e em geral são confirmadas com no máximo **5 séculos**.

Com base em informações desse instituto, Alatazan escolheu seu destino no Planeta Terra e também aprendeu **Português, Inglês e Espanhol** usando um moderníssimo método dinâmico de aprendizado de idiomas, que se basêia na teoria de que a cada 17 mil anos todo o conjunto de expressões de uma língua se repetem, mesmo.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_10.gif" align="right"/>

Como seu blog foi criado em **português**, ele tirou uma dessas últimas madrugadas que seus vizinhos fizeram festa pra refazer o site todo em **inglês e espanhol**, mas estava em dúvida de como faria pra deixar isso funcionando no site.

- Que zica é essa, Alatazan?

- Bom, é que eu quero que o site tenha três idiomas, e comecei a fazer outros templates pra adiantar o trabalho.

- Não cara, se a Nena estivesse aqui te daria um peteleco na cabeça...

- Mas o que há de errado aí? Eu até conferi no dicionário...

- Não, não... é que não é assim que se faz, vamos deletar isso tudo e começar do zero, você **complicou** as coisas...

## Conhecendo o suporte à internacionalização

Os recursos de **internacionalização** do Django só não fazem cair o queixo de dois tipos de pessoas: quem não a conhece e quem já se acostumou com ela.

Basicamente o que você precisa fazer é preparar o seu projeto com algumas práticas, e o restante o próprio Django faz, de forma transparente, sem precisar de URLs alternativas ou golpes de _ninja_.

### Preparando as settings do projeto

Então vamos começar pelas **definições** do projeto.

Na pasta do projeto, abra o arquivo **"settings.py"** para edição e localize a seguinte linha:

    LANGUAGE_CODE = 'pt-br'
    

Agora acrescente abaixo dela as seguintes linhas de código:

    LANGUAGES = (
        ('pt-br', u'Português'),
        ('en', u'Inglês'),
        ('es', u'Espanhol'),
    )

Isso foi feito porque queremos ter **3 idiomas** em nosso site: **Português**, **Inglês** e **Espanhol**.

Humm... mas tem um problema. Nos arquivos do Python, quando se quer informar qualquer caracter especial (como **"ê"** por exemplo) é preciso fazer uma coisinha: adicionar a seguinte linha ao início do arquivo:

    # -*- coding: utf-8 -*-
    

Isso é necessário porque o Python trabalha em diversos idiomas, e a codificação padrão não permite caracteres estranhos ao idioma **inglês**. Quando se acrescenta a linha acima no arquivo, o Python entende que esse arquivo deve ser interpretado no formato **Unicode**, então todos os caracteres especiais passam a serem suportados.

Agora localize a seguinte linha de código:

        'django.contrib.auth.middleware.AuthenticationMiddleware',
    

Acrescente esta linha abaixo dela:

        'django.middleware.locale.LocaleMiddleware',
    

E a setting **"MIDDLEWARE\_CLASSES"** passa a ficar assim:

    MIDDLEWARE_CLASSES = (
        'django.middleware.common.CommonMiddleware',
        'django.contrib.sessions.middleware.SessionMiddleware',
        'django.contrib.auth.middleware.AuthenticationMiddleware',
        'django.middleware.locale.LocaleMiddleware',
        'django.contrib.flatpages.middleware.FlatpageFallbackMiddleware',
    )

O middleware **"django.middleware.locale.LocaleMiddleware"** é quem faz a mágica de saber qual é o idioma usado ou preferido do usuário, e repassar essa informação ao restante do Django. Mas calma, nós vamos chegar lá.

Salve o arquivo. Feche o arquivo.

Agora execute o projeto - caso ele ainda não foi executado - clicando duas vezes sobre o arquivo **"executar.bat"** da pasta do projeto.

### Preparando os templates...

O próximo passo é ajustar os nossos templates para que sejam **internacionalizados**. Esse trabalho consiste em abrir cada um dos arquivos de template e fazer duas coisas:

1. Acrescentar a template tag **{% load i18n %}**;
1. Mudar as palavras que devem ser traduzidas para usarem a template tag **{% trans %}**

Então vamos começar pelo título do site, apenas ele...

Vá até a pasta **"templates"** da pasta do projeto e abra o arquivo **"base.html"** para edição. Localize a seguinte linha:

      <title>{% block titulo %}Blog do Alatazan{% endblock %}</title>
    

Modifique esta linha, para ficar assim:

      <title>{% block titulo %}{% trans "Blog do Alatazan" %}{% endblock %}</title>
    

Agora localize esta outra linha:

        Blog do Alatazan
    

Modifique-a para ficar assim:

        {% trans "Blog do Alatazan" %}
    

Salve o arquivo e abra o navegador na seguinte URL:

    http://localhost:8000/
    

Veja o que acontece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/i18n-invalid-trans/file/"/>
</p>

Temos uma mensagem de erro:

    TemplateSyntaxError at /
    Invalid block tag: 'trans'

E a mensagem de erro significa o que você deve estar pensando: a template tag **{% trans %}** é inválida. Não existe.

A template tag **{% trans %}** tem o papel de declarar um termo para que ele seja traduzido para o idioma escolhido pelo usuário. Portanto a seguinte expressão...

    {% trans "Blog do Alatazan" %}
    

...define que quando o idioma escolhido for o **inglês**, será exibido ali algo como **"Alatazan's Blog"**. Quando for **espanhol** será exibido **"Diário del Alatazan"** ou algo parecido, e assim por diante.

Mas essa template tag faz parte de uma **biblioteca para templates** que não está presente no sistema de templates padrão. Então é preciso carregar essa biblioteca. Para isso, acrescente a seguinte linha ao início do arquivo:

    {% load i18n %}
    

Salve o arquivo. Feche o arquivo. Volte ao navegador, pressione **F5** para atualizar e você pode ver que tudo voltou ao normal.

### Instalando Gettext no Windows

Agora que temos um termo para ser traduzido (**"Blog do Alatazan"**), precisamos escrever as traduções desse termo. Mas isso é feito em um lugar separado.

Quem trabalha com o **Windows** possui uma dificuldade a mais aqui. Porque na verdade o suporte à internacionalização do Django é baseado no **GNU Gettext** e depende dele. Este software é uma biblioteca fantástica que é **software livre** e é usada massivamente no Linux, Unix e outros sistemas operacionais. No Windows, ela deve ser instalada e o processo não é dos mais amigáveis, mas é bastante simples.

Primeiro, você deve ir até esta URL:

    http://sourceforge.net/projects/gettext
    

Clique na caixa **"Download"** e a seguinte página será mostrada:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/i18n-gettext-windows/file/"/>
</p>

Você deve clicar no primeiro link destacado em vermelho e localizar os seguintes arquivos para download:

- gettext-runtime-0.13.1.bin.woe32.zip
- gettext-tools-0.13.1.bin.woe32.zip

Feito o download, volte à página anterior e clique no segundo link destacado em vermelho. Localize o seguinte arquivo e faça o download:

- libiconv-1.9.1.bin.woe32.zip

Agora vá até a pasta de aplicativos instalados do HD (**c:\Arquivos de Programas**) e crie uma pasta chamada **"gettext-utils"**, para ficar assim:

    c:\Arquivos de Programas\gettext-utils
    

Ao concluir os downloads, você deve extrair os 3 arquivos para a nova pasta (todos para a mesma pasta). Os 3 arquivos possuem pastas como **"bin"**, **"lib"** e outros, mas não se preocupe, eles serão fundidos e misturados uns aos outros mesmo.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/i18n-instalando-gettext-no-windows/file/"/>
</p>

Agora o próximo passo é adicionar a nova pasta à variável de ambiente **PATH** do Windows. Para isso faça o seguinte:

1. Pressione as teclas **Windows + Pause** (a tecla Windows é a tecla do **logotipo** do Windows).
1. Na janela aberta, clique na aba **"Avançado"** e depois disso, no botão **"Variáveis de ambiente"**.
1. Na caixa de seleção **"Variáveis do Sistema"** clique duas vezes sobre o item **"Path"**.
1. E ao final do campo **"Valor da Variável"**, adicione um ponto-e-vírgula e o caminho da nova pasta onde instalamos o **gettext** mais **"\bin"** ao final, assim:

        ;c:\Arquivos de Programas\gettext-utils\bin
        

Depois disso, clique nos botões **"Ok"** até voltar para o estado anterior.

Pronto! Agora com o **Gettext** instalado, podemos seguir em frente.

### Usando makemessages para gerar arquivos de tradução

Agora voltando à pasta do projeto, crie um novo arquivo, chamado **"makemessages.bat"**, com o seguinte código dentro:

    python manage.py makemessages -l en
    python manage.py makemessages -l es
    pause

Esses comandos vão criar os arquivos de tradução para os idiomas **"en"** (Inglês) e **"es"** (Espanhol).

Salve o arquivo. Feche o arquivo.

Agora, ainda na pasta do projeto, precisamos criar a pasta onde esses arquivos serão gerados. Portanto crie uma nova pasta chamada **"locale"**.

E por fim, para gerar os arquivos de tradução, clique duas vezes sobre o arquivo **"makemessages.bat"**. Isso vai fazer com que o Django localize todas as strings marcadas para tradução (o que inclui a nossa **{% trans "Blog do Alatazan" %}**) e colocar nesses arquivos. Veja:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/i18n-makemessages/file/"/>
</p>

E veja como os arquivos foram criados pelo **"makemessages"**:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/i18n-arquivos-po-gerados/file/"/>
</p>

Agora você está descobrindo que o Django criou as pastas **"en"** e **"es"** com seus respectivos arquivos, e nós vamos editá-los para fazer a tradução dos termos que precisamos. Portanto, na pasta **"locale/en/LC\_MESSAGES"**, abra o arquivo **"django.po"** para edição, usando o **WordPad**, assim:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/i18n-editando-arquivo-po/file/"/>
</p>

A escolha pelo **WordPad** é pelo seguinte motivo: o Django é construído e usado principalmente por pessoas que usam e trabalham com **Linux ou MacOS X**. A forma como o Django gera arquivos segue os padrões desses dois sistemas operacionais em muitos aspectos, e este arquivo de traduções é um deles. Caso você abra usando o **Bloco de Notas**, o arquivo será todo exibido de uma forma distorcida, sem saltos de linha, e o WordPad é o editor compatível com esse formato que o Windows dispõe. Por isso fizemos escolha por ele.

Agora localize este trecho de código:

    #: .\templates\base.html.py:7
    msgid "Blog do Alatazan"
    msgstr ""

Modifique para ficar assim:

    #: .\templates\base.html.py:7
    msgid "Blog do Alatazan"
    msgstr "Alatazan's Blog"

Observe que alteramos somente o conteúdo do atributo **"msgstr"** e mantivemos o restante do bloco inalterado. Isso é importante, pois o **Gettext** localiza suas traduções com base nos termos identificados pelo atributo **"msgid"**.

Salve o arquivo. Feche o arquivo.

Agora vamos traduzir o mesmo termo para o idioma **espanhol**. Vá até a pasta **"locale/es/LC\_MESSAGES"** e abra o arquivo **"django.po"** e localize o mesmo bloco de código:

    #: .\templates\base.html.py:7
    msgid "Blog do Alatazan"
    msgstr ""

Modifique para ficar assim:

    #: .\templates\base.html.py:7
    msgid "Blog do Alatazan"
    msgstr "Diario del Alatazan"

Da mesma forma, modificamos somente o atributo **"msgstr"**.

Salve o arquivo. Feche o arquivo.

### Compilando os arquivos de tradução

Bom, não é só isso. Por questões de performance e outros motivos, é necessário compilar esses arquivos de extensão **.po** para gerar outros de extensão **.mo**, que serão de fato usados pelo Django.

Para isso, vá até a pasta do projeto e crie um novo arquivo chamado **"compilemessages.bat"**, com o seguinte código dentro:

    python manage.py compilemessages
    pause

Esse comando vai compilar os arquivos e gerar outros com extensão **.mo** para cada um dos **.po**.

Salve o arquivo. Feche o arquivo.

Agora clique duas vezes sobre ele veja o que acontece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/i18n-compilemessages/file/"/>
</p>

Como as mensagens indicam, os arquivos foram compilados com sucesso. Compilar os arquivos usando este comando é necessário para **todas as vezes** que os arquivos de tradução forem modificados.

Mas isso não é suficiente, pois a página permanece inalterada, e em **português**.

### Deixando que o usuário escolha seu idioma preferido

Pois agora vamos permitir que o usuário escolha o idioma do site como ele preferir. Para isso, vá até a pasta **"template"** da pasta do projeto e abra o arquivo **"base.html"**. Localize a seguinte linha:

       <div id="menu">
    

**Acima** dela, acrescente esta outra:

        {% include "idiomas.html" %}
    

Salve o arquivo. Feche o arquivo.

Agora na mesma pasta, crie um novo arquivo chamado **"idiomas.html"** com o seguinte código dentro:

    {% load i18n %}
    
    {% get_available_languages as LANGUAGES %}
    
    <div class="idiomas">
      <form action="/i18n/setlang/" method="post">
    
        <select name="language">
          {% for lang in LANGUAGES %}
          <option value="{{ lang.0 }}">{{ lang.1 }}</option>
          {% endfor %}
        </select>
    
        <input type="submit" value="{% trans "Mudar idioma" %}" />
      </form>
    </div>

Bom, o arquivo não é grande, mas precisamos esclarecer parte por parte...

Aqui nós carregamos a **biblioteca de internacionalização para templates**, com a qual já fizemos contato agora há pouco:

    {% load i18n %}
    

Agora nós carregamos todos os idiomas **disponíveis** no projeto. Você se lembra que nós declaramos a setting **"LANGUAGES"** lá no começo do capítulo? Pois elas serão usadas aqui agora:

    {% get_available_languages as LANGUAGES %}
    

O próximo passo é definir uma caixa e um **formulário manual** para o usuário selecionar seu idioma desejado:

    <div class="idiomas">
      <form action="/i18n/setlang/" method="post">

A seguir, temos o **campo de seleção** que vai dar ao usuário as opções de escolha do idioma. O nome desse campo é **"language"** e suas opções serão os três idiomas que determinamos na setting **"LANGUAGES"**:

        <select name="language">
          {% for lang in LANGUAGES %}
          <option value="{{ lang.0 }}">{{ lang.1 }}</option>
          {% endfor %}
        </select>

Aqui é preciso fazer uma observação. Veja as variáveis **{{ lang.0 }}** e **{{ lang.1 }}**.

No sistema de templates do Django não é permitido referenciar aos itens de uma lista da forma como fazemos no Python (observe as linhas com **uma\_lista[0]** e **uma\_lista[1]**:

    >>> uma_lista = ['sao paulo','rio','bh']
    >>> uma_lista
    ['sao paulo', 'rio', 'bh']
    >>> uma_lista[0]
    'sao paulo'
    >>> uma_lista[1]
    'rio'

Então, nas templates, a sintaxe muda e as chaves ( **[ ]** ) devem ser trocadas por um ponto **antes do número do item**, assim:

    {{ uma_lista.0 }}
    {{ uma_lista.1 }}

Voltando à settings **"LANGUAGES"**, veja como ela foi declarada no arquivo **"settings.py"**:

    LANGUAGES = (
        ('pt-br', u'Português'),
        ('en', u'Inglês'),
        ('es', u'Espanhol'),
    )

Se para cada volta do laço da template tag **{% for lang in LANGUAGES %}** a variável **"lang"** representa uma linha da setting **"LANGUAGES"**, então no primeiro item da setting **{{ lang.0 }}** contém **'pt-br'** e **{{ lang.1 }}** contém **u'Português'**. No segundo item, elas são **'en'** e **u'Inglês'** respectivamente, e assim por diante.

Por fim, temos aqui abaixo o **botão de envio** da escolha feita pelo usuário:

        <input type="submit" value="{% trans "Mudar idioma" %}" />
    

Salve o arquivo. Feche o arquivo.

Mas se fizemos referência à URL **/i18n/setlang/**, então temos aqui uma nova tarefa, certo?

### Declarando a URL de utilidades de internacionalização

Vá até a pasta do projeto, abra o arquivo **"urls.py"** para edição e acrescente a seguinte URL:

        (r'^i18n/', include('django.conf.urls.i18n')),
    

Agora o arquivo **"urls.py"** completo ficou assim:

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
        (r'^artigo/(?P<slug>[\w_-]+)/$', 'blog.views.artigo'),
        (r'^contato/$', 'views.contato'),
        (r'^comments/', include('django.contrib.comments.urls')),
        (r'^galeria/', include('galeria.urls')),
        (r'^tags/', include('tags.urls')),
        (r'^i18n/', include('django.conf.urls.i18n')),
    )
    
    if settings.LOCAL:
        urlpatterns += patterns('',
            (r'^media/(.*)$', 'django.views.static.serve',
             {'document_root': settings.MEDIA_ROOT}),
        )

Salve o arquivo. Feche o arquivo.

Agora volte ao navegador e carregue a URL principal do projeto:

    http://localhost:8000/
    

Veja como a página é carregada:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/i18n-pagina-inicial-com-form/file/"/>
</p>

Opa! Agora temos um formulário ali, para a escolha do idioma! Escolha o idioma **"Inglês"**, clique sobre o botão e veja como se sai:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/i18n-pagina-inicial-em-ingles/file/"/>
</p>

Pronto! Estamos navegando em inglês!

Uma vez que um idioma é selecionado, a opção escolhida é **armazenada na sessão**, e o idioma será trocado novamente somente quando a sessão expirar ou quando o usuário o fizer da mesma forma que você fez agora.

Além disso, quando o usuário entra pela primeira vez no site e enquanto não fez nenhuma escolha, o Django tenta se adequar ao idioma do navegador. Portanto, caso um usuário acesse seu site partindo de um navegador em espanhol, ele estará **automaticamente** navegando **em espanhol**!

Agora troque novamente o idioma, mas desta vez para **espanhol**. Veja:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/i18n-pagina-inicial-em-espanhol/file/"/>
</p>

Agora apenas carregue outra URL... a do **Admin**, por exemplo:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/i18n-admin-em-espanhol/file/"/>
</p>

_Arriba muchacho! Estamos hablando em español!_

Mas você notou que o quadro destacado da aplicação **"Galeria"** tem uma palavra que não está em **espanhol**?

### Usando a função de tradução em código Python: ugettext\_lazy()

De acordo com o tradutor do Google, a palavra **"Imagems"** deveria ser **"Cuadros"**, isso sem falar que **"Imagems"** já está errada mesmo em português!

Então, vamos fazer assim: vá até a pasta da aplicação **"galeria"**, abra o arquivo **"models.py"** para edição e localize o seguinte bloco de código:

        class Meta:
            ordering = ('album','titulo',)

Acrescente estas novas linhas dentro dele:

            verbose_name = ugettext_lazy('Imagem')
            verbose_name_plural = ugettext_lazy('Imagens')

Observe que a primeira linha que incluímos aponta um **nome amigável** para essa classe de modelo, **no singular**, usando o atributo **"verbose\_name"**. Mas além disso, ele também atribui a palavra **"Imagem"** dentro da função **"ugettext\_lazy()"**, que equivale à template tag **{% trans %}**, mas para ser usada em código Python. Ou seja, estamos dizendo aí que a palavra **"Imagem"** deve ser traduzida:

            verbose_name = ugettext_lazy('Imagem')
    

A segunda linha tem o mesmo papel, mas para o caso de **nome amigável no plural**:

            verbose_name_plural = ugettext_lazy('Imagens')
    

Agora aquele bloco que localizamos e modificamos está assim:

        class Meta:
            ordering = ('album','titulo',)
            verbose_name = ugettext_lazy('Imagem')
            verbose_name_plural = ugettext_lazy('Imagens')

Mas como estamos usando a função **"ugettext\_lazy()"**, devemos importá-la de algum lugar, certo? Pois então vá até o ínicio do arquivo e localize esta linha:

    from django.core.urlresolvers import reverse
    

Agora acrescente esta outra abaixo dela:

    from django.utils.translation import ugettext_lazy
    

Salve o arquivo. Feche o arquivo. Agora precisamos gerar novamente os arquivos de tradução, usando o utilitário **"makemessages.bat"** da pasta do projeto.

Depois que executar o **"makemessages.bat"**, vá até a pasta **"locale/en/LC\_MESSAGES"**, partindo da pasta do projeto, e abra o arquivo **"django.po"**. Veja que agora ele já tem mais coisas:

    # SOME DESCRIPTIVE TITLE.
    # Copyright (C) YEAR THE PACKAGE'S COPYRIGHT HOLDER
    # This file is distributed under the same license as the PACKAGE package.
    # FIRST AUTHOR <EMAIL@ADDRESS>, YEAR.
    #
    #, fuzzy
    msgid ""
    msgstr ""
    "Project-Id-Version: PACKAGE VERSION\n"
    "Report-Msgid-Bugs-To: \n"
    "POT-Creation-Date: 2008-12-09 18:49-0200\n"
    "PO-Revision-Date: YEAR-MO-DA HO:MI+ZONE\n"
    "Last-Translator: FULL NAME <EMAIL@ADDRESS>\n"
    "Language-Team: LANGUAGE <LL@li.org>\n"
    "MIME-Version: 1.0\n"
    "Content-Type: text/plain; charset=UTF-8\n"
    "Content-Transfer-Encoding: 8bit\n"
    
    #: .\galeria\models.py:28
    msgid "Imagem"
    msgstr ""
    
    #: .\galeria\models.py:29
    msgid "Imagens"
    msgstr ""
    
    #: .\templates\base.html.py:7 .\templates\base.html.py:33
    msgid "Blog do Alatazan"
    msgstr "Alatazan's Blog"
    
    #: .\templates\idiomas.html.py:14
    msgid "Mudar idioma"
    msgstr ""

Faça a tradução dos termos nos atributos **"msgstr"**, para ficar assim:

    # SOME DESCRIPTIVE TITLE.
    # Copyright (C) YEAR THE PACKAGE'S COPYRIGHT HOLDER
    # This file is distributed under the same license as the PACKAGE package.
    # FIRST AUTHOR <EMAIL@ADDRESS>, YEAR.
    #
    #, fuzzy
    msgid ""
    msgstr ""
    "Project-Id-Version: PACKAGE VERSION\n"
    "Report-Msgid-Bugs-To: \n"
    "POT-Creation-Date: 2008-12-09 18:49-0200\n"
    "PO-Revision-Date: YEAR-MO-DA HO:MI+ZONE\n"
    "Last-Translator: FULL NAME <EMAIL@ADDRESS>\n"
    "Language-Team: LANGUAGE <LL@li.org>\n"
    "MIME-Version: 1.0\n"
    "Content-Type: text/plain; charset=UTF-8\n"
    "Content-Transfer-Encoding: 8bit\n"
    
    #: .\galeria\models.py:28
    msgid "Imagem"
    msgstr "Picture"
    
    #: .\galeria\models.py:29
    msgid "Imagens"
    msgstr "Pictures"
    
    #: .\templates\base.html.py:7 .\templates\base.html.py:33
    msgid "Blog do Alatazan"
    msgstr "Alatazan's Blog"
    
    #: .\templates\idiomas.html.py:14
    msgid "Mudar idioma"
    msgstr "Change language"

Salve o arquivo. Feche o arquivo.

Agora faça o mesmo com o arquivo **"django.po"** da pasta **"locale/en/LC\_MESSAGES"**:

    # SOME DESCRIPTIVE TITLE.
    # Copyright (C) YEAR THE PACKAGE'S COPYRIGHT HOLDER
    # This file is distributed under the same license as the PACKAGE package.
    # FIRST AUTHOR <EMAIL@ADDRESS>, YEAR.
    #
    #, fuzzy
    msgid ""
    msgstr ""
    "Project-Id-Version: PACKAGE VERSION\n"
    "Report-Msgid-Bugs-To: \n"
    "POT-Creation-Date: 2008-12-09 18:49-0200\n"
    "PO-Revision-Date: YEAR-MO-DA HO:MI+ZONE\n"
    "Last-Translator: FULL NAME <EMAIL@ADDRESS>\n"
    "Language-Team: LANGUAGE <LL@li.org>\n"
    "MIME-Version: 1.0\n"
    "Content-Type: text/plain; charset=UTF-8\n"
    "Content-Transfer-Encoding: 8bit\n"
    
    #: .\galeria\models.py:28
    msgid "Imagem"
    msgstr ""
    
    #: .\galeria\models.py:29
    msgid "Imagens"
    msgstr ""
    
    #: .\templates\base.html.py:7 .\templates\base.html.py:33
    msgid "Blog do Alatazan"
    msgstr "Diario del Alatazan"
    
    #: .\templates\idiomas.html.py:14
    msgid "Mudar idioma"
    msgstr ""

Modifique traduzindo os termos para ficar assim:

    # SOME DESCRIPTIVE TITLE.
    # Copyright (C) YEAR THE PACKAGE'S COPYRIGHT HOLDER
    # This file is distributed under the same license as the PACKAGE package.
    # FIRST AUTHOR <EMAIL@ADDRESS>, YEAR.
    #
    #, fuzzy
    msgid ""
    msgstr ""
    "Project-Id-Version: PACKAGE VERSION\n"
    "Report-Msgid-Bugs-To: \n"
    "POT-Creation-Date: 2008-12-09 18:49-0200\n"
    "PO-Revision-Date: YEAR-MO-DA HO:MI+ZONE\n"
    "Last-Translator: FULL NAME <EMAIL@ADDRESS>\n"
    "Language-Team: LANGUAGE <LL@li.org>\n"
    "MIME-Version: 1.0\n"
    "Content-Type: text/plain; charset=UTF-8\n"
    "Content-Transfer-Encoding: 8bit\n"
    
    #: .\galeria\models.py:28
    msgid "Imagem"
    msgstr "Cuadro"
    
    #: .\galeria\models.py:29
    msgid "Imagens"
    msgstr "Cuadros"
    
    #: .\templates\base.html.py:7 .\templates\base.html.py:33
    msgid "Blog do Alatazan"
    msgstr "Diario del Alatazan"
    
    #: .\templates\idiomas.html.py:14
    msgid "Mudar idioma"
    msgstr "Cambiar de idioma"

Salve o arquivo. Feche o arquivo.

Agora é preciso **compilar** os arquivos de tradução novamente. Faça isso clicando duas vezes sobre o arquivo **"compilemessages.bat"** da pasta do projeto. Acompanhe as mensagens de erro para se certificar de que tudo ocorreu corretamente.

Agora volte ao navegador, na URL do Admin, atualize com **F5** e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/i18n-admin-em-espanhol-resolvido/file/"/>
</p>

E voltando a navegar no site na URL de **Contato**:

    http://localhost:8000/contato/
    

Veja como se sai:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/i18n-outro-termo-traduzido-em-espanhol/file/"/>
</p>

Temos o **"Cambiar de idioma"** funcionando perfeitamente! Mas como vemos, nosso **formulário de contato** também deveria ser traduzido. Pois vamos fazer isso!

### Usando a função ugettext()

Vá até a pasta do projeto, abra para edição o arquivo **"views.py"** e localize o seguinte bloco de código:

    class FormContato(forms.Form):
        nome = forms.CharField(max_length=50)
        email = forms.EmailField(required=False)
        mensagem = forms.Field(widget=forms.Textarea)

Ele deve ser ajustado para ficar assim:

    class FormContato(forms.Form):
        nome = forms.CharField(max_length=50, label=ugettext('Nome'))
        email = forms.EmailField(required=False, label=ugettext('E-mail'))
        mensagem = forms.Field(widget=forms.Textarea, label=ugettext('Mensagem'))

Observe que acrescentamos a função **"ugettext()"** para cada um dos campos do formulário.

Agora acrescente a seguinte linha ao início do arquivo:

    from django.utils.translation import ugettext
    

A questão agora é: porquê usamos a função **"ugettext\_lazy()"** no arquivo **"models.py"** e agora usamos a função **"ugettext()"** no arquivo **"views.py"**?

A função **"ugettext\_lazy()"** é **preguiçosa**. Isso significa que a tradução é feita somente quando ela é requisitada, o que é relativamente melhor para o caso de classes de modelo, pois elas são constantemente utilizadas sem que a tradução de um termo seja necessário de fato.

Por outro lado a função **"ugettext()"** traduz a string instantaneamente, o que é melhor para casos como o de formulários dinâmicos e *views*, pois eles não são usados de maneira tão constante quanto classes de modelo.

Mas para não deixar o desenvolvedor confuso, há uma truque que facilita muito toda essa mistura de coisas.

### Usando \_() ao invés de ugettext() ou ugettext\_lazy()

Localize a linha que você acabou de inserir:

    from django.utils.translation import ugettext
    

Modifique para ficar assim:

    from django.utils.translation import ugettext as _
    

Quando se usa o operador **"as"** numa importação do Python, você está fazendo aquela importação e dando um apelido ao pacote ou elemento importado. Então neste caso a função **"ugettext()"** é importada, mas seu nome neste módulo do Python será apenas um sublinhado, assim: **"\_()"**.

Agora vá até este bloco de código:

    class FormContato(forms.Form):
        nome = forms.CharField(max_length=50, label=ugettext('Nome'))
        email = forms.EmailField(required=False, label=ugettext('E-mail'))
        mensagem = forms.Field(widget=forms.Textarea, label=ugettext('Mensagem'))

E troque as palavras **"ugettext"** por **"\_"**, assim

    class FormContato(forms.Form):
        nome = forms.CharField(max_length=50, label=_('Nome'))
        email = forms.EmailField(required=False, label=_('E-mail'))
        mensagem = forms.Field(widget=forms.Textarea, label=_('Mensagem'))

Esse truque faz com que seu código fique mais limpo e facilita uma eventual mudança de **"ugettext"** pela **"ugettext\_lazy"**, quando for necessário.

Salve o arquivo. Feche o arquivo. Volte ao navegador e atualize a página com **F5**. Veja agora:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/i18n-pagina-de-contato-com-termos-traduzidos/file/"/>
</p>

É isso aí moçada! Como você deve perceber, há muito mais termos para traduzir em nosso projeto. Que tal fazer o restante agora por conta própria?

## Partindo para conhecer melhor o Admin

- É fantástico! É tão transparente e coeso que até assusta!

- Sim, meu caro, e veja só:

 - Primeiro você preparou as settings do project, declarando a setting **LANGUAGES** e adicionando o middleware **"django.middleware.locale.LocaleMiddleware"**;
 - Depois você ajustou alguns templates, usando a biblioteca **"i18n"** e a template tag **{% trans %}**;
 - Aí você instalou o **Gnu Gettext** nessa coisa aí - isso não aconteceria no meu **MacOSX**;
 - Criou dois arquivos de comandos para executar o **"makemessages"** e o **"compilemessages"**;
 - Abriu os arquivos **.po** e traduziu o conteúdo dos atributos **"msgstr"**;
 - Compilou os arquivos;
 - Criou um formulário lá no topo do blog para o usuário escolher um idioma;
 - Declarou a URL **"^i18n/"**;
 - Conheceu as funções **"ugettext\_lazy()"** e **"ugettext()"**, e descobriu que a primeira é **preguiçosa**, e a segunda é **proativa**... mas às vezes a preguiçosa é melhor;
 - E também agora sabe que usar um apelido **"\_()"** para elas facilita a sua vida;
 - E de bônus, no final, descobriu que alguns termos já são traduzidos antes que você o faça!

- Muito legal, Cartola... impressionante...

Os dois continuaram a conversar por um tempo, e Alatazan estava só um pouco preocupado com sua organização em sua vida pessoal, pois algumas contas se atrasavam e ele se esquecia disso...

<span class="no_print">
**Próximo capítulo: [Fazendo um sistema de Contas Pessoais](/fazendo-um-sistema-de-contas-pessoais/)**
</span>

