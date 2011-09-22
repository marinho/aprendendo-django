Uma das coisas que Alatazan mais gostou no Planeta Terra foram as **pinturas** de alguns artistas plásticos, especialmente os **impressionistas** e os **cubistas**.

A princípio ele achava que "impressionistas" eram aqueles caras corpulentos e de topete duro, queixo comprido e olhar de galã, vestindo uma tanguinha vermelha com uma pequena flor ao lado esquerdo.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_11.gif" align="right"/>

Não, não... umas semanas depois ele descobriu que isso se tratava de outro movimento. Impressionistas eram quase fotógrafos, só que mais generosos um pouco e com a diferença de precisarem de um oftalmologista urgente. Que vida dura a daquela época...

Então uma das coisas que Alatazan mais queria era uma **galeria de imagens** em seu site. Assim poderia mostrar as artes daqui para a sua mãe, numa tentativa a mais de fazê-la se interessar por arte.

## Uma nova aplicação no projeto

A primeira coisa a observar aqui é que a área de domínio de uma "galeria de imagens" está **fora do foco** de um blog. É possível ter um blog ou uma galeria de imagens, ou simplesmente **ter os dois**. Eles não são **dependentes** entre si, nem mesmo **excludentes**.

Por isso, começamos aqui a conhecer algumas das prática para se construir **aplicações plugáveis**.

### Criando a nova aplicação

Na pasta do projeto, crie uma nova pasta, chamada **"galeria"**. Dentro desta pasta crie um novo arquivo vazio, chamado **"\_\_init\_\_.py"**.

Agora, na mesma pasta, crie um arquivo chamado **"models.py"** e escreva o seguinte código dentro:

    from datetime import datetime
    from django.db import models
    
    class Album(models.Model):
        """Um album eh um pacote de imagens, ele tem um titulo e um slug para
        sua identificacao."""
        class Meta:
            ordering = ('titulo',)
            
        titulo = models.CharField(max_length=100)
        slug = models.SlugField(max_length=100, blank=True, unique=True)
    
        def __unicode__(self):
            return self.titulo
    
    class Imagem(models.Model):
        """Cada instancia desta classe contem uma imagem da galeria, com seu
        respectivo thumbnail (miniatura) e imagem em tamanho natural.
        Varias imagens podem conter dentro de um Album"""
        
        class Meta:
            ordering = ('album','titulo',)
            
        album = models.ForeignKey('Album')
        titulo = models.CharField(max_length=100)
        slug = models.SlugField(max_length=100, blank=True, unique=True)
        descricao = models.TextField(blank=True)
        original = models.ImageField(
            null=True,
            blank=True,
            upload_to='galeria/original',
            )
        thumbnail = models.ImageField(
            null=True,
            blank=True,
            upload_to='galeria/thumbnail',
            )
        publicacao = models.DateTimeField(default=datetime.now, blank=True)
    
        def __unicode__(self):
            return self.titulo

Puxa! Que tanto de coisas de uma só vez!

Bom, a representação em diagrama dessas duas classes de modelo é esta:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/diagrama-galeria-de-imagens/file/"/>
</p>

Vamos falar do que é novidade...

O trecho abaixo determina que a classe de modelo possui uma ordenação padrão pelos campos **"album"** e **"titulo"**, em ordem ascendente:

        class Meta:
            ordering = ('album','titulo',)

Já a linha seguinte, aponta uma **"Foreign Key"**, ou seja, um relacionamento da classe **"Imagem"** para a classe **"Album"**, o que significa que **uma imagem faz parte de um álbum**, e um álbum pode conter **muitas** delas:

        album = models.ForeignKey('Album')
    

Por fim, este outro trecho abaixo declara os campos para armazenamento de imagem. O primeiro campo ( **"original"** ) deve armazenar a imagem exatamente como esta foi enviada. Já o campo seguinte ( **"thumbnail"** ) deve armazenar sua miniatura, gerada automaticamente pela nossa aplicação.

        original = models.ImageField(
            null=True,
            blank=True,
            upload_to='galeria/original',
            )
        thumbnail = models.ImageField(
            null=True,
            blank=True,
            upload_to='galeria/thumbnail',
            )

Outra coisa importante nos campos declarados acima é o argumento **"upload\_to"** presente em cada um deles. Esse argumento define que as imagens enviadas para o campo em questão devem ser armazenadas na pasta indicada pela setting **"MEDIA\_ROOT"**, somada ao caminho atribuído ao argumento.

O campo **"original"** por exemplo indica que o caminho completo, partindo da pasta do projeto deve ser:

    media/galeria/original/
    

E o campo **"thumbnail"** indica este outro caminho:

    media/galeria/thumbnail/
    

Salve o arquivo. Feche o arquivo.

### Criando uma única função de signal para várias classes

Mas há ainda outra coisa importante para tratar. Você notou que ambas das classes possuem um campo **"slug"** seguindo a mesma linha de pensamento que tratamos no capítulo anterior? Pois é... isso significa que ambas precisam daquele **signal** que criamos para gerar o **slug** a partir do campo **"titulo"**.

Mas para evitar a repetição de código, vamos fazer agora uma coisa diferente: na pasta da aplicação **"blog"**, abra o arquivo **"models.py"** para edição e localize este trecho de código:

    # SIGNALS
    from django.db.models import signals
    from django.template.defaultfilters import slugify
    
    def artigo_pre_save(signal, instance, sender, **kwargs):
        """Este signal gera um slug automaticamente. Ele verifica se ja existe um
        artigo com o mesmo slug e acrescenta um numero ao final para evitar
        duplicidade"""
        if not instance.slug:
            slug = slugify(instance.titulo)
            novo_slug = slug
            contador = 0
    
            while Artigo.objects.filter(slug=novo_slug).exclude(id=instance.id).count() > 0:
                contador += 1
                novo_slug = '%s-%d'%(slug, contador)
    
            instance.slug = novo_slug
    
    signals.pre_save.connect(artigo_pre_save, sender=Artigo)

Modique para ficar assim:

    # SIGNALS
    from django.db.models import signals
    from utils.signals_comuns import slug_pre_save
    
    signals.pre_save.connect(slug_pre_save, sender=Artigo)

Levou um susto? Mas é isso mesmo! Nós importamos a função **"slug\_pre\_save"** do módulo **"utils.signals\_comuns"** que vai servir ao signal da classe **"Artigo"**.

Salve o arquivo. Feche o arquivo.

Agora voltando à pasta da aplicação **"galeria"**, abra novamente o arquivo **"models.py"** para edição e acrescente estas linhas ao final:

    # SIGNALS
    from django.db.models import signals
    from utils.signals_comuns import slug_pre_save
    
    signals.pre_save.connect(slug_pre_save, sender=Album)
    signals.pre_save.connect(slug_pre_save, sender=Imagem)

Observe que fizemos a mesma coisa que fizemos com a classe **"Artigo"** da aplicação **"blog"**.

Salve o arquivo. Feche o arquivo.

Agora na pasta do projeto, crie uma pasta chamada **"utils"** e dentro dela o arquivo **"\_\_init\_\_.py"** vazio. Crie também o arquivo **"signals\_comuns.py"** com o seguinte código dentro:

    from django.template.defaultfilters import slugify
    
    def slug_pre_save(signal, instance, sender, **kwargs):
        """Este signal gera um slug automaticamente. Ele verifica se ja existe um
        objeto com o mesmo slug e acrescenta um numero ao final para evitar
        duplicidade"""
        if not instance.slug:
            slug = slugify(instance.titulo)
            novo_slug = slug
            contador = 0
    
            while sender.objects.filter(slug=novo_slug).exclude(id=instance.id).count() > 0:
                contador += 1
                novo_slug = '%s-%d'%(slug, contador)
    
            instance.slug = novo_slug

Observe que há ali uma sutil diferença: nós trocamos a palavra **"Artigo"** para **"sender"**:

            while sender.objects.filter(slug=novo_slug).exclude(id=instance.id).count() > 0:
    

A variável **"sender"** dentro da função de signal representa a **classe** que enviou o signal. A variável **"instance"** representa o objeto em questão. E desde que os campos relacionados ao assunto sejam **"titulo"** e **"slug"**, essa função vai trabalhar bem em todos os casos.

Salve o arquivo. Feche o arquivo.

## Instalando a nova aplicação no projeto

Agora precisamos instalar a nova aplicação **"galeria"** no projeto. Para isso, abra o arquivo **"settings.py"** da pasta do projeto para edição e localize a setting **"INSTALLED\_APPS"** e ao final da tupla (logo abaixo da linha da aplicação **"blog"**), acrescente a seguinte linha:

        'galeria',
    

Para ficar assim:

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
    )

Salve o arquivo. Feche o arquivo.

Agora vamos gerar as novas tabelas no banco de dados. Na pasta do projeto, clique duas vezes sobre o arquivo **"gerar\_banco\_de\_dados.bat"** e veja que aparece o seguinte erro na tela do **MS-DOS**:

    Error: One or more models did not validate:
    galeria.imagem: "original": To use ImageFields, you need to install the Python Imaging Library.
    Get it at http://www.pythonware.com/products/pil/ .
    galeria.imagem: "thumbnail": To use ImageFields, you need to install the Python Imaging Library.
    Get it at http://www.pythonware.com/products/pil/ .

A mesma mensagem é mostrada duas vezes: uma para cada campo.

Para os campos **"original"** e **"thumbnail"**, que são do tipo **"ImageField"** é necessária a instalação de uma biblioteca adicional, chamada **"Python Imaging Library"**.

Para fazer o download da **PIL** (o nome curto e mais conhecido para a Python Imaging Library), vá a esta URL:

    http://www.pythonware.com/products/pil/
    

Na seção da página que inicia com **"Downloads"**, localize a versão mais adequada à versão do **Python** que você tem instalado. Nós trabalhamos no livro com a versão **2.5**, portanto, neste caso você deve clicar na seguinte opção:

    Python Imaging Library 1.1.6 for Python 2.5 (Windows only)
    

Depois de concluído o download, execute o arquivo e clique sobre o botão **"Avançar"** até finalizar a instalação.

Agora, com a **PIL** instalada, clique novamente duas vezes no arquivo **"gerar\_banco\_de\_dados.bat"** para gerar as novas tabelas no banco de dados. O resultado deve ser este:

    Creating table galeria_album
    Creating table galeria_imagem
    Installing index for galeria.Imagem model

Ou seja, foram criadas duas novas tabelas para a aplicação **"galeria"**.

### Configurando o admin para a nova aplicação

Agora, voltando à pasta da aplicação **"galeria"**, crie um novo arquivo, chamado **"admin.py"**, com o seguinte código dentro:

    from django.contrib import admin
    from django.contrib.admin.options import ModelAdmin
    
    from models import Album, Imagem
    
    class AdminAlbum(ModelAdmin):
        list_display = ('titulo',)
        search_fields = ('titulo',)
    
    class AdminImagem(ModelAdmin):
        list_display = ('album','titulo',)
        list_filter = ('album',)
        search_fields = ('titulo','descricao',)
    
    admin.site.register(Album, AdminAlbum)
    admin.site.register(Imagem, AdminImagem)

Como pode ver, nosso arquivo de **"admin"** para a aplicação **"galeria"** está melhor elaborado do que é o da aplicação **"blog"**.

A classe **"ModelAdmin"** existe para se trabalhar melhor o **Admin** de uma classe de modelo. É possível por exemplo informar quais campos você quer mostrar na listagem ( **"list\_display"** ) ou por quais campos você deseja filtrar os objetos ( **"list_filter"** ). Há ainda como informar por quais campos você deseja fazer buscas ( **"search\_fields"** ) e muitos outros recursos que ainda vamos estudar um pouco melhor num capítulo futuro.

Salve o arquivo. Feche o arquivo.

Agora execute o seu projeto, clicando duas vezes sobre o arquivo **"executar.bat"** da pasta do projeto. Caso ele já estava em execução, verifique se ele não foi afetado pela **mensagem de erro** que pedia a instalação da **PIL**. Neste caso, apenas feche a janela e execute o arquivo novamente.

Agora vá ao navegador e carregue a URL do Admin:

    http://localhost:8000/admin/
    

E a nova aplicação já mostra sua cara!

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/galeria-verificando-a-nova-aplicacao-no-admin/file/"/>
</p>

No entanto, para enviar uma nova imagem, antes é necessário criar as novas pastas, para onde os arquivos das imagens serão enviados.

Para isso, vá até a pasta **"media"** da pasta do projeto, crie uma nova pasta chamada **"galeria"**, e dentro dela crie outras duas: **"original"** e **"thumbnail"**.

Agora sim, você pode criar seu novo **Álbum** e logo em seguida, criar um **Imagem**, assim:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/galeria-nova-imagem/file/"/>
</p>

Ao clicar no botão **"Salvar e continuar editando"** você pode conferir que a imagem foi salva com sucesso, e o arquivo para o campo **"original"** foi enviado e gravado no HD. Mas você também vai descobrir uma outra coisa: **onde está o thumbnail**?

Nós não fizemos nada a respeito. O que precisamos é descobrir uma maneira de criar a miniatura da imagem **original** e salvar no campo **"thumbnail"**.

### Modificando o comportamento do Admin para criar a miniatura da imagem

Para isso, volte a abrir para edição o arquivo **"admin.py"** da pasta da aplicação **"galeria"**, e acrescente as seguintes linhas ao início do arquivo:

    try:
        import Image
    except ImportError:
        from PIL import Image
    
    from django import forms

Como você já sabe, o **"try"** faz uma tentativa e se algo der errado, ele evita que o software pare ali mesmo e executa um **plano "B"**. O que estamos fazendo alí é **tentar** importar o pacote **"Image"**, que se trata da biblioteca de imagens **"PIL"**, mas pode haver uma diferença de versões, e por segurança, caso isso dê errado, uma segunda importação é feita como **plano "B"**.

O pacote **"forms"** do Django também é importado, para a criação de um novo formulário dinâmico.

Agora localize a seguinte linha no arquivo:

    class AdminImagem(ModelAdmin):
    

**Antes** desta linha, você vai adicionar o seguinte trecho de código:

    class FormImagem(forms.ModelForm):
        class Meta:
            model = Imagem
    
        def save(self, *args, **kwargs):
            """Metodo declarado para criar miniatura da imagem depois de salvar"""
            imagem = super(FormImagem, self).save(*args, **kwargs)
    
            if 'original' in self.changed_data:
                extensao = imagem.original.name.split('.')[-1]
                imagem.thumbnail = 'galeria/thumbnail/%d.%s'%(imagem.id, extensao)
            
                miniatura = Image.open(imagem.original.path)
                miniatura.thumbnail((100,100), Image.ANTIALIAS)
                miniatura.save(imagem.thumbnail.path)
    
                imagem.save()
        
            return imagem

Vamos resumir o que fizemos:

O trecho abaixo declara uma classe de formulário dinâmico para a **entrada de dados** da classe de modelo **"Imagem"**. Observe que a palavra **"model"** está presente tanto na classe que herdamos quanto no atributo que indica a classe **"Imagem"**. Isso faz com que o formulário dinâmico tenha automaticamente todos os atributos da classe de modelo **"Imagem"** e ainda possui um método **"save()"** para **incluir** e **alterar** objetos dessa classe:

    class FormImagem(forms.ModelForm):
        class Meta:
            model = Imagem

A seguir, vem a declaração do método **save()**. Como este método já existe na classe herdada, nós tomamos o cuidado de não interferir em seu funcionamento, e para isso nós declaramos os argumentos **\*args** e **\*\*kwargs**, que recebem os mesmos argumentos e repassam ao método da classe herdada, usando **"super()"**.

Dessa forma, o formulário dinâmico permanece funcionando exatamente em seu comportamento padrão, pois a função **"super()"** tem o papel de executar um método da forma como ele foi declarado na classe herdada ( **"forms.ModelForm"** ), e não na classe atual ( **"FormImagem"** ).

        def save(self, *args, **kwargs):
            """Metodo declarado para criar miniatura da imagem depois de salvar"""
            imagem = super(FormImagem, self).save(*args, **kwargs)

Um pouco mais abaixo, fazemos uma interferência **após** a imagem ter sido salva. A primeira linha define a condição de o campo **"original"** da imagem ter sido **alterado**. Isso é importante porque uma imagem pode ter seu título ou descrição alterados sem que o arquivo da imagem o seja.

Nós também extraímos a **extensão** do arquivo original e atribuímos o nome do novo arquivo de miniatura ao campo **"thumbnail"**:

            if 'original' in self.changed_data:
                extensao = imagem.original.name.split('.')[-1]
                imagem.thumbnail = 'galeria/thumbnail/%d.%s'%(imagem.id, extensao)

Em seguida, está a linha que **carrega** a imagem do arquivo original, usando a biblioteca **PIL**...

                miniatura = Image.open(imagem.original.path)
    

... cria sua miniatura com dimensões máximas de **100 x 100 pixels**...

                miniatura.thumbnail((100,100), Image.ANTIALIAS)
    

... e salva com o caminho completo do arquivo de miniatura que definimos:

                miniatura.save(imagem.thumbnail.path)
    

Se a imagem que você enviou é do tipo **JPEG**, o caminho completo, retornado por **"imagem.thumbnail.path"** será:

    C:\Projetos\meu_blog\media\galeria\thumbnail\1.jpg
    

E para finalizar, a imagem é salva e retornada pelo método:

                imagem.save()
        
            return imagem

Agora lembre-se de que a classe **"AdminImagem"** não possui nenhum vínculo com o novo formulário dinâmico. Portanto, para dar efeito ao que fizemos, localize a seguinte linha:

        search_fields = ('titulo','descricao',)
    

E acrescente esta outra logo abaixo dela:

        form = FormImagem
    

Pronto! O arquivo **"admin.py"** completo ficou assim:

    try:
        import Image
    except ImportError:
        from PIL import Image
    
    from django import forms
    from django.contrib import admin
    from django.contrib.admin.options import ModelAdmin
    
    from models import Album, Imagem
    
    class AdminAlbum(ModelAdmin):
        list_display = ('titulo',)
        search_fields = ('titulo',)
    
    class FormImagem(forms.ModelForm):
        class Meta:
            model = Imagem
    
        def save(self, *args, **kwargs):
            """Metodo declarado para criar miniatura da imagem depois de salvar"""
            imagem = super(FormImagem, self).save(*args, **kwargs)
    
            if 'original' in self.changed_data:
                extensao = imagem.original.name.split('.')[-1]
                imagem.thumbnail = 'galeria/thumbnail/%d.%s'%(imagem.id, extensao)
                
                miniatura = Image.open(imagem.original.path)
                miniatura.thumbnail((100,100), Image.ANTIALIAS)
                miniatura.save(imagem.thumbnail.path)
    
                imagem.save()
            
            return imagem
    
    class AdminImagem(ModelAdmin):
        list_display = ('album','titulo',)
        list_filter = ('album',)
        search_fields = ('titulo','descricao',)
        form = FormImagem
    
    admin.site.register(Album, AdminAlbum)
    admin.site.register(Imagem, AdminImagem)

Salve o arquivo. Feche o arquivo. Faça o teste e veja como ficou!

### Resolvendo um problema de lógica

Agora, vamos fazer uma modificação. Você percebeu que trabalhamos o método **save()** da classe de formulário, mas se caso for criar uma nova imagem, um erro será exibido, indicando que o objeto da imagem está sendo requerido quando ainda não foi salvo no banco de dados. Isso ocorre porque o Admin está preparado para "salvar" apenas em memória neste método, para só depois efetuar de fato a persistência no banco de dados.

A forma correta de aplicar o *thumbnail* sem se preocupar com o momento de salvar, é usando o método **save_model()** da classe **ModelAdmin** ao invés deste que usamos.

Portanto, abra novamente o arquivo **admin.py** e modifique-o para ficar assim:

    try:
        import Image
    except ImportError:
        from PIL import Image
    
    from django import forms
    from django.contrib import admin
    from django.contrib.admin.options import ModelAdmin
    
    from models import Album, Imagem
    
    class AdminAlbum(ModelAdmin):
        list_display = ('titulo',)
        search_fields = ('titulo',)
    
    class FormImagem(forms.ModelForm):
        class Meta:
            model = Imagem
    
    class AdminImagem(ModelAdmin):
        list_display = ('album','titulo',)
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
    
    admin.site.register(Album, AdminAlbum)
    admin.site.register(Imagem, AdminImagem)

Salve o arquivo. Feche o arquivo. Crie uma nova imagem e confirme que agora tudo está ok!

### A página de apresentação da galeria de imagens

Agora que temos todo o _back-end_ da aplicação funcionando, vamos partir para o _front-end_.

Na pasta do projeto, abra o arquivo **"urls.py"** para edição e localize a seguinte linha:

        (r'^comments/', include('django.contrib.comments.urls')),
    

Esta linha faz a inclusão das URLs da aplicação **"comments"**, lembra-se? Pois bem, agora **abaixo** dela, escreva esta nova linha:

        (r'^galeria/', include('galeria.urls')),
    

Salve o arquivo. Feche o arquivo.

Vá até a pasta da aplicação **"galeria"** e crie um novo arquivo chamado **"urls.py"**, com o seguinte código dentro:

    from django.conf.urls.defaults import *
    
    urlpatterns = patterns('galeria.views',
        url(r'^$', 'albuns', name='albuns'),
    )

Salve o arquivo. Feche o arquivo.

Agora, para responder a essa nova URL que criamos, crie outro novo arquivo, chamado **"views.py"**, com o seguinte código dentro:

    from django.shortcuts import render_to_response
    from django.template import RequestContext
    
    from models import Album
    
    def albuns(request):
        lista = Album.objects.all()
        return render_to_response(
            'galeria/albuns.html',
            locals(),
            context_instance=RequestContext(request),
            )

Salve o arquivo. Feche o arquivo.

Até aqui não há nada de novo: teremos uma URL **"/galeria/"** que vai exibir uma página do template **"galeria/albuns.html"** com uma lista de álbuns.

Agora, ainda na pasta da aplicação **"galeria"**, crie uma nova pasta chamada **"templates"** e dentro dela crie outra pasta chamada **"galeria"**. E agora, dentro dessa nova pasta ( **"galeria/templates/galeria"** ), crie o arquivo de template, chamado **"albuns.html"** com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% block titulo %}Galeria de Imagens - {{ block.super }}{% endblock %}
    
    {% block h1 %}Galeria de imagens{% endblock %}

    {% block conteudo %}
    
    <ul>
        {% for album in lista %}
        <li><a href="{{ album.get_absolute_url }}">{{ album }}</a></li>
        {% endfor %}
    </ul>
    
    {% endblock conteudo %}

Salve o arquivo. Feche o arquivo.

Aqui também não houve nada de novo: nós extendemos o template **"base.html"** para trazer a página padrão do site, exibimos o **título da página** e fizemos um laço em todos os **Álbuns** da variável **"lista"**, exibindo o link para cada um em uma lista não-numerada ( **&lt;ul&gt;** ).

O conjunto que declaramos (a URL, a View e o Template) podem ser acessados agora no navegador pelo seguinte endereço:

    http://localhost:8000/galeria/
    

Veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/galeria-pagina-de-albuns/file/"/>
</p>

Agora precisamos criar uma página para o Álbum, certo? Pois vá até a pasta da aplicação **"galeria"**, abra novamente o arquivo **"urls.py"** para edição e acrescente esta nova URL:

        url(r'^(?P<slug>[\w_-]+)/$', 'album', name='album'),
    

Desta forma, o arquivo **"urls.py"** ficou assim:

    from django.conf.urls.defaults import *
    
    urlpatterns = patterns('galeria.views',
        url(r'^$', 'albuns', name='albuns'),
        url(r'^(?P<slug>[\w_-]+)/$', 'album', name='album'),
    )

Salve o arquivo. Feche o arquivo.

Ainda na mesma pasta, abra o arquivo **"views.py"** para edição, e acrescente as seguintes linhas de código ao final do arquivo:

    def album(request, slug):
        album = get_object_or_404(Album, slug=slug)
        imagens = Imagem.objects.filter(album=album)
    
        return render_to_response(
            'galeria/album.html',
            locals(),
            context_instance=RequestContext(request),
            )

Esta nova **view** vai responder à URL do álbum, mas isso não é suficiente, pois precisamos importar a função **"get\_object\_or\_404"**. Portanto localize a seguinte linha:

    from django.shortcuts import render_to_response
    

E a modifique para ficar assim

    from django.shortcuts import render_to_response, get_object_or_404
    

Observe que o que fizemos na linha acima é o mesmo que isso:

    from django.shortcuts import render_to_response
    from django.shortcuts import get_object_or_404

Também precisamos importar a classe **"Imagem"**. Localize a seguinte linha:

    from models import Album
    

E a modifique para ficar assim:

    from models import Album, Imagem
    

Agora todo o arquivo **"views.py"** ficou assim:

    from django.shortcuts import render_to_response, get_object_or_404
    from django.template import RequestContext
    
    from models import Album, Imagem
    
    def albuns(request):
        lista = Album.objects.all()
        return render_to_response(
            'galeria/albuns.html',
            locals(),
            context_instance=RequestContext(request),
            )
    
    def album(request, slug):
        album = get_object_or_404(Album, slug=slug)
        imagens = Imagem.objects.filter(album=album)
    
        return render_to_response(
            'galeria/album.html',
            locals(),
            context_instance=RequestContext(request),
            )

Salve o arquivo. Feche o arquivo.

Agora, partindo da pasta dessa aplicação, vá até a pasta **"templates/galeria"** e crie um novo arquivo chamado **"album.html"**, com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% block titulo %}{{ album }} - {{ block.super }}{% endblock %}
    
    {% block h1 %}{{ album }}{% endblock %}
    
    {% block conteudo %}
    <div class="miniaturas">
    
        {% for imagem in imagens %}
        <div class="miniatura">
            <a href="{{ imagem.get_absolute_url }}">
            <img src="{{ imagem.thumbnail.url }}"/>
            {{ imagem }}
            </a>
        </div>
        {% endfor %}
    
    </div>
    {% endblock conteudo %}

Veja que continuamos trabalhando em cima do que já conhecemos. Dê uma atenção especial às referências **"{{ album }}"** e **"{{ imagem.thumbnail.url }}"**. Observe que a URL do campo **"thumbnail"** é acessível através do seu atributo **"url"**.

Salve o arquivo. Feche o arquivo.

Agora, antes de testar, volte à pasta da aplicação **"galeria"**, abra o arquivo **"models.py"** para edição e localize a seguinte linha:

    from django.db import models
    

Acrescente a seguinte linha abaixo dela:

    from django.core.urlresolvers import reverse
    

Agora, ao final da classe **"Album"**, acrescente o seguinte trecho de código:

    def get_absolute_url(self):
        return reverse('album', kwargs={'slug': self.slug})

E faça o mesmo para a classe **"Imagem"**. Observe a diferença entre as duas - a palavra **"imagem"** como primeiro argumento da função **"reverse()"**, é o nome que demos a essas URLs, usando seu argumento **"name"**, lá atrás, no arquivo **"urls.py"**:

    def get_absolute_url(self):
        return reverse('imagem', kwargs={'slug': self.slug})

Agora o arquivo **"models.py"** ficou assim:

    from datetime import datetime
    from django.db import models
    from django.core.urlresolvers import reverse
    
    class Album(models.Model):
        """Um album eh um pacote de imagens, ele tem um titulo e um slug para
        sua identificacao."""
        class Meta:
            ordering = ('titulo',)
            
        titulo = models.CharField(max_length=100)
        slug = models.SlugField(max_length=100, blank=True, unique=True)
    
        def __unicode__(self):
            return self.titulo
    
        def get_absolute_url(self):
            return reverse('album', kwargs={'slug': self.slug})
    
    class Imagem(models.Model):
        """Cada instancia desta classe contem uma imagem da galeria, com seu
        respectivo thumbnail (miniatura) e imagem em tamanho natural.
        Varias imagens podem conter dentro de um Album"""
        
        class Meta:
            ordering = ('album','titulo',)
            
        album = models.ForeignKey('Album')
        titulo = models.CharField(max_length=100)
        slug = models.SlugField(max_length=100, blank=True, unique=True)
        descricao = models.TextField(blank=True)
        original = models.ImageField(
            null=True,
            blank=True,
            upload_to='galeria/original',
            )
        thumbnail = models.ImageField(
            null=True,
            blank=True,
            upload_to='galeria/thumbnail',
            )
        publicacao = models.DateTimeField(default=datetime.now, blank=True)
    
        def __unicode__(self):
            return self.titulo
    
        def get_absolute_url(self):
            return reverse('imagem', kwargs={'slug': self.slug})
    
    # SIGNALS
    from django.db.models import signals
    from utils.signals_comuns import slug_pre_save
    
    signals.pre_save.connect(slug_pre_save, sender=Album)
    signals.pre_save.connect(slug_pre_save, sender=Imagem)

Salve o arquivo. Feche o arquivo.

No navegador, carregue a URL de nossa galeria de imagens:

    http://localhost:8000/galeria/
    

Veja como aparece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/galeria-pagina-de-albuns-com-url/file/"/>
</p>

Agora clique sobre o **link** do álbum e veja como a página é carregada:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/galeria-pagina-do-album/file/"/>
</p>

Estamos agora com as miniaturas do álbum na tela! Agora, resta criar a página para visualizar a foto em seu tamanho original!

### Criando uma página só para a imagem em seu tamanho original

Na pasta da aplicação **"galeria"**, abra o arquivo **"urls.py"** para edição, e acrescente esta nova URL:

        url(r'^imagem/(?P<slug>[\w_-]+)/$', 'imagem', name='imagem'),
    

Agora, o arquivo **"urls.py"** deve ficar assim:

    from django.conf.urls.defaults import *
    
    urlpatterns = patterns('galeria.views',
        url(r'^$', 'albuns', name='albuns'),
        url(r'^(?P<slug>[\w_-]+)/$', 'album', name='album'),
        url(r'^imagem/(?P<slug>[\w_-]+)/$', 'imagem', name='imagem'),
    )

Salve o arquivo. Feche o arquivo.

Agora abra o arquivo **"views.py"** da mesma pasta para edição, e acrescente o trecho de código abaixo ao final do arquivo:

    def imagem(request, slug):
        imagem = get_object_or_404(Imagem, slug=slug)
    
        return render_to_response(
            'galeria/imagem.html',
            locals(),
            context_instance=RequestContext(request),
            )

Como pode ver, esta nova **view** é muito semelhante à do **Álbum**.

Salve o arquivo. Feche o arquivo.

Agora, ainda na pasta da aplicação, vá até a pasta **"templates/galeria"** e crie um novo arquivo chamado **"imagem.html"**, com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% block titulo %}{{ imagem }} - {{ block.super }}{% endblock %}
    
    {% block h1 %}{{ imagem }}{% endblock %}
    
    {% block conteudo %}
    <div class="imagem">
        <img src="{{ imagem.original.url }}"/>
    
        {{ imagem.descricao|linebreaks }}
    
    </div>
    {% endblock conteudo %}

Como pode ver, seguimos a mesma _cartilha_ também para este template.

Salve o arquivo. Feche o arquivo.

Agora volte ao navegador, atualize a página com **F5** e clique sobre a miniatura da imagem. Veja que a nova página é carregada:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/galeria-pagina-da-imagem/file/"/>
</p>

Uau! Temos uma batalha quase vencida! Agora é só colocar um item no menu!

### Ajustando o template base para a nova aplicação

Na pasta **"templates"** do projeto, abra o arquivo **"base.html"** para edição e localize a seguinte linha:

         <li><a href="/">Pagina inicial</a></li>
    

Logo abaixo da linha encontrada, acrescente esta outra:

         <li><a href="{% url galeria.views.albuns %}">Galeria de imagens</a></li>
    

Salve o arquivo. Feche o arquivo. Volte ao navegador e atualize, com a tecla **F5**. Veja agora como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/galeria-com-item-no-menu/file/"/>
</p>

Pronto! E assim temos mais uma página virada, ou várias páginas, né?

## Agora vamos organizar as coisas!

Alatazan ficou fascinado! Não parecia possível criar uma aplicação de imagens em tão pouco tempo!

- É fascinante! Resumindo o que fizemos não foi muita coisa:

 - Criamos uma nova aplicação;
 - Criamos duas classes, uma relacionada à outra, usando um campo de **ForeignKey**;
 - Definimos a **pasta para upload** dos arquivos para os campos de imagem;
 - Fizemos um _refactoring_ no signal que gera slugs;
 - Instalamos a aplicação no projeto, usando a setting **"INSTALLED\_APPS"**;
 - Instalamos a **PIL** para o Django trabalhar com imagens;
 - Geramos as **novas tabelas** no banco de dados;
 - Criamos um Admin melhor elaborado, com alguns campos na **lista**, **filtros** e **busca**;
 - Ajustamos o Admin para criar as miniaturas para o campo **"thumbnail"**... isso foi feito com um **ModelForm**;
 - E criamos os **templates** para a aplicação.

- É... pensando melhor, foram muitas coisas sim...

- E olha, Alatazan, foram muitas coisas que aprendemos, mas é importante você **observar com atenção** o código dos arquivos que criamos, pois há ali muitas informações importantes no aprendizado. O Django é muito claro, muito coeso, as palavras usadas para identificar qualquer coisa possuem um significado forte... - Nena fez sua (quase) advertência.

- T...

**Bláaaaaa!!!**

O barulho foi forte, e veio da biblioteca do pai de Cartola... Alatazan presumiu que seria alguma coisa relacionada ao **Ballzer**... ele estava tão comportado nos últimos dias que esse tipo de "comportado" não daria em coisa boa...

<img src="http://www.aprendendodjango.com/media/img/ilustracao_13.gif" align="right"/>

E ele presumiu com razão... Ballzer ainda estava um pouco atordoado com a situação, e o carpete verde-escuro tinha todos os livros espalhados... era fácil notar um livro sobre **"ervas medicinais"** enfiado ao meio de outro sobre **"raizes culturais"**.

Alatazan sabia que por mais íntimas que fossem as palavras **"ervas"** e **"raízes"**, não havia nada ali de comum entre os dois **temas**...

... e à medida que se desculpava e tentava dividir sua atenção entre juntar os livros e brigar com **Ballzer**, ele descobriu **mais uma coisa** pra colocar em seu site...

<span class="no_print">
**Próximo capítulo: [Organizando as coisas com Tags](/organizando-as-coisas-com-tags/)**
</span>

