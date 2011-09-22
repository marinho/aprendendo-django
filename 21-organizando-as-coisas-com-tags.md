O céu de Katara é pintado de cores **variadas**.

Os antepassados acreditavam que no alto da montanha mais alta, havia um alto senhor, de olhos altivos, cabelos alongados, barbas brancas e compridas, que calçava sandálias brancas e usava um vestido também comprido e branco.

Um dia aquele alto homem, de olhos altivos, cabelos alongados, barbas brancas e compridas, que calçava sempre as mesmas sandálias das mesmas cores e tudo muito branco, alto e comprido, se encheu de tudo isso e comprou **óculos coloridos**.

Ninguém nunca soube explicar onde ele encontrou aqueles óculos coloridos para comprar, mas como a história fala sobre a altivez do homem e na virada que isso causou em sua vida e sua coluna, isso não nos importa agora.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_13.gif" align="right"/>

O que realmente importa é que naquele dia o homem se rebelou e criou uma imensa tecnologia, dotada de jatos ultra-potentes de tinta, que fizeram o céu se colorir e o povo das planícies protestar contra a quantidade de tinta que caía lá embaixo.

Dizem que é por isso que todos os katarenses hoje sabem que entre o branco e o preto há **muitas cores**, e algumas delas podem estar misturadas em sua cabeça. Não se esqueça de lavar com shapoo Ultrapoo Limpadon, aquele que espalha, mistura e lustra, e depois que seca, também brilha.

Mas o sabão que o Alatazan deu no Ballzer não foi suficiente. Eles precisavam organizar agora tudo novamente em seus **devidos assuntos**... e na vida real, não é possível ter um mesmo livro em **duas seções ao mesmo tempo**... por sorte, no site seria mais fácil...

## Separando as coisas com etiquetas variadas

**Tags** são etiquetas, uma forma de organizar as informações na Web que foi massificada pela Web2.0, especialmente pelo site **del.icio.us**.

A idéia de usar *tags* geralmente é assim: um campo de texto simples onde o usuário informa as **palavras-chave** - as tags - daquele objeto, quantas palavras ele quiser informar. Ao salvar, o objeto é vinculado a todas aquelas palavras, e então através de outra página é possível ver todas essas Tags, clicar sobre elas e saber quais objetos estão ligados a elas.

Nós podemos ter *tags* em **artigos do blog** e em **imagens da galeria**. Com isso, as aplicações **"blog"** e **"galeria"** passam a ser dependentes da nova aplicação que vamos criar: **"tags"**. Mas por outro lado, a aplicação **"tags"** não será dependente de ninguém.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/aplicacoes-dependentes-de-tags/file/"/>
</p>

Então, *let's go*!

### Criando a nova aplicação de tags

Na pasta do projeto, crie uma pasta para a nova aplicação, chamada **"tags"** e dentro dela crie um arquivo vazio chamado **"\_\_init\_\_.py"**.

Agora crie um novo arquivo chamado **"models.py"**, com o seguinte código dentro:

    from django.db import models
    from django.contrib.contenttypes.models import ContentType
    
    class Tag(models.Model):
        nome = models.CharField(max_length=30, unique=True)
    
        def __unicode__(self):
            return self.nome
    
    class TagItem(models.Model):
        class Meta:
            unique_together = ('tag', 'content_type', 'object_id')
        
        tag = models.ForeignKey('Tag')
        content_type = models.ForeignKey(ContentType)
        object_id = models.PositiveIntegerField(db_index=True)

Você pode ver que temos duas classes de modelo: **"Tag"** e **"TagItem"**, e que a classe **"TagItem"** está relacionada à classe **"Tag"** de forma que uma **tag** pode conter muitos **itens**.

Mas você agora vai descobrir essa **importação** diferente do que aprendemos até aqui:

    from django.contrib.contenttypes.models import ContentType
    

A aplicação de _contrib_ **"contenttypes"** tem a função de trabalhar com **tipos dinâmicos**, ou melhor dizendo: ela permite que tipos **que não se conhecem** sejam relacionados uns aos outros.

Na prática, usando a classe de modelo que importamos na linha acima ( **"ContentType"** ) é possível ter um **artigo** do **blog** vinculado a uma **tag** sem que eles se conheçam, mantendo uma dependência mínima, apenas em um lugar do código-fonte, sem precisar de alterar o banco de dados para isso.

A seguinte linha possui um argumento importante: **"unique=True"**. Ele vai garantir que não teremos tags **duplicadas**:

        nome = models.CharField(max_length=30, unique=True)
    

Já a linha abaixo tem o papel de garantir que a classe de modelo (neste caso, **"TagItem"**) terá somente um único objeto para cada combinação dos três campos informados: **"tag"**, **"content\_type"** e **"object\_id"**.

Por exemplo: um objeto de **"TagItem"** com a tag **"livros"**, o content\_type **"blog.artigo"** e o object\_id **15** só pode existir uma única vez.

            unique_together = ('tag', 'content_type', 'object_id')
    

Agora observe este trecho de código abaixo. Traduzindo, a combinação desses campos com o campo **"tag"** permite que seja possível ligar uma **tag** a qualquer objeto do projeto.

        content_type = models.ForeignKey(ContentType)
        object_id = models.PositiveIntegerField(db_index=True)

O campo **"content\_type"** representa um **ContentType** - o tipo dinâmico. Ele pode ser **"blog.artigo"** ( classe **"Artigo"** da aplicação **"blog"** ) ou então **"galeria.imagem"** ( classe **"Imagem"** da aplicação **"galeria"** ). Na verdade pode ser qualquer outro tipo que exista no projeto.

E já o campo **"object\_id"** representa o código **id** do objeto relacionado.

Exemplo:

- tag: **"livro"**
- content\_type: **"blog.artigo"**
- object\_id: **15**

O exemplo acima indica que o **artigo do blog** de id **15** possui a tag **"livro"**.

O argumento **"db\_index=True"** cria um índice no banco de dados para o campo **"object\_id"**, o que deve tornar as nossas buscas mais rápidas quando o relacionamento for feito diretamente a ele.

Salve o arquivo. Feche o arquivo.

Agora precisamos instalar a nova aplicação no projeto, certo? Então na pasta do projeto, abra o arquivo **"settings.py"** para edição e acrescente a seguinte linha à setting **"INSTALLED\_APPS"**:

        'tags',
    

Agora a setting que modificamos ficou assim:

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
        'galeria',
        'tags',
    )

Salve o arquivo. Feche o arquivo.

Clique duas vezes no arquivo **gerar\_banco\_de\_dados.bat** para gerar as novas tabelas no banco de dados. O resultado será este:

    Creating table tags_tag
    Creating table tags_tagitem
    Installing index for tags.Tag model
    Installing index for tags.TagItem model

Feche a janela do **MS-DOS**.

Agora vamos modificar a aplicação **"blog"** para que a classe **"Artigo"** permita a entrada de **tags**. Para isso, vá até a pasta da aplicação **"blog"** e abra o arquivo **"admin.py"** para edição. Logo abaixo da primeira linha, acrescente o trecho de código abaixo:

    from django.contrib.admin.options import ModelAdmin
    from django import forms

Você já conhece essas duas importações. Lembra-se do último capítulo? A classe **"ModelAdmin"** permite a criação de um Admin mais elaborado para uma classe. Já o pacote **"forms"** permite a criação de **formulários dinâmicos**, e o ModelAdmin faz uso deles!

Agora encontre esta outra linha:

    from models import Artigo
    

Acrescente abaixo dela o seguinte trecho de código:

    class FormArtigo(forms.ModelForm):
        class Meta:
            model = Artigo
    
        tags = forms.CharField(max_length=30, required=False)

Esse formulário dinâmico vai representar a entrada de dados na classe ModelAdmin de **"Artigo"**, e além de todas as atribuições que ele já possui, ele vai ter um campo **calculado** a mais, chamado **"tags"**.

Campos calculados são campos que não existem de fato no banco de dados, mas que são úteis para alguma coisa ser tratada em **memória**.

Agora abaixo do trecho de código que escrevemos, acrescente mais este:

    class AdminArtigo(ModelAdmin):
        form = FormArtigo

Aqui nós demos vida ao formulário dinâmico **"FormArtigo"**, vinculando-o ao ModelAdmin que será registrado para a classe **"Artigo"**.

E por fim, **modifique** a última linha:

    admin.site.register(Artigo)
    

Para ficar assim:

    admin.site.register(Artigo, AdminArtigo)
    
Com as mudanças que fizemos, o arquivo **"admin.py"** ficou assim:

    from django.contrib import admin
    from django.contrib.admin.options import ModelAdmin
    from django import forms
    
    from models import Artigo
    
    class FormArtigo(forms.ModelForm):
        class Meta:
            model = Artigo
    
        tags = forms.CharField(max_length=30, required=False)
    
    class AdminArtigo(ModelAdmin):
        form = FormArtigo
    
    admin.site.register(Artigo, AdminArtigo)

Salve o arquivo. Feche o arquivo.

Agora vamos executar o projeto para ver o efeito do que fizemos. Na pasta do projeto, execute o arquivo **"executar.bat"**, abra seu navegador e carregue a seguinte URL:

    http://localhost:8000/admin/blog/artigo/1/
    

Essa URL vai abrir o **artigo** de código **1** no Admin. Veja:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/tags-campo-tags-sem-efeito/file/"/>
</p>

No entanto, ao informar algum valor ali e **salvar**, isso não terá efeito nenhum, porque trata-se de um campo de memória, sem uma representação **persistente** no banco de dados.

Para resolver isso, volte a abrir o arquivo **"admin.py"** da aplicação **"blog"** para edição e localize a seguinte linha:

        form = FormArtigo
    

Abaixo dela, acrescente o seguinte trecho de código:

        def save_model(self, request, obj, form, change):
            super(AdminArtigo, self).save_model(request, obj, form, change)
    
            aplicar_tags(obj, form.cleaned_data['tags'])

O método **"save\_model()** da classe **ModelAdmin** é chamado toda vez que o **Admin** salva um objeto. Você já sabe que a função **"super()"** executa o método como foi originalmente declarado pelo próprio Django, ou seja, o **método sobreposto**, pois não queremos interferir no que ele já fazia, queremos apenas acrescentar uma funcionalidade a mais.

E a seguinte linha é novidade para você:

            aplicar_tags(obj, form.cleaned_data['tags'])
    

Esta linha chama a função **"aplicar\_tags()"**, passando como seus argumentos: o artigo salvo e o conteúdo do campo **"tags"**, informado pelo usuário.

Todo formulário dinâmico possui o atributo **"cleaned_data"**, que é um dicionário que traz todos os campos do formulário, contendo o valor informado pelo usuário.

Já quanto à função **"aplicar\_tags()"**, nós vamos importá-la agora mesmo da aplicação **"tags"**. Para fazer isso, localize a seguinte linha:

    from models import Artigo
    

E acrescente esta abaixo dela:

    from tags import aplicar_tags
    

Agora após essas modificações, o arquivo **"admin.py"** ficou assim:

    from django.contrib import admin
    from django.contrib.admin.options import ModelAdmin
    from django import forms
    from models import Artigo
    from tags import aplicar_tags, tags_para_objeto
    
    class FormArtigo(forms.ModelForm):
        class Meta:
            model = Artigo
    
        tags = forms.CharField(max_length=30, required=False)
    
        def __init__(self, *args, **kwargs):
            super(FormArtigo, self).__init__(*args, **kwargs)
    
            if self.instance.id:
                self.initial['tags'] = tags_para_objeto(self.instance)
    
    class AdminArtigo(ModelAdmin):
        form = FormArtigo
    
        def save_model(self, request, obj, form, change):
            super(AdminArtigo, self).save_model(request, obj, form, change)
    
            aplicar_tags(obj, form.cleaned_data['tags'])
    
    admin.site.register(Artigo, AdminArtigo)

Salve o arquivo. Feche o arquivo.

Agora precisamos criar a função **"aplicar\_tags()"**, né?

Vá até a pasta da aplicação **"tags"** e abra o arquivo **"\_\_init\_\_.py"** para edição. Acrescente as seguintes linhas a ele:

    from models import Tag, TagItem
    from django.contrib.contenttypes.models import ContentType
    
    def aplicar_tags(obj, tags):
        tipo_dinamico = ContentType.objects.get_for_model(obj)
        
        TagItem.objects.filter(
            content_type=tipo_dinamico,
            object_id=obj.id,
            ).delete()
    
        tags = tags.split(' ')
        for tag_nome in tags:
            tag, nova = Tag.objects.get_or_create(nome=tag_nome)
    
            TagItem.objects.get_or_create(
                tag=tag,
                content_type=tipo_dinamico,
                object_id=obj.id,
                )

Observe que a classe **"Artigo"** não é informada, nem sequer a aplicação **"blog"** é citada.

Pois vamos agora analisar o que fizemos por partes:

As duas importações abaixo, você já conhece: são as classes das quais precisamos para trabalhar a função que vai **aplicar as tags** informadas no formulário do **artigo** ou de qualquer outro objeto.

    from models import Tag, TagItem
    from django.contrib.contenttypes.models import ContentType

Aqui nós declaramos a função. Ela possui dois argumentos: o **objeto** e as **tags** que serão aplicadas a ele:

    def aplicar_tags(obj, tags):
    

A próxima coisa que fazemos é carregar o **tipo dinâmico** para o objeto que estamos tratando. É aqui que o Django descobre se o objeto é **Artigo**, **Imagem** ou seja lá o que for. O valor atribuído à variável **"tipo\_dinamico"** será o **ContentType** que representa a classe de modelo do objeto informado pela variável **"obj"**:

        tipo_dinamico = ContentType.objects.get_for_model(obj)
    

Após carregar o tipo dinâmico para o objeto que estamos trabalhando, é a hora de **excluir** todas as tags que o objeto possui. As linhas a seguir carregam todos os objetos de **"TagItem"** do tipo dinâmico do objeto e de seu código ( **"obj.id"** ). Depois disso, exclui a todos com o método **".delete()"**:

        TagItem.objects.filter(
            content_type=tipo_dinamico,
            object_id=obj.id,
            ).delete()

Fazemos isso porque um pouco mais pra baixo vamos criá-las novamente, garantindo que ficarão para o objeto exatamente as tags da variável **"tags"**.

Mas **atenção**: o trecho de código acima não exclui as tags em si, mas sim o vínculo delas com o objeto.

Agora, na linha baixo, nós **separamos** por espaços ( **' '** ) a string das tags informadas pelo usuário. Ou seja, se o usuário houver informado **"arte brasil"**, o resultado será **['arte', 'brasil']**:

        tags = tags.split(' ')
    

Logo a seguir, fazemos um laço nas tags, ou seja, no caso citado acima ( **['arte', 'brasil']** ), esse laço irá executar seu bloco duas vezes: uma para a palavra **"arte"**, outra para a palavra **"brasil"**. A string da tag na execução do bloco está contida na variável **"tag\_nome"**:

        for tag_nome in tags:
    

Agora, dentro do bloco, a linha abaixo tenta carregar um objeto do banco de dados para a tag atual dentro do laço. Se não existir no banco de dados, ela então é criada, e a variável **"nova"** é atribuída com valor **True**. Isso é feito especificamente pelo método **"get\_or\_create()"**. Já a variável **"tag"** recebe o a tag carregada do banco de dados, seja ela nova ou não:

            tag, nova = Tag.objects.get_or_create(nome=tag_nome)
    

E por fim, temos o bloco que cria o vínculo de cada tag com o objeto, também seguindo a mesma lógica de pensamento, criando somente se existir, para evitar um eventual conflito:

            TagItem.objects.get_or_create(
                tag=tag,
                content_type=tipo_dinamico,
                object_id=obj.id,
                )

Puxa, quanto falatório!

Salve o arquivo. Feche o arquivo. Volte ao navegador, na URL do artigo no Admin:

    http://localhost:8000/admin/blog/artigo/1/
    

Informe as tags **"arte brasil"** e clique no botão de **"Salvar e continuar editando"**. No entanto, vai perceber que não adiantou nada, ele parece não salvar.

Acontece que o nosso formulário dinâmico **"FormArtigo"** possui o tratamento para **salvar** o campo **"tags"** usando a função **"aplicar\_tags"**, mas não possui o tratamento para **carregá-las** depois de salvo. Percebeu a diferença? Nós estamos gravando no banco de dados, mas isso não está sendo mostrado na tela, e como o campo **"tags"** é um campo calculado, ele vai mostrar somente aquilo que nós, manualmente, definirmos para ele mostrar.

Portanto, agora volte ao arquivo **"admin.py"** da aplicação **"blog"** para edição, e localize esta linha:

        tags = forms.CharField(max_length=30, required=False)
    

Abaixo dela, acrescente o seguinte trecho de código:

        def __init__(self, *args, **kwargs):
            super(FormArtigo, self).__init__(*args, **kwargs)

            if self.instance.id:
                self.initial['tags'] = tags_para_objeto(self.instance)

Este método, que é o **inicializador** do formulário dinâmico, é executado toda vez que o formulário é instanciado, seja para exibir dados na tela, seja para salvar os dados informados pelo usuário. E é exatamente aqui, depois de executar o inicializador da classe herdada ( com a função **"super()"** ) que ele carrega o valor para o campo calculado **"tags"**:

            if self.instance.id:
                self.initial['tags'] = tags_para_objeto(self.instance)

A linha acima tem o seguinte significado: se o formulário possui uma **instância** de objeto, o valor **inicial** para o campo **"tags"** deve ser o que é retornado pela função **"tags\_para\_objeto()"**.

E falando na função, ainda precisamos trazê-la de algum lugar, certo? Localize a seguinte linha:

    from tags import aplicar_tags
    

E a modifique, para ficar assim:

    from tags import aplicar_tags, tags_para_objeto
    

Pronto, agora desta forma o Admin da classe **"Artigo"** deve funcionar corretamente.

Salve o arquivo. Feche o arquivo.

E como declaramos a função **"tags\_para\_objeto()"**, precisamos agora que ela exista. Então, na pasta da aplicação **"tags"**, abra o arquivo **"\_\_init\_\_.py"** para edição e acrescente o trecho de código abaixo ao seu final:

    def tags_para_objeto(obj):
        tipo_dinamico = ContentType.objects.get_for_model(obj)
        
        tags = TagItem.objects.filter(
            content_type=tipo_dinamico,
            object_id=obj.id,
            )
    
        return ' '.join([item.tag.nome for item in tags])

Aí está a nossa função!

Veja que a primeira linha carrega o tipo dinâmico para o objeto:

        tipo_dinamico = ContentType.objects.get_for_model(obj)
    

E aqui nós carregamos a lista de **"TagItem"** para o objeto (tipo dinâmico + o **id** do objeto):

        tags = TagItem.objects.filter(
            content_type=tipo_dinamico,
            object_id=obj.id,
            )

E por fim, bom... na última linha nós temos uma _list comprehension_ das tags para transformá-las em uma string separada por espaços... mas vamos refletir um pouco para compreender como isso funciona...

Isto é uma _list comprehension_:

    [item for item in tags]
    

A variável **"tags"** possui uma lista de objetos do tipo **"TagItem"**, são os vínculos que temos entre o objeto em questão (o artigo "Olá mãe! Estou vivo!") e as tags "arte" e "brasil".

A _list comprehension_ acima, criada sobre a variável **"tags"**, retorna a variável exatamente como ela é, pois retornamos um a um de seus itens, sem nenhuma interferência, veja:

    [<TagItem: TagItem object>, <TagItem: TagItem object>]
    

Mas nós podemos retornar o nome das tags no lugar dos objetos, assim:

    [item.tag.nome for item in tags]
    

E o resultado será este:

    [u'arte', u'brasil']
    

É para isso que _list comprehensions_ existem: retornar o conteúdo de uma lista com algumas interferências que nós desejamos.

Agora, quando se usa o método **".join()"** em uma string, como abaixo:

    ' --- '.join([u'arte', u'brasil'])
    

Veja qual é o resultado disso:

    u'arte --- brasil'
    

Note que o **' --- '** é usado como separador entre os ítens da lista, sejam lá quantos eles forem, veja este outro exemplo:

    ', '.join([u'arte', u'brasil', 'outra string', 'mais uma'])
    

E aqui o resultado:

    u'arte, brasil, outra string, mais uma'
    

Veja que **', '** está entre cada uma das strings da lista.

Portanto, a última linha que escrevemos em nosso código:

        return ' '.join([item.tag.nome for item in tags])
    

Retornaria algo como isso:

    u'arte brasil'
    

Ignore o **"u'"**, ele é apenas um indicador de que a string está em **formato Unicode**.

E o arquivo **"admin.py"** da aplicação **"blog"** acabou ficando assim:

    from django.contrib import admin
    from django.contrib.admin.options import ModelAdmin
    from django import forms
    
    from models import Artigo
    from tags import aplicar_tags, tags_para_objeto
    
    class FormArtigo(forms.ModelForm):
        class Meta:
            model = Artigo
    
        tags = forms.CharField(max_length=30, required=False)
    
        def __init__(self, *args, **kwargs):
            super(FormArtigo, self).__init__(*args, **kwargs)
    
            if self.instance.id:
                self.initial['tags'] = tags_para_objeto(self.instance)
    
    class AdminArtigo(ModelAdmin):
        form = FormArtigo
    
        def save_model(self, request, obj, form, change):
            super(AdminArtigo, self).save_model(request, obj, form, change)
    
            aplicar_tags(obj, form.cleaned_data['tags'])
    
    admin.site.register(Artigo, AdminArtigo)

Salve o arquivo. Feche o arquivo. Volte ao navegador e carregue a página de Admin do nosso artigo novamente (mesmo que ele já esteja carregado na tela):

    http://localhost:8000/admin/blog/artigo/1/
    

E agora, mesmo sem você ter salvo nada, a página é exibida com os valores corretos:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/tags-campo-tags-com-efeito/file/"/>
</p>

### Fazendo a mesma coisa com as imagens da galeria

Agora, abra o arquivo **"admin.py"** da aplicação **"galeria"** e o modifique, para ficar assim:

    try:
        import Image
    except ImportError:
        from PIL import Image
    
    from django import forms
    from django.contrib import admin
    from django.contrib.admin.options import ModelAdmin
    
    from models import Album, Imagem
    from tags import aplicar_tags, tags_para_objeto
    
    class AdminAlbum(ModelAdmin):
        list_display = ('titulo',)
        search_fields = ('titulo',)
    
    class FormImagem(forms.ModelForm):
        class Meta:
            model = Imagem
    
        tags = forms.CharField(max_length=30, required=False)
    
        def __init__(self, *args, **kwargs):
            super(FormImagem, self).__init__(*args, **kwargs)
    
            if self.instance.id:
                self.initial['tags'] = tags_para_objeto(
                self.instance
                )
    
    class AdminImagem(ModelAdmin):
        list_display = ('album','titulo')
        list_filter = ('album',)
        search_fields = ('titulo','descricao',)
        form = FormImagem
    
        def save_model(self, request, obj, form, change):
            """Metodo declarado para criar miniatura da imagem depois de salvar"""
            super(AdminImagem, self).save_model(request, obj, form, change)
    
            if 'original' in form.changed_data:
                extensao = obj.original.name.split('.')[-1]
                obj.thumbnail = 'galeria/thumbnail/%s.%s'%(
                   obj.id, extensao)
    
                miniatura = Image.open(obj.original.path)
                miniatura.thumbnail((100,100), Image.ANTIALIAS)
                miniatura.save(obj.thumbnail.path)
    
                obj.save()
    
            aplicar_tags(obj, form.cleaned_data['tags'])
    
    admin.site.register(Album, AdminAlbum)
    admin.site.register(Imagem, AdminImagem)

Observe que as novidades são:

1. Importamos as funções **"aplicar\_tags()"** e **"tags\_para\_objeto()"**;
1. Acrescentamos o campo calculado **"tags"** ao formulário dinâmico da imagem;
1. Acrescentamos o método inicializador para carregar as tags;
1. Acrescentamos uma linha ao método **"save_model()"** para aplicar as tags à imagem;

Salve o arquivo. Feche o arquivo. Agora vá ao navegador e carregue a URL de uma imagem no Admin, esta por exemplo:

    http://localhost:8000/admin/galeria/imagem/1/
    

Informe algumas palavras no campo **"tags"** e clique sobre o botão **"Salvar e continuar editando"**. Tudo funcionando uma beleza:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/tags-campo-tags-na-imagem/file/"/>
</p>

### Agora criando uma página para as tags

Agora precisamos de uma página para listar as tags que existem em nosso site. Vamos começar pela URL: abra o arquivo **"urls.py"** da pasta do projeto e acrescente esta nova URL:

        (r'^tags/', include('tags.urls')),
    

Resumindo: criamos uma URL **"^tags/"** para todas as URLs da aplicação **"tags"**:

Agora o arquivo ficou assim:

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
    )
    
    if settings.LOCAL:
        urlpatterns += patterns('',
            (r'^media/(.*)$', 'django.views.static.serve',
             {'document_root': settings.MEDIA_ROOT}),
        )

Salve o arquivo. Feche o arquivo.

Agora vá à pasta da aplicação **"tags"** e crie um novo arquivo **"urls.py"** com o seguinte código dentro:

    from django.conf.urls.defaults import *
    
    urlpatterns = patterns('tags.views',
        url(r'^$', 'tags', name='tags'),
    )

Salve o arquivo. Feche o arquivo.

Agora precisamos criar a view que indicamos à URL acima. Para isso crie outro novo arquivo na pasta da aplicação **"tags"**, chamado **"views.py"**, com o seguinte código dentro:

    from django.shortcuts import render_to_response
    from django.template import RequestContext
    
    from models import Tag
    
    def tags(request):
        lista = Tag.objects.all()
        return render_to_response(
            'tags/tags.html',
            locals(),
            context_instance=RequestContext(request),
            )

Ali temos a view da URL **"/tags/"**, que retorna uma **lista de tags**, usando o template **"tags/tags.html"**.

Salve o arquivo. Feche o arquivo. Agora, vamos criar o template!

Ainda na pasta da aplicação **"tags"**, crie a pasta **"templates"** e dentro dela outra pasta chamada **"tags"**. Agora dentro da nova pasta crie o arquivo **"tags.html"** com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% block titulo %}Tags - {{ block.super }}{% endblock %}
    
    {% block h1 %}Tags{% endblock %}
    
    {% block conteudo %}
    
    <ul>
        {% for tag in lista %}
        <li><a href="{{ tag.get_absolute_url }}">{{ tag }}</a></li>
        {% endfor %}
    </ul>
    
    {% endblock conteudo %}

Temos aqui um template muito semelhante ao da **lista de álbuns** que trabalhamos no capítulo anterior.

Salve o arquivo. Feche o arquivo.

Agora vá ao navegador e carregue a seguinte URL:

    http://localhost:8000/tags/
    

Veja o que aparece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/tags-pagina-de-lista-de-tags/file/"/>
</p>

**Impressionado?**

Agora que tal criar...

### A página da tag e seus objetos!

Abra novamente o arquivo **"urls.py"** da aplicação **"tags"** para edição e acrescente a seguinte URL:

        url(r'^(?P<tag_nome>.*?)/$', 'tag', name='tag'),
    

E o arquivo vai ficar assim:

    from django.conf.urls.defaults import *
    
    urlpatterns = patterns('tags.views',
        url(r'^$', 'tags', name='tags'),
        url(r'^(?P<tag_nome>.*?)/$', 'tag', name='tag'),
    )




Salve o arquivo. Feche o arquivo.

Agora, abra o arquivo **"views.py"** da mesma pasta para edição, e acrescente as seguintes linhas de código ao final:

    def tag(request, tag_nome):
        tag = get_object_or_404(Tag, nome=tag_nome)
        return render_to_response(
            'tags/tag.html',
            locals(),
            context_instance=RequestContext(request),
            )

É apenas uma _view_ básica que carrega a **tag** e faz uso do template **"tags/tag.html"**, mas ela precisa também da função **"get\_object\_or\_404()"**. Para ajustar isso, localize esta linha:

    from django.shortcuts import render_to_response
    

E a modifique para ficar assim:

    from django.shortcuts import render_to_response, get_object_or_404
    

Salve o arquivo. Feche o arquivo.

Agora, vá até a pasta **"templates"** da pasta da aplicação, e abra a pasta **"tags"**, criando o arquivo **"tag.html"** com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% block titulo %}Tag "{{ tag.nome }}" - {{ block.super }}{% endblock %}
    
    {% block h1 %}Tag "{{ tag.nome }}"{% endblock %}
    
    {% block conteudo %}
    
    <ul>
        {% for item in tag.tagitem_set.all %}
        <li><a href="{{ item.objeto.get_absolute_url }}">{{ item.objeto }}</a></li>
        {% endfor %}
    </ul>
    
    {% endblock conteudo %}

Bom, agora veja bem: temos uma novidade aqui:

        {% for item in tag.tagitem_set.all %}
    

Essa linha faz referência a uma lista chamada **"tag.tagitem_set.all"** que não temos a menor idéia de onde veio.

Acontece que quando criamos o relacionamento entre as classes de modelo **"Tag"** e **"TagItem"** lá no começo do capítulo, a existência do campo **"tag"** como uma **"ForeignKey"** da classe **"Tag"** em **"TagItem"** faz o Django criar uma referência recíproca, fazendo o caminho inverso.

Ou seja: se a classe TagItem **faz parte** de uma Tag, então a Tag **possui** um **conjunto** de TagItem, e isso é representado por:

    tag.tagitem_set
    

E quando acrescentamos mais o elemento **all** estamos fazendo referência ao método **".all()"**, que retorna todos os objetos:

    tag.tagitem_set.all()
    

Só que no template, métodos devem ser chamados **sem os parênteses**. É apenas uma questão de sintaxe:

    {{ tag.tagitem_set.all }}
    

Por isso que fizemos um laço nele, usando a template tag **{% for %}**.

Agora, partindo para a novidade seguinte, veja esta outra linha:

        <li><a href="{{ item.objeto.get_absolute_url }}">{{ item.objeto }}</a></li>
    

Ali nós fizemos uma referência ao atributo **"objeto"** do item. Mas esse atributo não existe. Então, precisamos **fazê-lo existir**!

Salve o arquivo. Feche o arquivo.

### Conhecendo Generic Relations

Abra agora o arquivo **"models.py"** da aplicação **"tags"** e localize a seguinte linha:

        object_id = models.PositiveIntegerField(db_index=True)
    

Acrescente esta linha de código abaixo dela:

        objeto = GenericForeignKey('content_type', 'object_id')
    

O que esta nova linha de código faz é declarar uma **Generic Relation**, ou seja, nós estamos carregando o objeto relacionado (seja ele um **Artigo** ou uma **Imagem**) no atributo **"objeto"**. Para isso, usamos a classe **"GenericForeignKey"**.

Agora localize esta linha:

    from django.contrib.contenttypes.models import ContentType
    

E acrescente estas abaixo dela:

    from django.contrib.contenttypes.generic import GenericForeignKey
    from django.core.urlresolvers import reverse

Ali nós importamos a classe, que está num pacote da aplicação **"contenttypes"** que oferece funcionalidades para **Generic Relations**.

E a segunda importação nada tem a ver com Generic Relations, mas vai ajudar a outro ajuste que vai dar vida à página de **tags**:

Localize a seguinte linha:

        nome = models.CharField(max_length=30, unique=True)
    

E acrescente este bloco de código abaixo dela:

        def get_absolute_url(self):
            return reverse('tag', kwargs={'tag_nome': self.nome})

O arquivo **"models.py"** completo agora ficou assim:

    from django.db import models
    from django.contrib.contenttypes.models import ContentType
    from django.contrib.contenttypes.generic import GenericForeignKey
    from django.core.urlresolvers import reverse
    
    class Tag(models.Model):
        nome = models.CharField(max_length=30, unique=True)
    
        def get_absolute_url(self):
            return reverse('tag', kwargs={'tag_nome': self.nome})
    
        def __unicode__(self):
            return self.nome
    
    class TagItem(models.Model):
        class Meta:
            unique_together = ('tag', 'content_type', 'object_id')
            
        tag = models.ForeignKey('Tag')
        content_type = models.ForeignKey(ContentType)
        object_id = models.PositiveIntegerField(db_index=True)
        objeto = GenericForeignKey('content_type', 'object_id')

Salve o arquivo. Feche o arquivo.

Volte ao navegador, atualize a página com **F5** e clique sobre uma das tags listadas, veja como ela se sai:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/tags-pagina-da-tag/file/"/>
</p>

**Fascinante!** É incrível como as **Generic Relations** trabalham!

Mas como você descobriu, o artigo ficou com um nome estranho... isso porque lá uns capítulos atrás, quando criamos a aplicação **"blog"**, nós nos esquecemos de fazer uma coisinha.

### Ajustando a representação textual do artigo

Na pasta da aplicação **"blog"**, abra o arquivo **"models.py"** para edição e localize a seguinte linha:

            return reverse('blog.views.artigo', kwargs={'slug': self.slug})
    

Agora acrescente este trecho de código logo abaixo dela:

        def __unicode__(self):
            return self.titulo

Salve o arquivo. Feche o arquivo.

Volte ao navegador, atualize com **F5** e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/tags-pagina-da-tag-com-artigo-correto/file/"/>
</p>

### Mostrando as tags do artigo com um Template Filter

Tudo isso é impressionante, mas ainda não acabou. Nós precisamos também ajustar o **template do artigo** para mostrar suas **tags**, e não há um caminho mais fácil de fazer algo _cool_ no template do que criar um **Template Filter** pra isso!

**Template Filters** são funções usadas em templates para filtrar um objeto e retornar alguma outra coisa, e esta "outra coisa" pode ser filtrada por outro **Template Filter**, e assim por diante.

Na pasta da aplicação **"tags"**, crie uma nova pasta, chamada **"templatetags"**, e dentro dela, crie um arquivo vazio, chamado **"\_\_init\_\_.py"**. Crie ainda outro arquivo, chamado **"tags\_tags.py"** com o seguinte código dentro:

    from django.template import Library
    from django.contrib.contenttypes.models import ContentType
    
    from tags.models import TagItem
    
    register = Library()
    
    @register.filter
    def tags_para_objeto(objeto):
        tipo_dinamico = ContentType.objects.get_for_model(objeto)
        
        itens = TagItem.objects.filter(
            content_type=tipo_dinamico,
            object_id=objeto.id,
            )
        
        return [item.tag for item in itens]

Vamos decobrir o que foi feito neste código?

Na primeira linha, importamos a classe **"Library"** para templates de Django. O que nós estamos fazendo agora é criar uma **biblioteca de utilidades para templates** e para isso precisamos da classe **"Library"**

Já na segunda linha, importamos a classe **"ContentType"**, que você já conhece bem, e é nossa companheira para tipos dinâmicos.

    from django.template import Library
    from django.contrib.contenttypes.models import ContentType

Em seguida importamos a classe de modelo **"TagItem"**, pois vamos precisar dela:

    from tags.models import TagItem
    

Na linha a seguir, iniciamos a biblioteca de utilidades. É a ela que vamos incluir o nosso novo **template filter**:

    register = Library()
    

E é no restante do código que fazemos isso. Primeiro registramos o **template filter** na biblioteca. Ele vai receber um **objeto**, carregar seu **tipo dinâmico**, carregar seus vínculos com **tags** e finalmente fazer um _list comprehension_ para retornar a **lista das tags**:

<pre>
@register.filter
def tags_para_objeto(objeto):
    tipo_dinamico = ContentType.objects.get_for_model(objeto)

    itens = TagItem.objects.filter(
        content_type=tipo_dinamico,
        object_id=objeto.id,
        )

    return [item.tag for item in itens]
</pre>

Salve o arquivo. Feche o arquivo.

Agora a abra para edição o arquivo de template **"artigo.html"** da pasta **"blog/templates/blog"**, partindo da pasta do projeto, e localize esta linha:

    {% load comments %}
    

Modifique para ficar assim:

    {% load comments tags_tags %}
    

Com isso, acrescentamos a biblioteca que acabamos de criar ao **template do artigo**. Agora localize esta outra linha:

    {% block conteudo %}
    

E acrescente a seguinte linha abaixo dela:

    <p>
    Tags:
    {% for tag in artigo|tags_para_objeto %}
    <a href="{{ tag.get_absolute_url }}">{{ tag }}</a>
    {% endfor %}
    </p>

Observe que fazemos uso de nosso **template filter** dentro da tag **{% for tag in artigo|tags\_para\_objeto %}**.

Salve o arquivo. Feche o arquivo.

Agora vamos ao navegador para ver o efeito do que fizemos. Carregue a página de um artigo, assim:

    http://localhost:8000/artigo/ola-mae-estou-vivo/
    

Pois agora este é o resultado que você vê no navegador:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/tags-pagina-do-artigo-com-tags/file/"/>
</p>

Muito bacana esse tal **template filter** hein?

Mas agora nós vamos criar um template só para exibir as tags, e poder reusá-lo em outros templates com o mínimo de trabalho...

Volte ao arquivo de template **"artigo.html"** que acabamos de modificar e localize esta linha:

    {% load comments tags_tags %}
    

Volte-a para como estava antes, modificando para ficar assim:

    {% load comments %}
    

Agore localize este bloco de código e remova-o:

    <p>
    Tags:
    {% for tag in artigo|tags_para_objeto %}
    <a href="{{ tag.get_absolute_url }}">{{ tag }}</a>
    {% endfor %}
    </p>

No lugar do trecho de código que removeu, escreva este:

    {% with artigo as objeto %}
    {% include "tags/tags_para_objeto.html" %}
    {% endwith %}

Esse trecho de código vai criar um apelido para o artigo numa variável chamada **"objeto"**, que será válida dentro do bloco da template tag **{% with %}**. E então o template **"tags/tags\_para\_objeto.html"** será incluído, para cumprir o mesmo papel que tinha agorinha mesmo.

O resultado final do arquivo **"artigo.html"** será este:

    {% extends "base.html" %}
    
    {% load comments %}
    
    {% block titulo %}{{ artigo.titulo }} - {{ block.super }}{% endblock %}
    
    {% block h1 %}{{ artigo.titulo }}{% endblock %}
    
    {% block conteudo %}
    {% with artigo as objeto %}
    {% include "tags/tags_para_objeto.html" %}
    {% endwith %}
    
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

Salve o arquivo. Feche o arquivo.

Agora vá até a pasta **"tags/templates/tags"** partindo da pasta do projeto e crie um novo arquivo chamado **"tags\_para\_objeto.html"** com o seguinte código dentro

    {% load tags_tags %}
    
    <p>
    Tags:
    {% for tag in objeto|tags_para_objeto %}
    <a href="{{ tag.get_absolute_url }}">{{ tag }}</a>
    {% endfor %}
    </p>

Observe que desta vez, esta linha se refere à variável **"objeto"** e não à variável **"artigo"**:

    {% for tag in objeto|tags_para_objeto %}
    

Salve o arquivo. Feche o arquivo.

Agora que tal fazer o mesmo com a página de imagens?

Então abra para edição o arquivo **"imagem.html"** da pasta **"galeria/templates/galeria"**, partindo da pasta do projeto, e localize a seguinte linha:

        {{ imagem.descricao|linebreaks }}
    

Acrescente o seguinte trecho de código abaixo dela:

        {% with imagem as objeto %}
        {% include "tags/tags_para_objeto.html" %}
        {% endwith %}

Observe que a template tag **{% with imagem as objeto %}** faz referência à variável **"imagem"**, mas o apelido ainda é **"objeto"**.

Salve o arquivo. Feche o arquivo. Volte ao navegador, carregue a página de uma imagem, como esta URL, por exemplo:

    http://localhost:8000/galeria/imagem/impressao-nascer-do-sol/
    

Veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/tags-pagina-da-imagem-com-tags/file/"/>
</p>

Seja sincero com você mesmo: você conhece alguma ferramenta tão **elegante e produtiva** quanto esta?

### Um item para as Tags no menu

Agora vamos criar um item no menu para **tags** em seu site? Vamos? Vamos?!

Abra o arquivo **"base.html"** da pasta **"templates"** do projeto para edição, e localize esta linha:

         <li><a href="{% url galeria.views.albuns %}">Galeria de imagens</a></li>
    

Acrescente a seguinte linha abaixo dela:

         <li><a href="{% url tags.views.tags %}">Tags</a></li>
    

Salve o arquivo. Feche o arquivo.

Agora novamente no navegador, carregue qualquer página do site, esta por exemplo:

    http://localhost:8000/
    

Veja como ela ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/tags-item-no-menu/file/"/>
</p>

Pronto! Mais uma batalha vencida, e com muita elegância!

## Tudo agora em outra língua...

Alatazan olhou pra fora pela janela e o Sol já não estava mais lá... o céu um pouco mais escuro fez ele perceber que o tempo havia passado incrivelmente rápido, e já era hora de ir embora, havia um cheirinho de chuva no ar...

Naquele dia pela manhã, a aula fora um pouco chata e comentaram sobre uma certa "oportunidade" para ingressar em um **Emprego Tradicional**.

Havia algo errado com a escola de Engenharia de Software, e Alatazan, Cartola e Nena eram parte de um grupo anônimo de rebeldes que não concordavam com algumas práticas. A camiseta preta por baixo de suas roupas era uma forma de se identificarem, e eles não eram muitos...

Tudo isso se passou de relance na memória de Alatazan, e ele não notou aqueles cabelos avermelhados chegarem à janela e uma voz doce comentar, com suas costas encostadas na beirada da janela, uma mão segurando um lápis enquanto a outra segurou seu ombro:

- Vamos embora?

Alatazan se assustou um pouco, pois estava um tanto absorto naqueles pensamentos...

- Hã? Bom... me deixa resumir a aventura de hoje e nós vamos...

 - Nós criamos uma nova aplicação, chamada **"tags"**;

 - A aplicação possui duas classes de modelo: uma para as tags, outra para o vínculo delas com os objetos;

 - Todo esse vínculo é feito usando **Generic Relations**, que faz uso de três elementos: um campo **"content\_type"** para o tipo dinâmico, outro **"object_id"** para o **id** do objeto e um atributo **"objeto"** que é um **GenericForeignKey**;

 - Instalamos a aplicação no projeto e geramos as tabelas no banco de dados;

 - Modificamos o **Admin** das classes que relacionamos para ter um novo campo calculado, chamado **"tags"**, este campo salva e carrega as tags do objeto usando funções que criamos para isso;

 - Tudo é feito de forma discreta, com umas poucas mudanças no template e no **admin** das aplicações, sem interferências da camada de modelo ou no banco de dados;

 - Conhecemos um pouco de _list comprehensions_ e do método **".join()"** para concatenar strings;

 - Criamos templates para a aplicação **"tags"**;

 - Criamos um **template filter** para mostrar as tags dos objetos em templates;

 - Criamos um template para fazer uso desse template filter e ser incluído nos templates dos objetos que tenham tags;

 - Acrescentamos um item ao menu para mostrar isso tudo.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_10.gif" align="right"/>


- É isso **grande _Alata_**, dia de muitas novidades...

- Yep! Agora vamos fazer assim: amanhã eu quero ver aquela coisa da **internacionalização**, pode ser?

- Ô! Demorou! Já fica marcado então!

<span class="no_print">
**Próximo capítulo: [O mesmo site em vários idiomas](/o-mesmo-site-em-varios-idiomas/)**
</span>

