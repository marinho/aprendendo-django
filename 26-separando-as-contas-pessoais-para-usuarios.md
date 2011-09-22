- Dezessete! - a moça gritou lá na frente.

Alatazan olhou no seu papel..

_"Quarenta e cinco. Hummm..."_

Pensou ele...

O relógio dizia que estava ali havia uma hora e trinta e sete minutos...

Um papel na parede dizia:

_"Defenda o seu! A Lei protege você! 20 minutos é o tempo máximo para espera."_

Ele pensou:

- Porquê diabos eu sou obrigado a ver aquele...

- Dezoito! - gritou ela novamente.

- ... papel ali, se todo...

- Sinto muito senhora, mas a sua senha é na seção da esquerda.

- ... mês é a mesma coisa?

- Não, não atendemos senhoras debilitadas aqui. E na próxima fila o tempo começa a contar novamente, em breve você será atendida.

- Também não há banheiro público aqui.

## Cada um na sua e olhe lá

Nem tudo no sistema é coletivo. Não é assim, um oba-oba... uns são seus, outros são meus... e outros, aqueles outros, aqueles são os nossos, sabe? Contas são coisas que todo mundo quer doar mas ninguém quer assumir as de outros... é assim e pronto!

Então é por isso mesmo - exatamente isso mesmo - que nós precisamos mudar essas classes de modelo das Contas Pessoais para serem estritamente separadas por usuários.

### Campo para o usuário

Então, vamos começar pela classe **"Historico"**! Abra o arquivo **"models.py"** da aplicação **"contas"** para edição, e localize este bloco de código:

    class Historico(models.Model):
        class Meta:
            ordering = ('descricao',)
        
        descricao = models.CharField(max_length=50)

Logo abaixo dele, acrescente a seguinte linha:

        usuario = models.ForeignKey(User)
    

Agora localize este outro bloco:

    class Pessoa(models.Model):
        class Meta:
            ordering = ('nome',)
    
        nome = models.CharField(max_length=50)
        telefone = models.CharField(max_length=25, blank=True)

E acrescente esta abaixo dele:

        usuario = models.ForeignKey(User)
    

E por fim, faça o mesmo com a classe **"Conta"**:

    class Conta(models.Model):
        class Meta:
            ordering = ('-data_vencimento', 'valor')
    
        pessoa = models.ForeignKey('Pessoa')

Logo acima do campo **"pessoa"**, acrescente a linha abaixo:

        usuario = models.ForeignKey(User)
    

Agora encontre esta linha entre as primeiras do arquivo:

    from django.core.urlresolvers import reverse
    

E acrescente esta abaixo dela:

    from django.contrib.auth.models import User
    

Salve o arquivo. Feche o arquivo. E agora, vamos ter que criar todos esses campos manualmente?

Teríamos. Mas neste caso, o campo é importante demais para passar desapercebido. Para este caso, vamos usar um outro recurso do Django que elimina as **tabelas da aplicação** e as cria novamente com os campos novos. Mas cuidado com isso, pois seus dados nesta aplicação serão perdidos, ok?

### Usando o manage.py reset

Na pasta do projeto, crie um arquivo chamado **"reset_contas.bat"** com o seguinte código dentro:

    python manage.py reset contas
    pause

Salve o arquivo. Feche o arquivo. Agora clique duas vezes sobre o arquivo para executá-lo, e veja a mensagem que é exposta:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-manage-reset/file/"/>
</p>

Em outras palavras: todos os dados da aplicação **"contas"** serão perdidos. Para confirmar, digite **"yes"** e pressione a tecla **"ENTER"**.

Os dados da aplicação **"contas"** serão perdidos mas as demais serão mantidas intactas. Por isso, vá ao **Admin** e acrescente novas **Pessoas** e **Históricos** para continuar com nosso aprendizado.

Mas antes você deve executar o projeto clicando duas vezes sobre o arquivo **"executar.bat"** da pasta do projeto.

Agora nosso próximo passo será...

### Criar formulários para criar e editar Contas

É isso aí!

Agora, vamos editar o arquivo **"contas.html"** da pasta **"contas/templates/contas"** e localizar esta linha:

    <a href="{% url contas_a_pagar %}">{% trans "Ver todas" %}</a>
    

Abaixo dela, acrescente esta:

    <a href="{% url nova_conta_a_pagar %}">{% trans "Criar nova" %}</a>
    

Localize também esta:

    <a href="{% url contas_a_receber %}">{% trans "Ver todas" %}</a>
    

E acrescente esta abaixo dela:

    <a href="{% url nova_conta_a_receber %}">{% trans "Criar nova" %}</a>
    

Com isso, agora temos _links_ para criar novas contas para cada tipo: **"a Receber"** e **"a Pagar"**.

Salve o arquivo. Feche o arquivo. Pois agora vamos criar as URLs que acamos de referenciar nos _links_.

Na pasta da aplicação **"contas"**, abra o arquivo **"urls.py"** e acrescente as seguintes URLs:

        url('^pagar/nova/$', 'editar_conta',
            {'classe_form': FormContaPagar, 'titulo': 'Conta a Pagar'},
            name='nova_conta_a_pagar'),
        url('^receber/nova/$', 'editar_conta',
            {'classe_form': FormContaReceber, 'titulo': 'Conta a Receber'},
            name='nova_conta_a_receber'),

Veja que continuamos apostando na estratégia de duas URLs para contas distintas, porém usando a mesma _view_. Mas desta vez usamos um parâmetro chamado **"classe\_form"** ao invés de **"classe"**. Ele vai nos indicar a classe de formulário dinâmico que esta URL vai usar para validar e salvar os dados no banco de dados.

Mas ainda não acabamos. Localize esta linha:

    from models import ContaPagar, ContaReceber
    

E acrescente esta logo abaixo dela:

    from forms import FormContaPagar, FormContaReceber
    

Salve o arquivo. Feche o arquivo.

### Usando ModelForms

Agora, para dar vida às duas novas classes que importamos, abra o arquivo **"forms.py"** da mesma pasta e adicione estas linhas ao seu final:

    class FormContaPagar(forms.ModelForm):
        class Meta:
            model = ContaPagar
    
    class FormContaReceber(forms.ModelForm):
        class Meta:
            model = ContaReceber

Esses são os nossos dois novos formulários. Ao vincular as classes de modelo usando o atributo **"model"**, nós fizemos com que essas classes tenham todo comportamento necessário para criar e editar objetos de suas respectivas classes.

Agora encontre esta outra linha no mesmo arquivo:

    from django import forms
    

E acrescente esta logo abaixo dela:

    from models import ContaPagar, ContaReceber
    

Salve o arquivo. Feche o arquivo.

Ainda na mesma pasta, abra o arquivo **"views.py"** para edição e acrescente estas novas linhas de código ao seu final:

    def editar_conta(request, classe_form, titulo, conta_id=None):
        form = classe_form()
    
        return render_to_response(
            'contas/editar_conta.html',
            locals(),
            context_instance=RequestContext(request),
            )

Salve o arquivo. Feche o arquivo.

Esta é a view que deve atender às novas URLs que criamos. E para que ela funcione apropriadamente, vamos criar o template a que ela referencia. Para isso, crie o arquivo  **"editar\_conta.html"** na pasta **"contas/templates/contas"**, partindo da pasta do projeto, com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% block titulo %}{{ titulo }} - {{ block.super }}{% endblock %}
    {% block h1 %}{{ titulo }}{% endblock %}
    
    {% block conteudo %}
    <form method="post">
      <table class="form">
    
        <tr>
           <th>&nbsp;</th>
           <td><input type="submit" value="Salvar"/></td>
        </tr>
      </table>
    </form>
    {% endblock conteudo %}

Salve o arquivo e vá ao navegador nesta URL:

    http://localhost:8000/contas/pagar/nova/
    

Veja como ela ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-edicao-de-conta-sem-form/file/"/>
</p>

Humm... nada demais, certo? Mas voltando ao template que estamos criando, localize esta linha:

      <table class="form">
    

E acrescente esta logo abaixo dela:

        {{ form }}
    

Salve o arquivo. Feche o arquivo e veja a diferença no navegador:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-edicao-de-conta-com-form/file/"/>
</p>

Como você pode ver, a linha que acrescentamos fez **toda** a diferença. Isso é porque o **ModelForm** fez todo o trabalho de definir os campos, seus tipos e comportamentos, inclusive aqueles que são apenas uma caixa de seleção.

Acontece que ainda assim está longe do ideal, pois alguns campos ali deveriam ser exibidos de forma diferente.

Vamos portanto ajustar um pouco o formulário dinâmico para ficar ainda melhor. Para isso, abra o arquivo **"forms.py"** da pasta da aplicação **"contas"** para edição e localize esta linha de código:

            model = ContaPagar
    

Agora acrescente esta linha abaixo dela:

        exclude = ('usuario','operacao','data_pagamento')
    

Isso vai sumir com os campos **"usuario"** **"operacao"** e **"data\_pagamento"** do formulário, e a partir de agora eles serão ignorados, assumindo seus valores _default_. Precisamos de fazer isso porque o usuário deve ser sempre o **usuário atual**, a **operação** de uma conta é definida quando ela é salva e já a **data de pagamento** deve ser atribuída quando o pagamento é feito.

Acrescente mais estas linhas abaixo dela:

        def __init__(self, *args, **kwargs):
            self.base_fields['data_vencimento'].widget = SelectDateWidget()
            
            super(FormContaPagar, self).__init__(*args, **kwargs)

A declaração deste método inicializador faz com que o campo **"data\_vencimento"** passe a ter um _widget_ de edição mais amigável.

Agora faça o mesmo com a classe **"FormContaReceber"**, acrescentando as seguintes linhas ao seu final:

            exclude = ('usuario','operacao','data_pagamento')
    
        def __init__(self, *args, **kwargs):
            self.base_fields['data_vencimento'].widget = SelectDateWidget()
        
            super(FormContaReceber, self).__init__(*args, **kwargs)

Agora vá ao início do arquivo e acrescente esta linha de código:

    from django.forms.extras.widgets import SelectDateWidget
    

Salve o arquivo. Feche o arquivo.

Agora abra o arquivo **"views.py"** da mesma pasta, para edição, e localize este bloco de código:

    def editar_conta(request, classe_form, titulo, conta_id=None):
        form = classe_form()
    
        return render_to_response(
            'contas/editar_conta.html',
            locals(),
            context_instance=RequestContext(request),
            )

Modifique-o para ficar assim:

    def editar_conta(request, classe_form, titulo, conta_id=None):
        if request.method == 'POST':
            form = classe_form(request.POST)
    
            if form.is_valid():
                conta = form.save(commit=False)
                conta.usuario = request.user
                conta.save()
    
                return HttpResponseRedirect(conta.get_absolute_url())
        else:
            form = classe_form(initial={'data_vencimento': datetime.date.today()})
    
        return render_to_response(
            'contas/editar_conta.html',
            locals(),
            context_instance=RequestContext(request),
            )

Veja que as novidades ficam por conta deste trecho:

            if form.is_valid():
                conta = form.save(commit=False)
                conta.usuario = request.user
                conta.save()

Primeiro de tudo, o formulário é **validado** através do método **"is\_valid()"**. Se ele retornar **"True"** os dados são salvos mas o argumento **"commit=False"** determina que os dados não sejam persistidos no banco de dados. Isso porque logo em seguida o campo **"usuario"** deve receber o usuário autenticado do sistema ( **"request.user"** ) e por fim ele é salvo e persistido **de fato** no banco de dados com o método **"save()"** da **conta**.

A outra novidade está nesta linha de código:

            form = classe_form(initial={'data_vencimento': datetime.date.today()})
    

O argumento **"initial"** recebe os valores de inicialização do formulário, ou seja, seus valores **padrão**. Portanto o que a linha acima fez foi determinar que o valor de inicialização do campo **"data\_vencimento"** deve ser a data atual.

Agora acrescente esta linha ao início do arquivo:

    import datetime
    

Salve o arquivo. Feche o arquivo. Volte ao navegador e acrescente uma nova conta a pagar, assim por exemplo:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-criando-nova-conta-a-pagar/file/"/>
</p>

Ao final, clique sobre o botão **"Salvar"** para ver a conta ser salva no banco de dados e ser redirecionado à URL da conta.

### Ajustando o formulário para edição de objetos

Nosso próximo passo agora é editar a conta. Para começar, precisamos de um link na página da conta para a página de edição da mesma. Então vamos editar o arquivo **"conta.html"** da pasta **"contas\templates\contas"** partindo da pasta do projeto e localizar esta linha:

    {% block conteudo %}
    

Abaixo dela, acrescente esta:

    <a href="editar/">{% trans "Editar" %}</a>
    

Salve o arquivo. Feche o arquivo. Volte ao navegador, atualize a página da conta e veja que temos um novo link:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-conta-salva-com-link-para-editar/file/"/>
</p>

Agora na pasta da aplicação **"contas"**, abra o arquivo **"urls.py"** para edição e acrescente estas duas novas URLs:

        url('^pagar/(?P<conta_id>\d+)/editar/$', 'editar_conta',
            {'classe_form': FormContaPagar, 'titulo': 'Conta a Pagar'},
            name='editar_conta_a_pagar'),
        url('^receber/(?P<conta_id>\d+)/editar/$', 'editar_conta',
            {'classe_form': FormContaReceber, 'titulo': 'Conta a Receber'},
            name='editar_conta_a_receber'),

Veja que fazemos referência a uma _view_ que já criamos: **"editar\_conta"**, mas desta vez nós vamos carregá-la para uma conta que já existe, indicada pelo argumento **"conta\_id"**, que é citado na expressão regular da URL.

Salve o arquivo. Feche o arquivo. Agora abra o arquivo **"views.py"** da mesma pasta e localize esta linha de código:

    def editar_conta(request, classe_form, titulo, conta_id=None):
    

Veja que na declaração da _view_ o argumento **"conta\_id"** é passado. Quando trata-se de uma **nova conta**, ele não recebe valor nenhum e assume seu valor padrão: **None**. Mas agora ele será atribuído com um valor: o ID da conta a editar.

Para fazer uso disso, precisamos acrescentar as seguintes linhas de código abaixo da linha que localizamos:

        if conta_id:
            conta = get_object_or_404(classe_form._meta.model, id=conta_id)
        else:
            conta = None

A variável **"classe\_form"** traz a classe de formulário dinâmico indicado pela URL. Seu atributo **"\_meta"** traz diversas informações de sua definição, e uma delas é o atributo **"model"**, que contém a classe de modelo à qual o formulário dinâmico está declarado. Em outras palavras, para a classe **"FormContaPagar"**, o elemento **"classe\_form.\_meta.model"** contém **"ContaPagar"**, e para a classe **"FormContaReceber"** o mesmo elemento contém **"ContaReceber"**.

O que fizemos nesse trecho de código foi **carregar a conta** se o argumento **"conta\_id"** tiver algum valor válido.

Agora localize esta linha:

            form = classe_form(request.POST)
    

E a modifique para ficar assim:

            form = classe_form(request.POST, instance=conta)
    

Localize esta linha também:

            form = classe_form(initial={'data_vencimento': datetime.date.today()})
    

E a modifique para ficar assim:

            form = classe_form(
                initial={'data_vencimento': datetime.date.today()},
                instance=conta,
                )

Ambas as linhas que modificamos definem a mesma coisa: a variável **"conta"** é atribuída ao formulário dinâmico para edição. O formulário dinâmico se encarregará de verificar se a variável possui um valor válido e editar se for o caso.

Salve o arquivo. Feche o arquivo. Volte ao navegador, clique sobre o link **"Editar"** e veja a página que será carregada:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-editando-conta/file/"/>
</p>

Viu como é simples?

### Exclusão de objetos

Agora vamos criar um meio para excluir uma conta. Da mesma forma como fizemos com a edição, precisamos novamente de um link para a exclusão.

Para isso, abra para edição o arquivo **"conta.html"** da pasta **"contas\templates\contas"** e localize esta linha:

    <a href="editar/">{% trans "Editar" %}</a>
    

Acrescente esta abaixo dela:

    <a href="excluir/">{% trans "Excluir" %}</a>
    

Salve o arquivo. Feche o arquivo. Agora vamos declarar as URLs para tal.

Abra o arquivo **"urls.py"** da pasta da aplicação **"contas"** e acrescente estas novas URLs:

        url('^pagar/(?P<conta_id>\d+)/excluir/$', 'excluir_conta',
            {'classe': ContaPagar, 'proxima': '/contas/pagar/'},
            name='excluir_conta_a_pagar'),
        url('^receber/(?P<conta_id>\d+)/excluir/$', 'excluir_conta',
            {'classe': ContaReceber, 'proxima': '/contas/receber/'},
            name='excluir_conta_a_receber'),

Como pode ver, novamente fizemos algo muito semelhante às URLs que declaramos hoje, mas agora apontando para a _view_ **"excluir\_conta"**.

A novidade fica no argumento **"proxima"**, que aponta para uma URL. Este argumento será usado para redirecionar o navegador após a exclusão da conta, já que a exclusão não possui template ou página de destino.

Salve o arquivo. Feche o arquivo. Abra o arquivo **"views.py"** da mesma pasta para edição e acrescente este bloco de código ao final:

    def excluir_conta(request, classe, conta_id, proxima='/contas/'):
        conta = get_object_or_404(classe, id=conta_id)
        conta.delete()
    
        request.user.message_set.create(message='Conta excluida com sucesso!')
        
        return HttpResponseRedirect(proxima)

Humm... veja só:

As duas primeiras linhas são bastante conhecidas: a _view_ recebe os argumentos que definimos na URL e logo a seguir a conta é carregada usando a função **"get\_object\_or\_404()"**.

    def excluir_conta(request, classe, conta_id, proxima='/contas/'):
        conta = get_object_or_404(classe, id=conta_id)

A primeira novidade está aqui:

        conta.delete()
    

Esta linha acima exclui a **conta** carregada do banco de dados, definitivamente.

E uma novidade mais diferente ainda é esta:

        request.user.message_set.create(message='Conta excluida com sucesso!')
    

Este recurso só funciona quando há algum usuário autenticado. Ele tem a função de armazenar uma fila de mensagens para exibir para aquele usuário. Quando o usuário visualiza a mensagem, automaticamente ela é excluída do banco de dados.

A coisa bacana disso é que uma mensagem pode ficar armazenada até mesmo quando o usuário não está usando o site, por dias, semanas ou meses. Ela só será removida da fila quando o usuário visualizá-la!

Por fim, o navegador é redirecionado para o caminho indicado pela variável **"proxima"**:

        return HttpResponseRedirect(proxima)
    

Salve o arquivo. Feche o arquivo.

Agora para que o usuário possa visualizar a mensagem, abra o arquivo **"base.html"** da pasta **"templates"** do projeto e localize esta linha:

       {% include "idiomas.html" %}
    

Acrescente estas linhas acima dela:

       {% for msg in messages %}
       <div class="mensagem">{{ msg }}</div>
       {% endfor %}

Salve o arquivo. Feche o arquivo. Agora vamos dar um visual para essas mensagens. Abra o arquivo **"layout.css"** da pasta **"media"** do projeto e acrescente esta linha de código ao final:

    .mensagem {
      margin: 5px;
      padding: 5px;
      background: yellow;
      border: 3px solid red;
      font-weight: bold;
      color: red;
    }

Salve o arquivo. Feche o arquivo. Agora volte ao navegador, na seguinte URL:

    http://localhost:8000/contas/pagar/
    

Escolha uma conta a pagar já existente clicando sobre ela. Caso não exista uma, vá até a URL de Contas a Pagar do **Admin**, crie uma nova conta e após salvá-la, volte à URL de contas a pagar do site para ver a nova Conta a Pagar e clicar sobre ela.

Encontre o link **"Excluir"** e clique sobre ele, veja o que acontece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-exclusao/file/"/>
</p>

O que achou? Isso não é bacana?

Agora que tal fazer o mesmo para as classes **"Pessoa"** e **"Historico"**? Legal! Mas isso agora é com você!

## Registro de usuários, mudança de senha, essas coisas...

- Aí garoto! Como vai essa força aí?

A voz do outro lado da linha era inconfundível. Cartola vestia um sungão verde com borboletas psicodélicas azúis, pra combinar com a praia de Jericoacoara - essa dedução ele fez quando entendeu não havia nada mais psicodélico do que Jericoacoara... mas disso Alatazan não sabia, e o telefone ainda é só por voz...

- Fala folgado, já estou terminando a aplicação de contas, agora já está tudo separadinho por usuários e amanhã eu vou fazer aquela coisarada toda de registro de usuário e tal...

- Bacana, camarada, e... _summing up_, o que você fez?

- Foi basicamente o seguinte:

 - A primeira coisa foi criar um campo **"usuario"** para as classes que eu queria separar por usuário;
 - Pra efetivar isso no banco de dados, usei a ferramenta **"manage.py reset"**, que exclui todas as tabelas e as cria novamente... **perdi todas** as minhas pessoas, históricos e contas, mas eu estava **consciente** disso;
 - Depois usei **ModelForms** pra fazer o cadastro de contas, ocultei alguns campos usando o atributo **"exclude"**, mudei o _widget_ do campo de data...
 - Para cada uma das URLs de criação, edição e exclusão de contas, usei sempre parâmetros para definir qual era a classe em questão, assim evitei repetir código;
 - Na _view_ de salvar contas, usei o método **"save()"** do formulário dinâmico, com o argumento **"commit=False"** pra dizer que não queria salvar no banco de dados ainda, pra dar tempo de informar qual era o usuário e só então efetivar a **persistência**;
 - Usei o argumento **"initial"** pra indicar um valor inicial ao formulário dinâmico;
 - E por fim, criei a função de excluir contas, usando o método **"delete()"**. Nessa hora o mais legal foi usar um recurso para jogar frases em uma **fila de mensagens** que é exibida ao usuário...

- Show de bola, meu irmão!

- Cartola, preciso dar uma volta por aí pra _jogar um grilo no esôfago_

Aquela expressão não era comum para Cartola, mas considerando que nem tudo na vida de Alatazan era comum a ele, ele deu de ombros, se despediu e voltou pra curtir a praia...

Agora, nosso próximo passo será o registro do usuário, mudança de senha e algumas definições de segurança para o que construímos hoje, afinal, ninguém quer ter suas contas sendo mexidas por qualquer um por aí né...

