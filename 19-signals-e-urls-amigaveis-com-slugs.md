<img src="http://www.aprendendodjango.com/media/img/ilustracao_8.gif" align="right"/>

Em Katara há uma classe de artistas auto-denominados **"Artistas Peônicos Geométricos Biocorretistas Surfadores"**, um nome criado por eles que não é visto da mesma forma pelos agricultores.

Sua arte consiste em levar seus discos voadores até uma plantação bacana, amarrar um cabo à nave, e prender um cortador de grama na outra extremidade do cabo.

Surfando em pranchas voadoras, eles giram suas naves enquando sobem e descem no ar, cortando e quebrando a plantação em formas geométricas como bolas, bolinhas, bolonas, meias-luas e outras formas curvas sem nenhum fundamento. Há aqueles de nível avançado que já fazem quadradinhos, losangos e outras coisas, mas ainda assim geométricas demais para agradar a qualquer **crítico de arte** ou **agricultor**.

Depois de um tempo, esses Artistas Peônicos Geométricos Biocorretistas Surfadores foram expulsos de Katara, pra deixar seus críticos de arte e agricultores em paz.

E saíram em busca de outros planetas. Mas agora eles gostam de dar um clima à situação, antes de se revelar como autores da arte.

Alguns agricultores de outros planetas já começaram a reclamar, mas os críticos de arte ainda não se pronunciaram a respeito, devido a não saber exatamente quem fez e assim ficam com medo de ser alguém famoso e descolado.

O que eles ainda não notaram é alguns agricultores entenderam sua arte como **sinais do além**, e a cada novo desenho que é feito numa plantação, seus novos **seguidores** reagem aos "sinais" de forma cada vez mais convencida.

Para desenhos com bolinhas eles dançam uma dança especial. Para desenhos com quadradinhos soltam fogos de artifício. E assim por diante.

## Usando signals para criar slugs

Antes de falar em **Signals** vamos falar dos **Slugs**.

"Slug" é uma expressão que não tem nada a ver com uma **lesmas**. "Slug" é uma expressão do meio jornalístico para criar identificações mais claras e intuitivas para conteúdo publicado na web.

Dessa forma, o monte bolinhas, quadradinhos e risquinhos abaixo não é visto exatamente como uma obra de arte:

    http://turismo.terra.com.br/interna/0,,OI1768089-EI176,00.html
    

Pois ela poderia ser assim:

    http://turismo.terra.com.br/interna/confira-dicas-para-arrumar-malas-para-viagem/
    

E sem dúvida ficaria mais fácil para pessoas **se identificarem com ela**. Mecanismos de **buscas** também ficam muito mais confortáveis com esse tipo de URL.

Agora, inicie o projeto em nosso ambiente de desenvolvimento, clicando duas vezes no arquivo **"executar.bat"** da pasta do projeto ( **"C:\Projetos\meu\_blog"** ).

Abra o navegador e carregue a seguinte URL:

    http://localhost:8000/artigo/1/
    

A seguinte página será carregada:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-url-sem-slug/file/"/>
</p>

Um artigo com o título **"Olá mãe! Estou vivo!"** poderia muito bem ter uma URL como esta abaixo, não?

    http://localhost:8000/artigo/ola-mae-estou-vivo/
    

Muito mais amigável e sem dúvida mais fácil de identificar. Então vamos lá!

### Acrescentando um novo campo de slug à classe de modelo

Na pasta da aplicação **"blog"**, carregue o arquivo **"models.py"** para edição e localize a seguinte linha:

        publicacao = models.DateTimeField(default=datetime.now, blank=True)
    

Logo abaixo dela, acrescente a seguinte linha de código:

        slug = models.SlugField(max_length=100, blank=True)
    

Observe que definimos uma **largura máxima de 100 caracteres**, para que ele tenha a mesma largura máxima do campo **"titulo"**.

Observe também que definimos este campo com **blank=True**, o que significa que é um campo que **não precisa ser preenchido**.

Salve o arquivo. Feche o arquivo.

Agora, volte ao navegador e atualize pressionando a tecla **F5**. Veja o que acontece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-coluna-nao-encontrada/file/"/>
</p>

A mensagem de erro é enfática: **não existe a coluna 'blog\_artigo.slug'**. Ou seja, não existe o campo **"slug"** na tabela **"blog\_artigo"**.

Agora que a tabela **"blog\_artigo"** já existe no banco de dados, com os próprios recursos do Django não é possível gerar automaticamente mudanças como a criação da nova coluna **"slug"**. Isso porque esse processo nunca é de fato tão simples quanto parece.

O Django possui um comando chamado **"dbshell"** para executar com o arquivo **"manage.py"** da pasta do projeto. Este comando acessa o *shell* do banco de dados para que você faça qualquer tarefa ali, seja de definição do modelo de dados, consultas ou tarefas de manutenção. Infelizmente esse comando não trabalha bem no Windows.

No Windows, é preciso usar um pequeno aplicativo de *shell* do **SQLite**. Portanto, vamos baixá-lo, certo?

Vá até a seguinte página e faça o download:

    http://www.sqlite.org/download.html
    

Localize o bloco que inicia com **"Precompiled Binaries For Windows"**. Faça o download do primeiro item da lista, que tem um nome semelhante a este:

    sqlite-3_6_6_2.zip
    

Ao concluir o download, extraia o único arquivo ( **"sqlite3.exe"** ) para a pasta do projeto. Agora para usá-lo de uma maneira fácil, crie um novo arquivo chamado **"dbshell.bat"** na pasta do projeto com a seguinte linha de código:

    sqlite3.exe meu_blog.db
    

Salve o arquivo. Feche o arquivo. Clique duas vezes sobre ele para executá-lo, e veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-executando-shell-do-sqlite/file/"/>
</p>

Estamos dentro do shell do **SQLite**.

Agora para criar a nova coluna, digite a seguinte expressão SQL no shell do SQLite:

    alter table blog_artigo add slug varchar(200);
    

Feito isso, feche a janela do shell e volte ao navegador. Atualize a página com a tecla **F5** e veja que voltamos ao estado normal da página.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-url-sem-slug/file/"/>
</p>

Agora vá à página de manutenção desse artigo no **Admin**, para ver como está o campo **"slug"**:

    http://localhost:8000/admin/blog/artigo/1/
    

A apresentação da página é esta:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-no-admin-campo-vazio/file/"/>
</p>

Sim, o campo está vazio. E não vá preenchê-lo por você próprio. Não faz sentido se o Django faz isso automaticamente. Mas como? Precisamos de definir um **signal**, um recurso do Django para agendar eventos.

### Criando um signal

Os **Signals** no Django são como **triggers** em bancos de dados. E como eventos em programação gráfica. Basicamente a coisa funciona assim:

1. O artigo é modificado;
1. Quando esse artigo é salvo, antes que as mudanças sejam enviadas para o banco de dados, os signals do tipo **"pre\_save"** da classe de modelo **Artigo** são executados, um a um, em sequência;
1. Caso nada ocorra de errado as modificações são gravadas no banco de dados;
1. Depois disso, os signals do tipo **"post\_save"** da mesma classe de modelo são executados, também em sequência.

Portanto, vamos criar um signal de **"pre_save"** para preencher o campo **"slug"** automaticamente antes que os dados sejam gravados no banco de dados.

Para isso, vá até a pasta da aplicação **"blog"**, abra o arquivo **"models.py"** para edição e acrescente as seguintes linhas ao final do arquivo:

    # SIGNALS
    from django.db.models import signals
    from django.template.defaultfilters import slugify
    
    def artigo_pre_save(signal, instance, sender, **kwargs):
        instance.slug = slugify(instance.titulo)
    
    signals.pre_save.connect(artigo_pre_save, sender=Artigo)

Resumindo o que fizemos:

Nós importamos os pacotes de **signals** para classes de modelo e a função **"slugify"**, que transforma qualquer *string* para formato de **slug**:

    # SIGNALS
    from django.db.models import signals
    from django.template.defaultfilters import slugify

Definimos a função a ser conectada ao **signal**. Esta função será executada toda vez que um **artigo** for salvo, antes que as mudanças sejam persistidas no banco de dados:

    def artigo_pre_save(signal, instance, sender, **kwargs):
    

A linha seguinte faz exatamente o que precisamos: atribui ao campo **"slug"** do **artigo** em questão o valor do campo **"titulo"**, formatado para slug:

        instance.slug = slugify(instance.titulo)
    

Por fim, conectamos a função ao *signal*, dizendo para o Django que a função deve ser executada pelo signal **"pre\_save"**, quando este for enviado pela classe **"Artigo"**:

    signals.pre_save.connect(artigo_pre_save, sender=Artigo)
    

Salve o arquivo. Feche o arquivo.

Volte ao navegador, na página do artigo no **Admin** e apenas clique sobre o botão **"Salvar e continuar editando"** para ver o efeito do que fizemos, veja com seus próprios olhos:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-no-admin-campo-preenchido/file/"/>
</p>

Nosso signal está funcionando bacana, certo? Agora carregue a página principal do blog:

    http://localhost:8000/
    

Mova o mouse sobre o **título do artigo** (que é um **link**) e veja que a URL do artigo permanece da mesma forma:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-url-sem-slug-no-link-da-home/file/"/>
</p>

Isso é porquê, de fato, não fizemos nada nesse sentido. Agora vá à pasta **"blog/templates/blog/"**, abra o arquivo **"artigo\_archive.html"** para edição e localize a seguinte linha:

        <a href="{% url blog.views.artigo artigo_id=artigo.id %}"><h2>{{ artigo.titulo }}</h2></a>
    

Modifique esta linha para ficar assim:

        <a href="{{ artigo.get_absolute_url }}"><h2>{{ artigo.titulo }}</h2></a>
    

Salve o arquivo. Feche o arquivo.

Porque estamos fazendo isso?

Quando criamos a listagem do blog, no **capítulo 4 - "Criando um Blog maneiro"** - informamos a template tag **"{% url %}"** para obter a URL do artigo e criar nosso link com ela. Depois, quando habilitamos o recurso de RSS no **capítulo 6 - "O RSS é o entregador fiel"**, definimos o método **"get\_absolute\_url()"** na classe de modelo **Artigo**, também para representar a mesma URL. Ou seja, para dois casos diferentes, usamos duas formas também diferentes de obter a **mesma URL**: a URL do artigo.

Isso não estava certo.

Portanto agora estamos fazendo um casamento das duas idéias: acabamos de ajustar o template da lista de artigos para usar o método **"get\_absolute\_url()"**, e vamos agora ajustar esse método para trabalhar da mesma forma que trabalha a template tag **"{% url %}"**.

Vá até à pasta da aplicação **"blog"** e abra o arquivo **"models.py"** para edição. Localize o seguinte trecho de código:

        def get_absolute_url(self):
            return '/artigo/%d/'%self.id

Modifique para ficar assim:

        def get_absolute_url(self):
            return reverse('blog.views.artigo', kwargs={'slug': self.slug})

E como a função **"reverse"** é um elemento estranho aqui, vamos importá-la no início do arquivo. Localize a seguinte linha:

    from django.db import models
    

E acrescente logo abaixo dela:

    from django.core.urlresolvers import reverse
    

Salve o arquivo. Feche o arquivo. Agora volte ao navegador, atualize a página com **F5** e veja o que acontece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-url-vazia-no-link-da-home/file/"/>
</p>

Mas o que aconteceu? Perdemos o link? Sim. Perdemos porque ainda falta uma coisa.

Na definição da URL do artigo há uma referência a seu campo **"id"**, mas nós acabamos de definir a seguinte linha de código, que agora faz uso do **"slug"** em seu lugar:

            return reverse('blog.views.artigo', kwargs={'slug': self.slug})
    

Portanto, na pasta do projeto, abra o arquivo **"urls.py"** para edição e localize a seguinte linha de código:

        (r'^artigo/(?P<artigo_id>\d+)/$', 'blog.views.artigo'),
    

Modifique para ficar assim:

        (r'^artigo/(?P<slug>[\w_-]+)/$', 'blog.views.artigo'),
    

Você notou que trocamos o **"artigo_id"** pelo **"slug"**? Você vai descobrir também que essa expressão regular exige **letras, números e os caracteres "\_" e "-"**, quando define **"[\w\_-]+"** no lugar de **"\d+"**

Salve o arquivo. Feche o arquivo. Volte ao navegador, atualize a página e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-url-correta-no-link-da-home/file/"/>
</p>

Até que enfim, temos uma URL amigável!

Agora clique sobre o link, e... mais uma tela de erro!

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-view-sem-parametro-slug/file/"/>
</p>

Esse erro ocorreu porque precisamos também ajustar a **view** do artigo, para aceitar o parâmetro **"slug"**.

Então, na pasta da aplicação **"blog"**, abra o arquivo **"views.py"** para edição e localize o seguinte trecho de código:

    def artigo(request, artigo_id):
        artigo = Artigo.objects.get(id=artigo_id)
        return render_to_response('blog/artigo.html', locals(),
            context_instance=RequestContext(request))

Agora modifique para ficar assim:

    def artigo(request, slug):
        artigo = Artigo.objects.get(slug=slug)
        return render_to_response('blog/artigo.html', locals(),
            context_instance=RequestContext(request))

Notou que trocamos as referências ao campo **"id"** para o campo **"slug"** do artigo?

Salve o arquivo. Volte ao navegador, atualize a página com **F5** e veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-view-com-parametro-slug/file/"/>
</p>

Satisfeito? Sim, claro, mas vamos tentar uma coisa diferente agora...

### Usando get\_object\_or\_404

.. carregue a seguinte URL no navegador:

    http://localhost:8000/artigo/artigo-que-nao-existe/
    

E veja o que ocorre quando se tenta carregar a página de um artigo que não existe:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-artigo-nao-existe/file/"/>
</p>

A mensagem diz o que já dissemos:

    Artigo matching query does not exist.
    

Ou seja: **este artigo não existe**. Mas nós podemos melhorar isso. Volte ao arquivo **"views.py"** da pasta da aplicação **"blog"** e localize esta linha:

        artigo = Artigo.objects.get(slug=slug)
    

Agora modifique, para ficar assim:

        artigo = get_object_or_404(Artigo, slug=slug)
    

Em outras palavras: a partir de agora, quando o artigo com o slug informado não for encontrado, será retornado um erro do tipo **"Página não encontrada"**, o famoso **erro 404**.

Precisamos também importar a função **"get\_object\_or\_404()"**. Para isso localize esta outra linha:

    from django.template import RequestContext
    

E acrescente esta abaixo dela:

    from django.shortcuts import get_object_or_404
    

Agora, o arquivo **"views.py"** está assim:

    from django.shortcuts import render_to_response
    from django.template import RequestContext
    from django.shortcuts import get_object_or_404
    
    from models import Artigo
    
    def artigo(request, slug):
        artigo = get_object_or_404(Artigo, slug=slug)
        return render_to_response('blog/artigo.html', locals(),
            context_instance=RequestContext(request))

Salve o arquivo. Feche o arquivo. Volte ao navegador, atualize com **F5** e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-artigo-nao-existe-com-erro-404/file/"/>
</p>

Pronto! Mais uma questão resolvida!

### Evitando duplicidades

Vamos agora testar outra coisa importante. Vá à seguinte URL para criar um novo artigo:

    http://localhost:8000/admin/blog/artigo/add/
    

Preencha os campos assim:

- Título: **"Olá mãe! Estou vivo!"**
- Conteúdo: **"Outro artigo com o mesmo título só pra ver no que dá"**

Clique sobre o botão **"Salvar"**.

Agora tente carregar a URL do artigo:

    http://localhost:8000/artigo/ola-mae-estou-vivo/
    

Veja o que aparece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-2-artigos-com-o-mesmo-titulo/file/"/>
</p>

A mensagem é bastante clara:

    MultipleObjectsReturned at /artigo/ola-mae-estou-vivo/
    get() returned more than one Artigo -- it returned 2! Lookup parameters were {'slug': u'ola-mae-estou-vivo'}

Resumindo: existem dois artigos com o **mesmo slug**, e o Django não sabe qual dos dois trazer! Só há uma forma de resolver isso: garantindo que não vão existir dois artigos com o mesmo slug.

Então, na pasta da aplicação **"blog"**, abra o arquivo **"models.py"** para edição e localize a seguinte linha:

        slug = models.SlugField(max_length=100, blank=True)
    

Modifique para ficar assim:

        slug = models.SlugField(max_length=100, blank=True, unique=True)
    

Com **"unique=True"**, o campo passa a ser tratado no banco de dados como **"índice único"**, para garantir que de que não haverão dois registros com um mesmo valor repetido nesse campo.

Mas só isso não é suficiente. Localize este trecho de código:

    def artigo_pre_save(signal, instance, sender, **kwargs):
        instance.slug = slugify(instance.titulo)

Ele está muito vulnerável. Podemos melhorar isso para ajustar o slug caso ele já exista, permitindo que seu **título** seja repetido sem que seu **slug** o seja. Portanto, modifique esta função de signal para ficar assim:

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

Salve o arquivo. Vamos detalhar cada parte do que fizemos?

Aqui nós definimos que vamos gerar o slug somente se o campo estiver vazio:

        if not instance.slug:

Geramos o **slug** do **título** do artigo e o armazenamos em duas variáveis: **"slug"** e **"novo\_slug"**. Para quê isso? É porque logo à frente, podemos modificar o valor de **"novo\_slug"**, mantendo o valor original da variavel **"slug"** como referência:

            slug = slugify(instance.titulo)
            novo_slug = slug

Iniciamos uma variável **"contador"** com valor zero. Ela irá somar à variável **"slug"** em caso de haver duplicidade para acrescentar um número adicional ao final do slug:

            contador = 0
    

Aqui, teremos um **laço**, **enquanto** a quantidade de **Artigos** com o slug contido na variável **"novo\_slug"** for **maior que zero**, será incrementado o contador e atribuído à variável **"novo\_slug"**, para que a tentativa seja feita novamente:

            while Artigo.objects.filter(slug=novo_slug).exclude(id=instance.id).count() > 0:
                contador += 1
                novo_slug = '%s-%d'%(slug, contador)

Por fim, depois que sair do laço **"while"**, já com a variável **"novo\_slug"** com o valor ideal, este valor é atribuído ao campo **"slug"** do objeto:

            instance.slug = novo_slug
    

Vamos tomar como exemplo o caso do artigo com título **"Olá mãe! Estou vivo!"**:

1. O slug gerado será "ola-mae-estou-vivo";
 1. Se não forem encontrados outros artigos com o slug gerado, então ok;
1. Caso o contrário, acrescenta um número "1" ao final, e o novo slug passa a ser "ola-mae-estou-vivo-1";
 1. Se não forem encontrados outros artigos com o novo slug, então ok;
1. Caso o contrário, acrescenta um número "2" ao final, e o novo slug passa a ser "ola-mae-estou-vivo-2";
 1. Se não forem encontrados outros artigos com o novo slug, então ok;
1. Caso o contrário, acrescenta um número "3" ao final, e o novo slug passa a ser "ola-mae-estou-vivo-3";
1. E assim por diante...

No fim da história, o arquivo **"models.py"** todo ficou assim:

    from datetime import datetime
    from django.db import models
    from django.core.urlresolvers import reverse
    
    class Artigo(models.Model):
        class Meta:
            ordering = ('-publicacao',)
            
        titulo = models.CharField(max_length=100)
        conteudo = models.TextField()
        publicacao = models.DateTimeField(default=datetime.now, blank=True)
        slug = models.SlugField(max_length=100, blank=True, unique=True)
    
        def get_absolute_url(self):
            return reverse('blog.views.artigo', kwargs={'slug': self.slug})
    
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

Feche o arquivo. Volte ao navegador na página do novo artigo no **Admin**:

    http://localhost:8000/admin/blog/artigo/2/
    

Remova todo o conteúdo do campo **"slug"**, deixando-o **vazio** e clique sobre o botão **"Salvar"**. Agora volte à URL do artigo:

    http://localhost:8000/artigo/ola-mae-estou-vivo/
    

Veja que agora dá tudo certo:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-artigo-com-slug-unico/file/"/>
</p>

Tudo certo. E carregando esta outra URL:

    http://localhost:8000/artigo/ola-mae-estou-vivo-1/
    

Veja que tudo dá certo também:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/slug-artigo-com-slug-unico-2/file/"/>
</p>

## Então, e agora? Que tal um álbum de fotos?

Alatazan sente cada vez mais consistência no que aprende, mas uma coisa está muito clara para ele:

- Tudo isso é muito bacana, mas a cada dia que passa, cada página virada, percebo que há muito mais a aprender...

- Alatazan, isso é porque o que vemos aqui é apenas um exemplo dos vários recursos que o Django oferece para cada tema.

Cartola entrou complementando Nena:

- Sim, e falando de **signals**, por exemplo: existem vários deles. Hoje só usamos o **"pre\_save"** para classes de modelo, e citamos o **"post\_save"**, mas existem signals também para **exclusão**, para inicialização da classe e diversos outros. Você não pode ficar só aqui, no que falamos, há um universo todo ao nosso redor...

A vida é realmente irônica... Cartola pouco sabe que Alatazan conhece muito mais sobre a imensidão do universo do que qualquer terráqueo, mas...

- Tá, então podemos dizer que o resumo do que aprendemos hoje é este:

 - Slugs são identificadores para evitar endereços complexos e ajudar os usuários e mecanismos de busca a encontrarem nossas páginas;
 - Para criar um novo campo no banco de dados é preciso saber um pouquinho de SQL;

- Ou ter uma ferramenta gráfica para fazer isso... - interrompeu Nena.

Alatazan continuou:

 - Certo... uma URL de slug deve ter a expressão **[\w_-]+** para garantir que serão informados apenas slugs válidos;
 - Signals devem ser definidos no arquivo **"models.py"**. São funções, com argumentos específicos, que são **conectadas** aos signals. Elas serão executadas uma por uma quando aquele **evento** acontece;
 - Usar o método **get\_absolute\_url()** evita repetições;
 - Usar a função **reverse()** dentro do método **get\_absolute\_url()** é a forma ideal para evitar mais repetições;
 - Usar **get\_object\_or\_404()** ajuda muito a deixar as coisas mais claras para o usuário;
 - Quando a assinatura de uma **URL** é modificada, a **view** também deve ser ajustada a isso;
 - Para garantir que não haverão duplicações, devo usar **unique=True** e ajustar o signal com um código consistente e claro...
 - Ufa! É isso!

Cartola reagiu, empolgado:

- Aí, meu chapa, falou bonito hein! Está pegando o jeito no português!

E aí Nena tratou de encerrar o dia...

- Pois falando em bonito, amanhã vamos fazer uma nova aplicação, uma galeria de fotos para o seu blog!

<span class="no_print">
**Próximo capítulo: [Uma galeria de imagens simples e útil](/uma-galeria-de-imagens-simples-e-util/)**
</span>

