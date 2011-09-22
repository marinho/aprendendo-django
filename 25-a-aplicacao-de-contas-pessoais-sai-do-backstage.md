Alatazan esticou suas pernas para relaxar um pouco na cama. Naquele dia não haveria aula e ele sabia o que isso significava.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_11.gif" align="right"/>

- Hoje eu dou uma cara para a minha aplicação web 2.0!

Nas várias vezes que acordou durante à noite ele pensou automaticamente em como faria as Contas Pessoais de seu site uma forma de ganhar dinheiro e em todas as vezes adormeceu enquanto hesitava entre anotar aquelas coisas e pensar mais um pouco.

Levantou-se, estalou as costas num movimento de fúria que faria qualquer cachorrinho tremer. Mas era só pra relaxar, nada mais...

Uma curta corrida e um pulinho, e as pontas dos pés se tocaram, quando ele percebeu que esse movimento era um tanto constrangedor no planeta onde estava vivendo.

Voltou ao chão, se refez e pegou a **conta telefônica**. Quantas páginas só pra dizer que ele devia pagar aquele valor no dia 15!

E ele se lembrou de que as contas eram mais, e algumas delas estavam um tanto desfolheadas, enfiadas como se quisessem saltar para fora de uma caixa de sapatos.

## Dando uma cara à aplicação de contas pessoais

Muito do que vamos ver hoje é apenas a confirmação do que já vimos em capítulos anteriores, mas nós vamos um pouco além.

A primeira coisa a fazer é executar o seu projeto. Clique duas vezes sobre o arquivo **"executar.bat"** da pasta do projeto e pronto. A primeira coisa está feita.

Dã... mas tá, isso não pode ser considerada uma coisa a fazer, pode?

Não importa, vamos começar abrindo o arquivo **"urls.py"** para edição, para incluir essa nova URL:

        (r'^contas/', include('contas.urls')),
    

Agora o arquivo inteiro ficou assim:

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
        (r'^contas/', include('contas.urls')),
    )
    
    if settings.LOCAL:
        urlpatterns += patterns('',
            (r'^media/(.*)$', 'django.views.static.serve',
             {'document_root': settings.MEDIA_ROOT}),
        )

Salve o arquivo. Feche o arquivo. Vá agora até a pasta da aplicação **"contas"** e crie o arquivo **"urls.py"**, com o seguinte código dentro:

    from django.conf.urls.defaults import *
    
    urlpatterns = patterns('contas.views',
        url('^$', 'contas', name='contas'),
    )

Salve o arquivo. Feche o arquivo. Agora crie mais um novo arquivo nesta pasta, chamado **"views.py"**, com o seguinte código dentro:

    from django.shortcuts import render_to_response
    from django.template import RequestContext
    
    from models import ContaPagar, ContaReceber, CONTA_STATUS_APAGAR
    
    def contas(request):
        contas_a_pagar = ContaPagar.objects.filter(
            status=CONTA_STATUS_APAGAR,
            )
        contas_a_receber = ContaReceber.objects.filter(
            status=CONTA_STATUS_APAGAR,
            )
        
        return render_to_response(
            'contas/contas.html',
            locals(),
            context_instance=RequestContext(request),
            )

Bom, o que temos que novo aqui?

Nesta linha de código, nós importamos as duas classes de contas: **"ContaPagar"** e **"ContaReceber"**. E também importamos a variável que representa o status **"A Pagar"** das contas.

    from models import ContaPagar, ContaReceber, CONTA_STATUS_APAGAR
    

Já este outro trecho de código carrega os objetos de contas a pagar e a receber, filtrados pelo campo **"status"**, que deve ter o valor contido na variável **"CONTA\_STATUS\_APAGAR"**. Isso significa que somente as contas que seu campo **"status"** contiver o valor informado serão retornadas para as variáveis **"contas\_a\_pagar"** e **"contas\_a\_receber"**:

        contas_a_pagar = ContaPagar.objects.filter(
            status=CONTA_STATUS_APAGAR,
            )
        contas_a_receber = ContaReceber.objects.filter(
            status=CONTA_STATUS_APAGAR,
            )


No mais, são as mesmas já conhecidas linhas de código.

Salve o arquivo. Feche o arquivo.

Agora precisamos criar o template **"contas/contas.html"** para esta view, certo? Pois ainda na pasta da aplicação **"contas"**, abra a pasta **"templates"** e crie uma nova pasta, chamada **"contas"**, e dentro dela um arquivo chamado **"contas.html"**, com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% load i18n %}
    
    {% block titulo %}{% trans "Contas Pessoais" %} - {{ block.super }}{% endblock %}
    {% block h1 %}{% trans "Contas Pessoais" %}{% endblock %}
    
    {% block conteudo %}
    <h2>{% trans "Contas a Pagar" %}</h2>
    
    <ul>
      {% for conta in contas_a_pagar %}
      <li><a href="{{ conta.get_absolute_url }}">{{ conta }}</a></li>
      {% endfor %}
    </ul>
    
    <h2>{% trans "Contas a Receber" %}</h2>
    
    <ul>
      {% for conta in contas_a_receber %}
      <li><a href="{{ conta.get_absolute_url }}">{{ conta }}</a></li>
      {% endfor %}
    </ul>
    {% endblock conteudo %}

Observe bem para notar que não há novidades ali. Temos duas listagens: uma de **Contas a Pagar** e outra de **Contas a Receber**, usando as variáveis que declaramos na _view_.

Salve o arquivo. Feche o arquivo.

Agora, vá ao navegador e carregue a seguinte URL:

    http://localhost:8000/contas/
    

Veja como ela é carregada:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-front-end-primeira-versao/file/"/>
</p>

Como você pode ver, fizemos progresso, mas temos que melhorar muito. Agora vá até a pasta da aplicação **"contas"** e abra o arquivo **"models.py"** para edição. Localize a seguinte linha:

    from django.utils.translation import ugettext_lazy as _
    

Abaixo dela, acrescente esta:

    from django.core.urlresolvers import reverse
    

Agora localize este linha:

        descricao = models.TextField(blank=True)
    

Acrescente esta abaixo dela:

        def __unicode__(self):
            data_vencto = self.data_vencimento.strftime('%d/%m/%Y')
            valor = '%0.02f'%self.valor
            return '%s - %s (%s)'%(valor, self.pessoa.nome, data_vencto)

Nós acrescentamso o método **"\_\_unicode\_\_()"** aqui porque ele será idêntico tanto para Contas a Pagar quanto para Contas a Receber, e a classe **"Conta"** concentra tudo o que há de idêntico entre as classes **"ContaPagar"** e **"ContaReceber"**.

Na linha seguinte, nós formatamos a **data de vencimento** em um formato amigável ( **"dia/mês/ano"** ), usando o método **"strftime()"**:

            data_vencto = self.data_vencimento.strftime('%d/%m/%Y')
    

E a seguir, formatamos novamente, mas dessa vez, ao invés de ser uma data, formatamos um valor monetário, garantindo duas casas decimais, veja:

            valor = '%0.02f'%self.valor
    

E por fim, juntamos tudo em outra formatação, para que a representação textual desse objeto fique no formato **"valor - pessoa (data de vencimento)"**:

            return '%s - %s (%s)'%(valor, self.pessoa.nome, data_vencto)
    

Quanta formatação hein?

Agora localize a classe **"ContaPagar"**:

    class ContaPagar(Conta):
        def save(self, *args, **kwargs):
            self.operacao = CONTA_OPERACAO_DEBITO
            super(ContaPagar, self).save(*args, **kwargs)

E acrescente esse bloco de código ao final dela:

        def get_absolute_url(self):
            return reverse('conta_a_pagar', kwargs={'conta_id': self.id})

O que fizemos aí você já conhece: usamos a função **"reverse()"** para indicar a URL de exibição desta Conta a Pagar, que tem o nome de **"conta\_a\_pagar"**. E agora vamos fazer o mesmo com a Conta a Receber:

Localize a classe **"ContaReceber"**:

    class ContaReceber(Conta):
        def save(self, *args, **kwargs):
            self.operacao = CONTA_OPERACAO_CREDITO
            super(ContaReceber, self).save(*args, **kwargs)

E acrescente esse bloco de código ao final dela:

        def get_absolute_url(self):
            return reverse('conta_a_receber', kwargs={'conta_id': self.id})

O código é muito semelhante ao da Conta a Pagar, mas desta vez indicamos a URL com o nome **"conta\_a\_receber"**.

Salve o arquivo. Feche o arquivo.

E como indicamos as URLs **"conta\_a\_pagar"** e **"conta\_a\_receber"**, agora precisamos que elas existam, certo? Pois então abra argora o arquivo **"urls.py"** da aplicação **"contas"** para edição e acrescente as seguintes URLs:

        url('^pagar/(?P<conta_id>\d+)/$', 'conta', {'classe': ContaPagar},
            name='conta_a_pagar'),
        url('^receber/(?P<conta_id>\d+)/$', 'conta', {'classe': ContaReceber},
            name='conta_a_receber'),

Você pode notar que ambas as URLs fazem uso do mesmo argumento para levar o código da conta: **"conta\_id"**. As duas também apontam para a mesma _view_: **"conta"**, mas daí em diante as coisas mudam, pois cada uma indica o parâmetro **"classe"** para sua respectiva classe de modelo e cada uma também possui um **nome** diferente.

Indicar parâmetros a uma URL é uma medida prática para evitar repetição de código, pois ambas as classes possuem um comportamento muito semelhante uma da outra e não há necessidade de duplicar coisas que podem ser generalizadas entre elas.

Mas falta uma coisa: importar as classes **"ContaPagar"** e **"ContaReceber"**. Para isso localize a primeira linha do arquivo:

    from django.conf.urls.defaults import *
    

E acrescente esta abaixo dela:

    from models import ContaPagar, ContaReceber
    

Salve o arquivo. Feche o arquivo. Agora vamos criar a _view_ **"conta"**.

Para isso, ainda na pasta da aplicação **"contas"**, abra o arquivo **"views.py"** e acrescente o seguinte trecho de código ao seu final:

    def conta(request, conta_id, classe):
        conta = get_object_or_404(classe, id=conta_id)
        return render_to_response(
            'contas/conta.html',
            locals(),
            context_instance=RequestContext(request),
            )

Agora para entender melhor como essa view funciona, observe as duas primeiras linhas:

    def conta(request, conta_id, classe):
        conta = get_object_or_404(classe, id=conta_id)

Veja que aqui temos o argumento **"classe"**, que recebe ora a classe **"ContaPagar"**, ora **"ContaReceber"**, e quem atribui a classe a esse argumento está no código que escrevemos lá atrás, veja novamente:

        url('^pagar/(?P<conta_id>\d+)/$', 'conta', {'classe': ContaPagar},
            name='conta_a_pagar'),
        url('^receber/(?P<conta_id>\d+)/$', 'conta', {'classe': ContaReceber},
            name='conta_a_receber'),

Observe que além do argumento **"conta\_id"**, temos também o dicionário com um parâmetro dentro: **"classe"**, atribuindo a classe de modelo que esta URL indica.

Está claro? Observe com atenção, pois esta é uma prática que muitas vezes ajuda no seu dia-a-dia...

Agora localize a primeira linha do arquivo:

    from django.shortcuts import render_to_response
    

E modifique-a, acrescentando a função **"get\_object\_or\_404"** ao seu final, assim:

    from django.shortcuts import render_to_response, get_object_or_404
    

Salve o arquivo. Feche o arquivo. Já quer testar? Espere... ainda temos uma coisa a fazer.

Na pasta da aplicação **"contas"**, abra a pasta **"templates/contas"** e crie um novo arquivo, chamado **"conta.html"** com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% load i18n %}
    
    {% block titulo %}{{ conta }} - {{ block.super }}{% endblock %}
    {% block h1 %}{{ conta }}{% endblock %}
    
    {% block conteudo %}
    <ul>
      <li>Valor: {{ conta.valor }}</li>
      <li>Vencimento: {{ conta.data_vencimento }}</li>
      <li>Pessoa: {{ conta.pessoa }}</li>
      <li>Histórico: {{ conta.historico }}</li>
      <li>Status: {{ conta.status }}</li>
    </ul>
    {% endblock conteudo %}

Salve o arquivo (lembre-se de salvar no formato **"UTF-8"** caso esteja usando **Bloco de Notas no Windows**). Feche o arquivo.

Volte ao navegador e atualize a página com a tecla **F5**. Veja como ficou agora:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-front-end-com-titulos-corretos/file/"/>
</p>

Agora clique sobre a **Conta a Receber** e veja como ela é carregada:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-conta-a-receber-no-front-end/file/"/>
</p>

Estamos caminhando bem, não estamos? Vamos agora dar uma melhorada nessa página?

Faça assim: abra para edição novamente o arquivo **"conta.html"** da pasta **"contas/templates/contas"**, partindo da pasta do projeto e localize este bloco de código:

      <li>Valor: {{ conta.valor }}</li>
      <li>Vencimento: {{ conta.data_vencimento }}</li>
      <li>Pessoa: {{ conta.pessoa }}</li>
      <li>Histórico: {{ conta.historico }}</li>
      <li>Status: {{ conta.status }}</li>

Modifique para ficar assim:

      <li>{% trans "Valor" %}: {{ conta.valor|floatformat:2 }}</li>
      <li>{% trans "Vencimento" %}: {{ conta.data_vencimento|date:"d/m/Y" }}</li>
      <li>{% trans "Pessoa" %}: <a href="{{ conta.pessoa.get_absolute_url }}">{{ conta.pessoa }}</a></li>
      <li>{% trans "Histórico" %}: <a href="{{ conta.historico.get_absolute_url }}">{{ conta.historico }}</a></li>
      <li>{% trans "Status" %}: {{ conta.get_status_display }}</li>

Vamos passar parte por parte:

Na primeira linha, nós formatamos o **valor** da conta para **sempre** ser exibido com **duas casas decimais**. Elas serão **arredondadas** caso sejam **mais que duas**:

      <li>{% trans "Valor" %}: {{ conta.valor|floatformat:2 }}</li>
    

Em seguida, nós formatamos a **data de vencimento** no formato **"dia/mês/ano"**:

      <li>{% trans "Vencimento" %}: {{ conta.data_vencimento|date:"d/m/Y" }}</li>
    

Nas duas linhas seguintes, nós definimos um link para os campos **"Pessoa"** e **"Histórico"**:

      <li>{% trans "Pessoa" %}: <a href="{{ conta.pessoa.get_absolute_url }}">{{ conta.pessoa }}</a></li>
      <li>{% trans "Histórico" %}: <a href="{{ conta.historico.get_absolute_url }}">{{ conta.historico }}</a></li>

E em seguida... você notou que no campo **"Status"** foi mostrada uma letra **"a"**? Pois é, isso não é nada agradável... vamos mostrar seu rótulo correto! Para isso, usamos o método **"get\_status\_display"**, que trata-se de um método calculado, construído em memória para o campo **"status"**.

      <li>{% trans "Status" %}: {{ conta.get_status_display }}</li>
    

Para cada um dos campos que possuem o argumento **"choices"** é oferecido um método com o nome assim: **"get\_NOMEDOCAMPO\_display()"**, que pode ser usado tanto em templates (removendo os parênteses) como no código em Python normalmente.

Agora vamos fazer mais. Abaixo do bloco que modificamos, acrescente mais este:

      {% ifequal conta.status "p" %}
      <li>{% trans "Pagamento" %}: {{ conta.data_pagamento|date:"d/m/Y" }}</li>
      {% endifequal %}

Este código faz uma condição para ser exibido: é necessário que o **status** da conta seja **"Pago"**, representado pela condição da template tag **"{% ifequal conta.status "p" %}"**.

Agora abaixo desta linha:

    </ul>
    

Acrescente mais estas linhas código:

    {{ conta.descricao|linebreaks }}
    
    {% if conta.pagamentos.count %}
    <h2>{% trans "Pagamentos" %}</h2>
    
    <table>
      <tr>
        <th>{% trans "Data" %}</th>
        <th>{% trans "Valor" %}</th>
      </tr>
    
      {% for pagamento in conta.pagamentos %}
      <tr>
        <td>{{ pagamento.data_pagamento|date:"d/m/Y" }}</td>
        <td>{{ pagamento.valor|floatformat:2 }}</td>
      </tr>
      {% endfor %}
    </table>
    {% endif %}

Puxa, mas quanta coisa!

Veja, nesta linha nós exibimos a **Descrição** da conta:

    {{ conta.descricao|linebreaks }}
    

E aqui, iniciamos um bloco que será exibido somente se houverem sido feitos **pagamentos** na conta. A condição é: se a **quantidade** de **pagamentos** for diferente de zero (valor verdadeiro), o bloco é exibido.

    {% if conta.pagamentos.count %}
    

E todo o restante do código que escrevemos é uma tabela com sua linha de cabeçalho e um **laço** através da template tag **{% for pagamento in conta.pagamentos %}** que vai exibir uma linha na tabela para cada um dos pagamentos.

Salve o arquivo. Feche o arquivo. Volte ao navegador e atualize a página. Veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-conta-a-receber-formatada/file/"/>
</p>

Mas que bacana, tudo formatadinho... mas acontece que o atributo **"conta.pagamentos"** não existe, então, devemos criá-lo agora!

Abra o arquivo **"models.py"** da pasta da aplicação **"contas"** para edição e localize a seguinte linha. Ela faz parte da classe **"ContaPagar"**:

            return reverse('conta_a_pagar', kwargs={'conta_id': self.id})
    

Abaixo dela, acrescente o seguinte código:

        def pagamentos(self):
            return self.pagamentopago_set.all()

Isso vai fazê-la retornar a lista de pagamentos da **Conta a Pagar**.

O mesmo deve ser feito para a classe **"ContaReceber"**. Localize esta outra linha:

            return reverse('conta_a_receber', kwargs={'conta_id': self.id})
    

E acrescente estas abaixo dela:

        def pagamentos(self):
            return self.pagamentorecebido_set.all()

Salve o arquivo. Feche o arquivo. Agora volte ao navegador e atualize com **F5**:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-conta-a-receber-com-pagamentos/file/"/>
</p>

Muito legal! Estamos caminhando a passos largos!

Agora precisamos de um **formulário** para fazer esses pagamentos!

### Criando um formulário dinâmico para pagamentos

Então vamos lá! Volte a editar o arquivo **"conta.html"** da pasta **"contas/templates/contas"** partindo da pasta do projeto, e localize esta linha aqui:

    {% endblock conteudo %}
    

Acima dela, acrescente estas linhas de código:

    {% ifequal conta.status "a" %}
    <hr/>
    <h2>{% trans "Novo pagamento" %}</h2>
    
    <form action="{{ conta.get_absolute_url }}pagar/" method="post">
      <table>
        {{ form_pagamento }}
      </table>
    
      <input type="submit" value="{% trans "Salvar pagamento" %}"/>
    </form>
    {% endifequal %}

Na primeira linha nós estabelecemos uma condição: só exibimos o formulário de pagamento se a conta ainda tiver seu **Status** como **"A Pagar"** (valor **"a"**):

    {% ifequal conta.status "a" %}
    

Traçamos uma linha para separar visualmente o formulário do restante da página:

    <hr/>
    

Declaramos uma tag HTML **&lt;form&gt;** que direciona o formulário para a URL da **conta** somada de **"pagar/"**. Observe também que ele utiliza o método **"post"**:

    <form action="{{ conta.get_absolute_url }}pagar/" method="post">
    

E a outra coisa a observar no que fizemos é o uso do formulário dinâmico da variável **"form\_pagamento"**:

        {{ form_pagamento }}
    

Salve o arquivo. Feche o arquivo. O próximo passo agora é fazer a variável **"form\_pagamento"** existir!

Na pasta da aplicação **"contas"**, abra o arquivo **"views.py"** para edição e localize a seguinte linha de código:

        conta = get_object_or_404(classe, id=conta_id)
    

Acrescente esta linha abaixo dela:

        form_pagamento = FormPagamento()
    

Agora localize esta outra linha:

    from models import ContaPagar, ContaReceber, CONTA_STATUS_APAGAR
    

Acrescente abaixo dela:

    from forms import FormPagamento
    

Agora salve o arquivo. Feche o arquivo. Vamos criar a classe **"FormPagamento"**?

Na pasta da aplicação **"contas"**, crie um novo arquivo chamado **"forms.py"** com o seguinte código dentro:

    from datetime import date
    from django import forms
    
    class FormPagamento(forms.Form):
        valor = forms.DecimalField(max_digits=15, decimal_places=2)

Humm... eu acho que não há novidades aí...

Fizemos a importação dos elementos que vamos precisar no arquivo: o objeto **"date"** para nos retornar a **data atual** e o pacote **"forms"** para criarmos o formulário dinâmico:

    from datetime import date
    from django import forms

Criamos a classe do formulário dinâmico e declaramos seu único campo: **"valor"**, seguindo o mesmo tipo de campo que seguimos na classe de modelo **"Pagamento"**:

    class FormPagamento(forms.Form):
        valor = forms.DecimalField(max_digits=15, decimal_places=2)

Salve o arquivo. Feche o arquivo. Agora volte ao navegador e atualize a página da conta com **F5**. Veja:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-conta-a-receber-com-form-de-pagamento/file/"/>
</p>

Show! Nosso formulário já está aí... mas ainda sem vida hein? Pois então vamos dar vida a ele agora!

Abra o arquivo **"urls.py"** da pasta da aplicação **"contas"** e acrescente estas duas novas URLs:

        url('^pagar/(?P<conta_id>\d+)/pagar/$', 'conta_pagamento',
            {'classe': ContaPagar},
            name='conta_a_pagar_pagamento'),
        url('^receber/(?P<conta_id>\d+)/pagar/$', 'conta_pagamento',
            {'classe': ContaReceber},
            name='conta_a_receber_pagamento'),

Repare que seguimos o mesmo raciocínio das URLs da Conta a Pagar e da Conta a Receber, mas desta vez acrescentamos a palavra **"pagar/"** à expressão regular e a palavra **"\_pagamento"** ao nome da _view_ e ao nome da URL. Não há segredo na sopa de letrinhas!

Salve o arquivo. Feche o arquivo.

Agora vamos criar essa nova _view_! Para isso abra o arquivo **"views.py"** da mesma pasta e acrescente estas linhas de código ao final:

    def conta_pagamento(request, conta_id, classe):
        conta = get_object_or_404(classe, id=conta_id)
    
        if request.method == 'POST':
            form_pagamento = FormPagamento(request.POST)
    
            if form_pagamento.is_valid():
                form_pagamento.salvar_pagamento(conta)
            
        return HttpResponseRedirect(request.META['HTTP_REFERER'])

Bom, veja só o que nós fizemos:

Aqui na declaração da função usamos o mesmo raciocínio da _view_ **"conta()"**. Carregamos o objeto **"conta"** com base na classe informada na definição da URL:

    def conta_pagamento(request, conta_id, classe):
        conta = get_object_or_404(classe, id=conta_id)

O próximo passo usou o mesmo tipo de código que usamos no formulário dinâmico de **Contato**, lembra-se do **capítulo 10**? Só aceitamos envio de dados usando método HTTP **"post"**, que aliás, usamos quando declaramos a tag HTML **&lt;form&gt;** neste mesmo capítulo.

        if request.method == 'POST':
            form_pagamento = FormPagamento(request.POST)

Em seguida, fazemos a **validação** do formulário dinâmico, usando o método **"is\_valid()"**, isso não só valida os dados informados pelo usuário como também habilita o atributo **"cleaned\_data"** do formulário. Depois disso o método **"salvar\_pagamento()"** é chamado para o objeto **"conta"**:

            if form_pagamento.is_valid():
                form_pagamento.salvar_pagamento(conta)

E por fim, a _view_ retorna um redirecionamento, usando **"HttpResponseRedirect"** e indicando a URL anterior como destino:

        return HttpResponseRedirect(request.META['HTTP_REFERER'])

O atributo **"META"** da **request** vem recheado de informações do protocolo HTTP. Isso inclui dados sobre o navegador que originou a requisição, sistema operacional, IP e outras informações. Entre elas está o item **"HTTP\_REFERER"**, que carrega a URL anterior, a que o usuário estava quando clicou no botão **"Salvar pagamento"**.

E agora, para dizer ao Python quem é o tal **"HttpResponseRedirect"**, localize esta linha:

    from django.template import RequestContext
    

E acrescente esta abaixo dela:

    from django.http import HttpResponseRedirect
    

Salve o arquivo. Feche o arquivo. O próximo passo agora é fazer existir o método **"salvar\_pagamento()"** do formulário **"FormPagamento"**.

Para isso, abra o arquivo **"forms.py"** da pasta da aplicação **"contas"** para edição e localize a classe **"FormPagamento"**:

    class FormPagamento(forms.Form):
        valor = forms.DecimalField(max_digits=15, decimal_places=2)

Ao final dela, acrescente o método do qual precisamos:

        def salvar_pagamento(self, conta):
            return conta.lancar_pagamento(
                data_pagamento=date.today(),
                valor=self.cleaned_data['valor'],
                )

Veja que o método **"salvar\_pagamento()"** recebe o argumento **"conta"** e chama outro método, agora do objeto **"conta"** para gravar o pagamento. O novo método se chama **"lancar\_pagamento"** e recebe os argumentos **"data\_pagamento"** e **"valor"** com a **data atual** e o **valor** informado pelo usuário.

Porquê fazemos isso? Porque assim podemos mantêr esse formulário útil tanto para Contas a Pagar quanto para Contas a Receber, mantemos também as devidas responsabilidades separadas e nos concentramos apenas em repassar as informações das quais o formulário dinâmico é responsável, respeitando as camadas.

Salve o arquivo. Feche o arquivo.

Agora na mesma pasta abra o arquivo **"models.py"** para edição e localize este trecho de código:

        def pagamentos(self):
            return self.pagamentorecebido_set.all()

Abaixo disso, acrescente o método que acabamos de citar:

        def lancar_pagamento(self, data_pagamento, valor):
            return PagamentoRecebido.objects.create(
                conta=self,
                data_pagamento=data_pagamento,
                valor=valor,
                )

Veja que o método **"lancar\_pagamento()"** recebe os argumentos **"data\_pagamento"** e **"valor"** e cria o novo objeto de **"PagamentoRecebido"** com o valor dos argumentos e a referência à **conta** - que se trata do próprio objeto do método: **self**.

Faça o mesmo com a classe **"ContaPagar"**. Para isso localize o trecho de código:

        def pagamentos(self):
            return self.pagamentopago_set.all()

Acrescente este trecho abaixo dele:

        def lancar_pagamento(self, data_pagamento, valor):
            return PagamentoPago.objects.create(
                conta=self,
                data_pagamento=data_pagamento,
                valor=valor,
                )

A única diferença aqui é que criamos um novo objeto da classe **"PagamentoPago"** ao invés da classe **"PagamentoRecebido"**.

Salve o arquivo. Feche o arquivo.

Agora volte ao navegador, informe um valor no campo **"valor"**, clique sobre o botão **"Salvar pagamento"** e veja o que acontece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-salvando-pagamento-usando-o-formulario/file/"/>
</p>

Está lá o arquivo que você acrescentou!

### Trabalhando com agrupamentos

Vamos agora criar uma página para exibir uma listagem com todas as Contas, organizadas em **páginas**.

Na pasta **"contas/templates/contas"** partindo da pasta do projeto, abra o arquivo **"contas.html"** para edição e localize esta linha:

    <h2>{% trans "Contas a Receber" %}</h2>
    

**Acima** dela, acrescente esta linha:

    <a href="{% url contas_a_pagar %}">{% trans "Ver todas" %}</a>
    

O link acima vai permitir que o usuário clique sobre ele para ver **todas as contas a pagar"**. Para fazer o mesmo com as contas a receber, localize esta outra linha de código:

    {% endblock conteudo %}
    

E acima dela acrescente esta:

    <a href="{% url contas_a_receber %}">{% trans "Ver todas" %}</a>
    

Salve o arquivo. Feche o arquivo. Precisamos agora criar as URLs com os nomes **"contas\_a\_pagar"** e **"contas\_a\_receber"** na aplicação **"contas"**. Para isso, vá até a pasta da aplicação **"contas"** e abra o arquivo **"urls.py"** para edição. Acrescente as novas URLs:

        url('^pagar/$', 'contas_por_classe',
            {'classe': ContaPagar, 'titulo': 'Contas a Pagar'},
            name='contas_a_pagar'),
        url('^receber/$', 'contas_por_classe',
            {'classe': ContaReceber, 'titulo': 'Contas a Receber'},
            name='contas_a_receber'),

Veja que indicamos ambas para a _view_ **"contas\_por\_classe"**, mas cada uma possui sua **classe** de modelo e seu nome respectivo.

A novidade está no parâmetro **"titulo"**, que leva o título da página. Logo vamos saber porquê.

Salve o arquivo. Feche o arquivo.

Agora abra o arquivo **"views.py"** da mesma pasta para edição e acrescente ao final o bloco de código abaixo:

    def contas_por_classe(request, classe, titulo):
        contas = classe.objects.order_by('status','data_vencimento')
        titulo = _(titulo)
        return render_to_response(
            'contas/contas_por_classe.html',
            locals(),
            context_instance=RequestContext(request),
            )

Como você pode ver, a _view_ carrega todas as contas para a variável **"contas"** usando a QuerySet de **"classe.objects.order_by('status','data\_vencimento')"**, e por fim utiliza o template **"contas/contas\_por\_classe.html"** para retornar.

        contas = classe.objects.order_by('status','data_vencimento')
    

O método **"order_by('status','data\_vencimento')"** define que a QuerySet deve retornar os objetos **ordenados** pelos campos **"status"** e **"data\_vencimento"**, ambos em ordem ascendente.

E o argumento **"titulo"** é atribuído a uma variável com o mesmo nome, mas fazendo uso da função de **internacionalização**. Isso é porque o **título da página** deve variar dependendo da classe de modelo, veja:

        titulo = _(titulo)
    

Ainda precisamos importar a função de internacionalização, não é? Então encontre esta linha:

    from django.http import HttpResponseRedirect
    

E acrescente esta abaixo dela:

    from django.utils.translation import ugettext as _
    

Salve o arquivo. Feche o arquivo.

Vamos agora criar o novo template! Volte à pasta **"contas/templates/contas"** e crie um arquivo chamado **"contas\_por\_classe.html"** com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% load i18n %}
    
    {% block titulo %}{{ titulo }} - {{ block.super }}{% endblock %}
    {% block h1 %}{{ titulo }}{% endblock %}
    
    {% block conteudo %}{{ block.super }}
    
    {% for conta in contas %}
    <div>
      <a href="{{ conta.get_absolute_url }}">{{ conta }}</a>
    </div>
    {% endfor %}
    
    {% endblock conteudo %}

Você pode ver que usamos a variável **"titulo"** nos _blocks_ **"titulo"** e **"h1"**. No mais, fazemos um laço com a template tag **{% for conta in contas %}**.

Salve o arquivo. Feche o arquivo.

Volte ao navegador e carregue a seguinte URL:

    http://localhost:8000/contas/
    

Veja como a página ficou agora:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-links-para-as-paginas-de-contas/file/"/>
</p>

Bacana! Agora clique sobre um dos links **"Ver todas"**, e veja:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-listagem-de-contas-a-pagar/file/"/>
</p>

Ótimo! Mas que tal agrupar as contas pelo campo de **Status**?

Então volte a editar o arquivo **"contas\_por\_classe.html"** da pasta **"contas/templates/contas"** localize este trecho de código:

    {% for conta in contas %}
    <div>
      <a href="{{ conta.get_absolute_url }}">{{ conta }}</a>
    </div>
    {% endfor %}

Acrescente este acima dele:

    {% regroup contas by get_status_display as contas %}
    

Salve e volte ao navegador, atualize com **F5** e veja:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-com-regroup-mas-lista-errada/file/"/>
</p>

Humm... esquisito né? Porquê isso?

Veja, a template tag **{% regroup %}** agrupa uma lista de objetos por um de seus campos, atributos e métodos. Os objetos de contas possuem um método chamado **"get\_status\_display"** que retorna o rótulo do campo **"status"**, certo? Pois então a linha que criamos **reagrupa** a variável **"contas"** pelos valores do método **"get\_status\_display"** para um nome de variável: **"contas"**.

Veja que a variável que usamos como base tem o mesmo nome que a variável que damos saída: **"contas"**. Isso significa que a antiga será substituída pela nova, com o agrupamento.

A partir daí a variável **"contas"** passa a conter uma **lista de agrupamentos**. Cada grupo é um dicionário com dois itens:

* **"grouper"** - que carrega o valor agrupador - que neste caso é o rótulo do status da conta;
* **"list"** - que carrega a lista de objetos que se enquadraram dentro daquele grupo.

Agora volte ao arquivo que estamos editando e modifique este bloco de código:

    {% for conta in contas %}
    <div>
      <a href="{{ conta.get_absolute_url }}">{{ conta }}</a>
    </div>
    {% endfor %}

Para ficar assim:

    {% for grupo in contas %}
    <h2>{{ grupo.grouper }}</h2>
    
    {% for conta in grupo.list %}
    <div>
      <a href="{{ conta.get_absolute_url }}">{{ conta }}</a>
    </div>
    {% endfor %}
    
    {% endfor %}

Veja que ali fazemos **dois laços**:

O primeiro passa a ser o laço para os grupos:

    {% for grupo in contas %}
    <h2>{{ grupo.grouper }}</h2>

A variável **"grupo.grouper"** é exibida como **título** do grupo.

Em seguida, esta linha muda para ser um laço em **"grupo.list"**, que contém a lista de contas a partir de agora:

    {% for conta in grupo.list %}
    

No mais, as coisas permanecem como estão, apenas acrescentando o fechamento dos laços.

Salve o arquivo. Feche o arquivo. Atualize o navegador com **F5** e veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-lista-de-contas-a-pagar-agrupada/file/"/>
</p>

Viu? Agora que tal criar mais uma porção de contas no Admin, mudar seus **status** e voltar aqui para ver como fica? Veja um exemplo:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-admin-com-varias-contas-a-pagar/file/"/>
</p>

E a listagem equivalente na página que acabamos de construir:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-lista-de-contas-a-pagar-com-muitos/file/"/>
</p>

É isso aí!

### Trabalhando com paginação

Agora que tal organizar essas contas por **páginas de 5 em 5**? Então vamos lá!

O Django possui uma classe chamada *Paginator* que organiza listas de objetos de forma que possam ser divididas por páginas.

Na pasta da aplicação **"contas"**, abra o arquivo **"views.py"** para edição e localize a linha de código abaixo:

        contas = classe.objects.order_by('status','data_vencimento')
    

Acrescente estas linhas de código abaixo dela:

        paginacao = Paginator(contas, 5)
        pagina = paginacao.page(request.GET.get('pagina', 1))

O que fizemos aí foi instanciar o controlador **"paginacao"** da lista **"contas"** com páginas de 5 objetos e na segunda linha carregamos a página atual com base no número da página contido no parâmetro **"pagina"**.

Agora precisamos de importar a classe **"Paginator"**. Para isso, localize esta linha de código:

    from django.utils.translation import ugettext as _
    

Acrescente a linha abaixo para importar a classe **"Paginator"**:

    from django.core.paginator import Paginator
    

Salve o arquivo. Feche o arquivo.

Agora vamos ao template para habilitar a nossa paginação. Abra para edição o arquivo **"contas\_por\_classe.html"** da pasta **"contas/templates/contas"**, partindo da pasta do projeto. Localize esta linha:

    {% regroup contas by get_status_display as contas %}
    

Modifique a linha para ficar assim:

    {% regroup pagina.object_list by get_status_display as contas %}
    

Observe que trocamos o elemento **"contas"** pelo **"pagina.object\_list"** e o resto permaneceu inalterado. A variável **"pagina.object\_list"** traz consigo a **lista de contas** para a página atual.

Agora localize esta outra linha:

    {% endblock conteudo %}
    

Acima dela, acrescente este trecho de código:

     <hr/>
     <div class="paginacao">
       Paginas:
       {% for pagina_numero in paginacao.page_range %}
       <a href="{{ request.path }}?pagina={{ pagina_numero }}">{{ pagina_numero }}</a>
       {% endfor %}
     </div>

O trecho de código acima faz um laço em **"paginacao.page\_range"** - a variável que carrega consigo a lista de páginas disponíveis para paginação. Dentro do laço o trabalho é exibir o número da página com seu respectivo link.

O link é composto pela URL atual em **"request.path"**, mais o parametro **"?pagina="**, somado ao número da página, em **"pagina\_numero"**.

Mas aí há uma questão: de onde saiu a variável **"request"** que citamos em **{{ request.path }}**? É preciso fazer uma pequena modificação para que ela ganhe vida no template de forma segura!

Salve o arquivo. Feche o arquivo. Vá até a pasta do projeto e abra o arquivo **"settings.py"** para edição. Localize a seguinte linha:

    TEMPLATE_DIRS = (
    

Acima dela, acrescente o seguinte trecho de código:

    TEMPLATE_CONTEXT_PROCESSORS = (
        'django.core.context_processors.auth',
        'django.core.context_processors.debug',
        'django.core.context_processors.i18n',
        'django.core.context_processors.media',
        'django.core.context_processors.request',
    )

O valor padrão da setting **"TEMPLATE\_CONTEXT\_PROCESSORS"** é o mesmo que definimos, exceto pelo último item. Então o que fizemos na verdade foi acrescentar este item:

        'django.core.context_processors.request',
    

É ele quem vai fazer a variável **"request"** ser visível em qualquer _view_.

**Template Context Processors** são funções que retornam dicionários de variáveis para o contexto padrão dos templates, o que significa que são variáveis que estarão presentes em todos os templates mesmo que não sejam declarados nas _views_.

Salve o arquivo. Feche o arquivo. Agora vá ao navegador, atualize com **F5** e veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-paginacao/file/"/>
</p>

E aí, fantástico, não?

## Melhorando um pouco mais e separando as contas para usuários

Depois desse ritmo alucinante, Alatazan estava cansado...

... em parte porque todas as vezes que ele imaginava um **"ahh, agora te peguei!"** para o Django, o Django respondia com um: **"hummm, vê se pegou direito o que mostrei pra você hoje!"**.

Mas em outra parte também porque sua aplicação caminhava firme para ser organizada por usuários e publicada em breve e ele não aguentava mais de ansiedade.

Mas o que Alatazan precisava rapidamente fazer era organizar as idéias sobre o aprendizado de hoje, e elas foram:

- Algumas funcionalidades da aplicação podem ser compartilhadas entre outras em comum, de forma a evitar a repetição de código;
- URLs podem passar parâmetros para _views_ para que elas sejam extensíveis;
- Todos os campos que possuem a propriedade **"choices"** podem ser usados pelo método **"get_NOMEDOCAMPO_display()"** para retornar seus rótulos;
- Formulários dinâmicos podem ser usados para fazer lançamentos em classes de modelo de infinitas formas diferentes;
- A template tag **"{% regroup %}"** organiza uma lista em grupos;
- Com duas linhas cria-se a paginação para uma lista, usando a classe **"Paginator"**;
- Com mais algumas linhas se resolve a paginação no template;
- **Template Context Processors** adicionam variáveis ao contexto das templates e isso pode ser muito útil;

Alatazan parou por aqui, querendo cama e um bom chá com pão e manteiga.

O próximo passo agora é a **edição, criação e exclusão** de Contas, Pessoas e Históricos. Por fim, também organizar isso tudo para separar para usuários diferentes.

