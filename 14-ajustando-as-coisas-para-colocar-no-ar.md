Nesse dia, caiu uma chuva daquelas que não têm nada a ver com o tempo nem com a época.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_3.gif" align="right"/>

Alatazan nem sabia disso, pois estava no planeta há poucas semanas. Mas ele sentiu um gostinho de vingança quando viu os panfletos de gás, supermercado, mãe-de-santo e outros mais derretendo na caixinha de correios.

Definitivamente: se havia uma coisa chatíssima na Terra eram esses malditos panfletos.

Alatazan compreendeu mais rápido que ninguém a idéia de **"colocar no ar"**. Em Katara, há empresas de publicidade que colocam milhares de propagandas no ar todos os dias.

O problema é que essas propagandas são pequenos **robôs** que se parecem com **moscas**, e ficam em grupos de 5 ou 6 por ali, perto dos portões como se não quisessem nada, assobiando e praticando um vôo manjado. Quando o infeliz morador sai de sua casa, é atacado por eles com as mais variadas propagandas. Às vezes essas pessoas entram em ataque de nervos e passam um bom tempo tentando acertar os robôs com tapas ou jornais, mas nunca se resolve, e no outro dia é a mesma coisa.

O pai de Alatazan trabalha na fábrica onde esses robôs são feitos, na seção onde eles são treinados, e ali eles usam uma capa frágil, resistente a coisa nenhuma. Depois do treinamento eles recebem uma capa resistente e flexível, que não permite que se derretam quando a chuva cai.

Só depois disso, podem ser *colocados no ar*.

## Vamos agora preparar o projeto

Para enviar o projeto a um servidor e colocá-lo no ar, é importante fazer algumas modificações para isso. No servidor, ele deve se comportar de forma mais leve, sem recursos de **depuração** e outras coisinhas.

Então, mão na massa!

Antes de mais nada, execute seu projeto clicando duas vezes sobre o arquivo **"executar.bat"** da pasta do projeto.

Primeiro de tudo, abra o arquivo **"settings.py"** da pasta do projeto para edição. Vamos pensar as **settings** do projeto na seguinte divisão:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/settings/file/"/>
</p>

Há algumas **settings** que são muito relativas ao ambiente ao qual seu projeto está no momento. Vamos dar o exemplo mais simples: o estado de **DEBUG**. Esta setting só deve ter valor **True** em ambientes de **teste ou desenvolvimento**, nunca em produção.

Outras settings já são resultado de uma definição fixa, somada a outra setting do caso citado acima. Um bom exemplo é a setting **MEDIA\_ROOT** que seria sempre a mesma pasta **"media"** da pasta do projeto, mas a pasta do projeto pode estar em um caminho no ambiente de desenvolvimento e em outro no servidor.

E há ainda as settings que dificilmente mudam, como **INSTALLED\_APPS** por exemplo.

A definição de quais settings são flexíveis e quais não o são variam de acordo com o projeto, mas em geral há um ponto divisor no arquivo **"settings.py"** que se adapta à maioria dos casos.

Localize a seguinte linha no arquivo **"settings.py"**:

    USE_I18N = True
    

Na maioria dos projetos, esta é a última linha de um grupo de settings que variam muito de acordo com a situação atual do projeto. Abaixo desta linha estão aquelas settings que se ajustam a elas, ou que não se ajustam a nada.

Portanto, vamos acrescentar um novo trecho de código abaixo dessa linha:

    from local_settings import *

Salve o arquivo e vá à janela do **MS-DOS** (ou **Console**, no caso do Linux) onde o projeto está rodando. Veja o que aconteceu:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/local_settings-invalida/file/"/>
</p>

O erro que surgiu é este:

    Error: Can't find the file 'settings.py' in the directory containing 'manage.py'.
    It appears you've customized things.
    You'll have to run django-admin.py, passing it your settings module.
    (If the file settings.py does indeed exist, it's causing an ImportError somehow.)

Ou seja, nosso projeto caiu e parou de funcionar, porque houve um erro do tipo **ImportError** no arquivo **"settings.py"**. Isso aconteceu porque o arquivo importado **"local\_settings.py"** não foi encontrado.

Pois agora volte ao arquivo **"settings.py"** e modifique o trecho de código que você acrescentou para ficar assim:

    try:
        from local_settings import *
    except ImportError:
        pass

Salve o arquivo. Execute o projeto novamente, clicando duas vezes sobre o arquivo **"executar.bat"** da pasta do projeto.

No Python, o **try/except** é um recurso que permite que você faça uma tentativa, e caso essa tentativa resulte em erro, você pode tratar esse erro sem que seu software seja afetado ou paralizado.

Em outras palavras nós tentamos importar todas as settings do arquivo **"local\_settings.py"**, mas caso ocorra algum **erro de importação** ( ImportError ), a situação será ignorada e o interpretador do Python deve seguir adiante como se nada tivesse acontecido. Veja que apenas erros de importação estão sendo suportados aqui, ou seja, qualquer outro tipo de erro terá o comportamento padrão de levantar uma exceção.

O erro de importação mais comum é o de o arquivo não existir.

O raciocínio é o seguinte: todas as settings do arquivo **"local\_settings.py"** serão importadas, substituindo o valor das settings que estão acima desse trecho de código. Se elas não tiverem sido declaradas no **"local\_settings.py"**, permanecem como estão.

Agora, na pasta do projeto, crie um novo arquivo chamado **"local\_settings.py"** com o seguinte código dentro:

    import os
    PROJECT_ROOT_PATH = os.path.dirname(os.path.abspath(__file__))
    
    LOCAL = True
    DEBUG = True
    TEMPLATE_DEBUG = DEBUG
    
    DATABASE_ENGINE = 'sqlite3'
    DATABASE_NAME = os.path.join(PROJECT_ROOT_PATH,'meu_blog.db')

Estas são as settings que fatalmente serão modificadas quando o site for levado para o servidor, portanto, ao trazê-las para cá, garantimos que na máquina local elas permanecerão como estão.

Salve o arquivo. Feche o arquivo.

Agora de volta ao arquivo **"settings.py"**, localize esta linha:

    DEBUG = True
    

Modifique-a para ficar assim:

    DEBUG = False
    

E acima dela, acrescente a seguinte linha:

    LOCAL = False
    

Ao mudar a setting **DEBUG** para **False**, desativamos o **estado de depuração** do projeto em suas configurações padrão, mas elas permanecerão funcionando localmente, pois no arquivo **"local\_settings.py"** elas foram mantidas como estavam.

Ok, por agora isso é suficiente.

Salve o arquivo. Feche o arquivo.

Agora abra o arquivo **"urls.py"** da pasta do projeto para edição e localize o seguinte techo de código:

        (r'^media/(.*)$', 'django.views.static.serve',
         {'document_root': settings.MEDIA_ROOT}),

É a nossa URL para arquivos estáticos, certo? **Remova** esse trecho de código.

Agora ao final do arquivo, acrescente as seguintes linhas:

    if settings.LOCAL:
        urlpatterns += patterns('',
            (r'^media/(.*)$', 'django.views.static.serve',
             {'document_root': settings.MEDIA_ROOT}),
        )

O que fizemos aí foi o seguinte: a URL **'^media/'** só deve existir se o projeto estiver em máquina **local**, ou seja, na sua máquina, em ambiente de desenvolvimento.

Isso é importante, pois o Django **não foi feito** para servir arquivos **estáticos** em servidores. No servidor esta tarefa deve ser deixada para um software que foi criado para isso: um **servidor web**, como o **Apache**, Lighttpd, IIS ou outro.

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
        (r'^contato/$', 'views.contato'),
        (r'^comments/', include('django.contrib.comments.urls')),
    )
    
    if settings.LOCAL:
        urlpatterns += patterns('',
            (r'^media/(.*)$', 'django.views.static.serve',
             {'document_root': settings.MEDIA_ROOT}),
        )

Salve o arquivo. Feche o arquivo.

A divisão que trabalhamos no arquivo **"settings.py"**, que você pôde observar no diagrama, é simples de se fazer e poderosa. Pois seguindo essa linha de raciocínio é possível não só configurar o projeto para diferenciar o **servidor em produção** de seu ambiente de **desenvolvimento**, como é útil também para ambientes de **homologação e testes**, situações de um projeto em **mais de um servidor** ou de **múltiplos sites** em um só projeto.

Tire um tempo para pensar nisso, observe o arquivo **"settings.py"** com atenção e note aquilo que poderia variar em situações distintas. Se for necessário, mova a setting desejada para antes ou depois do trecho de código que importa o módulo **"local\_settings.py"**.

**Atualização:** este bloco abaixo foi atualizado no dia 29/11/2008. Verifique se você já o aplicou.

Agora, para concluir, abra novamente o arquivo **"settings.py"** do projeto e localize a seguinte linha:

    ROOT_URLCONF = 'meu_blog.urls'
    

Agora a modifique para ficar assim:

    ROOT_URLCONF = 'urls'
    

Esta modificação é recomendável, pois assim seu projeto trabalha de uma forma transparente em relação à **PYTHONPATH** - o conjunto de pastas reconhecidos pelo Python em sua máquina virtual para encontrar pacotes e módulos. Desta forma, quando seu projeto for lançado ao servidor, vamos adicionar a pasta do projeto à **PYTHONPATH** e tudo estará resolvido.

Salve o arquivo. Feche o arquivo.

**Fim** do bloco atualizado.

Pronto. Os primeiros passos para levar o projeto ao servidor são esses.

## Vamos adiante?

Alatazan perguntou, só pra confirmar:

- Então o projeto tem comportamentos diferentes, dependendo se está no servidor ou em minha máquina local?

Ao que Cartola respondeu:

- Isso, e há um grupo de settings potencialmente adaptáveis a essas situações diferentes, relacionadas a caminhos de pastas, modo de depuração e outras coisas assim...

<img src="http://www.aprendendodjango.com/media/img/ilustracao_4.gif" align="right"/>

- E tem também aquelas que são flexíveis, pois recebem valores somados a valores de settings modificadas em linhas de código anteriores ou no arquivo **"local\_settings.py"**.

- Por último, há aquelas que dificilmente vão se modificar, pois estão na estrutura do projeto, como **MIDDLEWARE\_CLASSES** ou **INSTALLED\_APPS** por exemplo.

- Lembrando que o arquivo **"settings.py"** deve permanecer com as configurações **ideais** para o projeto, ou seja, as que serão usadas caso o arquivo **"local\_settings.py"** não exista.

- O que você precisa ficar ligado é que, ao levar o seu projeto para o servidor, o arquivo **"settings.py"** deverá permanecer idêntico, sem alterações, pois elas devem ser feitas **apenas** no arquivo **"local\_settings.py"**. Se ao levar seu projeto para o servidor, você fizer mudanças em outras partes do código, é muito provável que esteja no caminho errado. **Não se esqueça disso!**

O bate-bola de Cartola e Nena resumiu rapidamente o aprendizado do dia, e Alatazan notou aqui uma diferença fundamental de hoje para os dias anteriores: agora estavam entrando em uma fase mais **profissional** da coisa.

<span class="no_print">
**Próximo capítulo: [Infinitas formas de se fazer deploy](/infinitas-formas-de-se-fazer-deploy/)**
</span>

