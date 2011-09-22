Ainda na fila do banco, não faltava muito para Alatazan ser atendido. Um velho senhor japonês do tipo que tem bom gosto e gosta de fazer tudo ao seu jeito, aguardava calmamente a sua vez.

Sua senha era 2 posições antes da de Alatazan e ele ansiava por fazer seu registro e resolver suas coisas pessoais. Do nada os dois começaram a conversar...

- Meu nome é Geraldo Boaventura. É... eu sei que pareço ser alguém com um nome como "Turo Hayakawa" ou qualquer coisa assim, mas a verdade é que sou só metade japonês. Minha mãe era uma italiana que casou-se com um português, moramos por muito tempo no interior de Minas Gerais mas cansei daquelas aventuras. Hoje sou só um velho querendo resolver suas coisas da maneira mais simples possível...

- Quarenta e três!

- Senhor, acho que é a sua vez.

- Ahh, obrigado, obrigado.

Ele se virou para a moça e começaram o processor para a abrir sua conta bancária.

É... não era tão simples quanto ele gostaria... eram três senhas, dois conjuntos de combinações de letras e mais os números da agência, conta corrente, cartão de crédito, código de segurança... meu Deus!

## Registro e outras funções para usuários

Se temos uma aplicação organizada e separada por usuários, o que falta são as funções para permitir que esses usuários se cadastrem no site, e iniciem sua vida por aqui, certo?

Então que tal começar pelo registro do usuário?

A primeira coisa é ajustar o menu principal para isso!

Abra o arquivo **"base.html"** da pasta **"templates"** do projeto para edição e localize esta linha de código:

         <li><a href="{% url views.contato %}">Contato</a></li>
    

Agora acrescente estas linhas logo abaixo dela:

         {% if user.is_authenticated %}
          <li><a href="{% url sair %}">{% trans "Sair" %}</a></li>
         {% else %}
          <li><a href="{% url entrar %}">{% trans "Entrar" %}</a></li>
          <li><a href="{% url registrar %}">{% trans "Registre-se" %}</a></li>
         {% endif %}

Como você pode ver, nós fazemos a seguinte verificação:

- Se há um usuário autenticado, ele tem uma nova opção: **Sair do site**, efetuando seu _logout_;
- Se o usuário não está autenticado, ele tem duas novas opções: **Entrar no site** (efetuar _logon_) ou **Registrar-se**.

Salve o arquivo. Feche o arquivo.

E agora vamos criar as URLs que indicamos no template. Para isso, abra o arquivo **"urls.py"** da pasta do projeto para edição, e acrescente as seguintes URLs:

        (r'^entrar/$', 'django.contrib.auth.views.login',
            {'template_name': 'entrar.html'}, 'entrar'),
        (r'^sair/$', 'django.contrib.auth.views.logout',
            {'template_name': 'sair.html'}, 'sair'),
        (r'^registrar/$', 'views.registrar', {}, 'registrar'),

Observe que as duas primeiras URLs ( **"entrar"** e **"sair"** ) referem-se a duas _views_ já existentes, que fazem parte da aplicação **"auth"**. Elas são duas das **generic views** que já fazem a maior parte do trabalho, dependendo somente por definir seus templates para que elas funcionem adequadamente.

Já a terceira URL ( **"registrar"** ) faz referência a uma nova _view_, que vamos criar agora mesmo!

Salve o arquivo. Feche o arquivo. Abra o arquivo **"views.py"** da pasta do projeto e acrescente as seguintes linhas ao final:

    def registrar(request):
        if request.method == 'POST':
            form = FormRegistro(request.POST)
    
            if form.is_valid():
                novo_usuario = form.save()
                return HttpResponseRedirect('/')
        else:
            form = FormRegistro()
        
        return render_to_response(
            'registrar.html',
            locals(),
            context_instance=RequestContext(request),
            )

A nova _view_ não apresenta grandes novidades. Apenas fazemos referência a um novo formulário dinâmico e o uso de um novo template.

Antes de partir para o novo formulário e o novo template, precisamos importar os dois elementos estranhos a este arquivo. Portanto, vá até o início do arquivo e acrescente estas duas novas linhas de código:

    from django.http import HttpResponseRedirect
    
    from forms import FormRegistro, FormContato

Salve o arquivo.

Agora, ainda na pasta do projeto, crie o novo arquivo **"forms.py"** com o seguinte código dentro:

    from django.contrib.auth.models import User
    from django import forms
    from django.utils.translation import ugettext as _
    
    class FormRegistro(forms.ModelForm):
        class Meta:
            model = User

Faça ainda uma coisa a mais: volte ao arquivo **"views.py"** que acabamos de modificar e localize este bloco de código:

    class FormContato(forms.Form):
        nome = forms.CharField(max_length=50, label=_('Nome'))
        email = forms.EmailField(required=False, label=_('E-mail'))
        mensagem = forms.Field(widget=forms.Textarea, label=_('Mensagem'))
    
        def enviar(self):
            titulo = 'Mensagem enviada pelo site'
            destino = 'alatazan@gmail.com'
            texto = """
            Nome: %(nome)s
            E-mail: %(email)s
            Mensagem:
            %(mensagem)s
            """ % self.cleaned_data
            
            send_mail(
                subject=titulo,
                message=texto,
                from_email=destino,
                recipient_list=[destino],
                )

Recorte e cole o bloco acima no arquivo **"forms.py"** que acabamos de criar. Faça o mesmo com a seguinte linha, que deve colada entre as primeiras linhas do novo arquivo:

    from django.core.mail import send_mail
    

Assim o nosso código fica um pouco mais organizado.

Salve ambos os arquivos, e feche-os. Agora precisamos criar o novo template, chamado **"registrar.html"** na pasta **"templates"** da pasta do projeto, com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% load i18n %}
    
    {% block titulo %}{% trans "Registro de usuario" %} - {{ block.super }}{% endblock %}
    {% block h1 %}{% trans "Registro de usuario" %}{% endblock %}
    
    {% block conteudo %}{{ block.super }}
    
    <form method="post">
     <table class="form">
      {{ form }}
    
      <tr>
       <th>&nbsp;</th>
       <td><input type="submit" value="{% trans "Registrar" %}"/></td>
      </tr>
     </table>
    </form>
    {% endblock conteudo %}

Salve o arquivo. Feche o arquivo. Observe que continuamos fazendo aquilo que já conhecemos: extendemos o template **"base.html"** e seus **blocos**, e acrescentamos um novo formulário ali dentro.

Agora execute seu projeto, clicando duas vezes sobre o arquivo **"executar.bat"**, e abra o navegador na URL principal:

    http://localhost:8000/
    

Certifique-se de que você não está autenticado pelo **Admin** e seu resultado deve ser este:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/usuarios-links-no-menu/file/"/>
</p>

E ao clicar no _link_ **"Registrar-se"**, o resultado deve ser este:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/usuarios-formulario-de-registro-com-tudo/file/"/>
</p>

Opa! Mas pera lá... a dose foi um pouco mais forte do que precisamos!

Então agora volte ao arquivo **"forms.py"** da pasta do projeto para edição e localize este bloco de código:

    class FormRegistro(forms.ModelForm):
        class Meta:
            model = User

Acrescente mais estas linhas de código logo abaixo do bloco localizado:

            fields = ('username','first_name','last_name','email','password')
    
        confirme_a_senha = forms.CharField(
            max_length=30, widget=forms.PasswordInput
            )
    
        def __init__(self, *args, **kwargs):
            self.base_fields['password'].help_text = 'Informe uma senha segura'
            self.base_fields['password'].widget = forms.PasswordInput()
            super(FormRegistro, self).__init__(*args, **kwargs)

Salve o arquivo. Volte ao navegador e atualize a página com **F5**. Veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/usuarios-formulario-de-registro-com-campos-ajustados/file/"/>
</p>

Uau! Melhorou muito! Mas ainda faltam algumas coisas para ter seu pleno funcionamento.

Para permitir o registro do novo usuário, algumas validações são necessárias:

- Verificar se já existe algum usuário com aquele **"username"**;
- Verificar se já existe algum usuário com aquele **"e-mail"**;
- Verificar se a senha informada foi **confirmada** corretamente;
- e criptografar a senha antes de salvar.

Não se preocupe, isso não é complicado. Para fazer isso, volte ao arquivo **"forms.py"** para edição, e acrescente este bloco de código ao final da classe **"FormRegistro"** (logo abaixo da linha que modificamos por último):

        def clean_username(self):
            if User.objects.filter(
                username=self.cleaned_data['username'],
                ).count():
                raise forms.ValidationError('Ja existe um usuario com este username')
    
            return self.cleaned_data['username']
    
        def clean_email(self):
            if User.objects.filter(
                email=self.cleaned_data['email'],
                ).count():
                raise forms.ValidationError('Ja existe um usuario com este e-mail')
    
            return self.cleaned_data['email']
    
        def clean_confirme_a_senha(self):
            if self.cleaned_data['confirme_a_senha'] != self.data['password']:
                raise forms.ValidationError('Confirmacao da senha nao confere!')
    
            return self.cleaned_data['confirme_a_senha']
    
        def save(self, commit=True):
            usuario = super(FormRegistro, self).save(commit=False)
    
            usuario.set_password(self.cleaned_data['password'])
    
            if commit:
                usuario.save()
    
            return usuario

Você pode notar que são **quatro métodos**, que fazem respectivamente o que citamos anteriormente:

O primeiro método valida se já existe um usuário com o **"username"** informado. Para isso, ele faz uso do método **"count()"** da queryset, que retorna o total de registros da classe **"User"** para o filtro onde **"username"** seja o que o usuário informou ( **"self.cleaned\_data['username']"** ):

        def clean_username(self):
            if User.objects.filter(
                username=self.cleaned_data['username'],
                ).count():
                raise forms.ValidationError('Ja existe um usuario com este username')
    
            return self.cleaned_data['username']

Vale ressaltar que para cada campo do formulário, é possível declarar um método **"clean\_NOMEDOCAMPO(self)"** para validar aquele campo isoladamente.

Ao levantar a exceção **"forms.ValidationError()"**, dizemos ao Django que aquele campo não foi informado corretamente e informamos a mensagem que explica o porquê disso.

Em seguida fazemos o mesmo, mas desta vez a nossa atenção é dada ao campo **"email"**:

        def clean_email(self):
            if User.objects.filter(
                email=self.cleaned_data['email'],
                ).count():
                raise forms.ValidationError('Ja existe um usuario com este e-mail')
    
            return self.cleaned_data['email']

O método seguinte já possui um artifício um pouco mais delicado: comparar os dois campos informados de senha ( **"password"** ) e confirmação da senha ( **"confirme\_a\_senha"** ):

        def clean_confirme_a_senha(self):
            if self.cleaned_data['confirme_a_senha'] != self.data['password']:
                raise forms.ValidationError('Confirmacao da senha nao confere!')
    
            return self.cleaned_data['confirme_a_senha']

Alí há uma pequena armadilha. Note que a nossa condição compara um item do dicionário **"self.cleaned\_data"** com um item do dicionário **"self.data"**. Isso é necessário pois o método de validação do campo não faz parte de uma sequência definida.

A sequência das validações é feita aleatoriamente, dependendo de como a memória posicionou os campos, e portanto, não é seguro verificar o valor de outro campo ( **"password"**, no caso ) no dicionário de valores **já validados** ( **"self.cleaned\_data"** neste caso ) simplesmente porque ele pode não ter sido validado ainda. Mas o dicionário de **valores informados** ( **"self.data"** ) é uma fonte segura dessa informação.

No caso do campo **"confirme\_a\_senha"**, podemos confiar pois se trata do campo que está sendo validado neste método, e portanto, é certo que uma validação básica foi feita antes disso e ele seguramente estará no dicionário **"self.cleaned\_data"**.

Por fim, sobrepusemos o método **"save()"** para repetir o seu comportamento ( com a função **"super()"** ) mas interferir em uma coisa: atribuir a senha usando o método que a criptografa para a sua atribuição. E logo em seguida, salvamos o registro (se assim estiver definido pelo argumento **"commit"**):

        def save(self, commit=True):
            usuario = super(FormRegistro, self).save(commit=False)
    
            usuario.set_password(self.cleaned_data['password'])
    
            if commit:
                usuario.save()
    
            return usuario

Salve o arquivo. Feche o arquivo.

Agora você pode registrar seu usuário numa boa.

### Funções de logon e logout

Você se lembra de que nós criamos duas URLs usando _generic views_ para **entrar** e **sair** do site, certo? Pois bem, precisamos agora criar os templates para elas.

Na pasta **"templates"** do projeto, crie o arquivo **"entrar.html"** com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% load i18n %}
    
    {% block titulo %}{% trans "Entrar" %} - {{ block.super }}{% endblock %}
    {% block h1 %}{% trans "Entrar" %}{% endblock %}
    
    {% block conteudo %}{{ block.super }}
    
    <form method="post">
     <table class="form">
      {{ form }}
    
      <tr>
       <th>&nbsp;</th>
       <td><input type="submit" value="{% trans "Entrar" %}"/></td>
      </tr>
     </table>
    </form>
    {% endblock conteudo %}

Notou que é um template muito parecido com outros que já fizemos, especialmente o que criamos para o registro do usuário? Sim, sim... eles são muito semelhantes!

A _generic view_ já se trata de colocar ali os campos corretamente, basta usar a variável **{{ form }}** dentro de uma tag HTML **&lt;FORM&gt;**.

Salve o arquivo. Feche o arquivo. Agora crie mais um arquivo na mesma pasta, chamado **"sair.html"**, com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% load i18n %}
    
    {% block titulo %}{% trans "Voce saiu do sistema" %} - {{ block.super }}{% endblock %}
    {% block h1 %}{% trans "Voce saiu do sistema" %}{% endblock %}
    
    {% block conteudo %}{{ block.super }}
    
    Voce saiu do sistema. Obrigado e volte sempre!
    {% endblock conteudo %}

Templatezinho básico, só pra avisar o usuário de que ele saiu do sistema com êxito e pode voltar assim que quiser. Agora no navegador, carregue a seguinte URL:

    http://localhost:8000/entrar/
    

Veja como ela é mostrada:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/usuarios-entrar/file/"/>
</p>

Legal! Informe ali um usuário e senha. Pode ser algum usuário que você tenha criado usando a página **"Registre-se"** ou pode ser o que temos trabalhado desde o começo do estudo:

* Usuário: **"admin"**
* Senha: **"1"**

Clique no botão **"Entrar"** e veja o efeito:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/usuarios-profile-apos-entrar/file/"/>
</p>

O que acontece é que, por padrão o Django entende que seu usuário terá uma página própria e que esta página própria por padrão estaria na URL **"/accounts/profile/"**. Como não é o nosso caso, vamos resolver isso assim:

Vá até a pasta do projeto e abra o arquivo **"settings.py"** para edição. Acrescente a seguinte linha de código ao final do arquivo:

    LOGIN_REDIRECT_URL = '/contas/'
    

Salve o arquivo. Feche o arquivo. Volte à URL para **entrar no site** novamente e veja que dessa vez a página de **Contas** é mostrada!

Feito isso, clique sobre o link **"Sair"** e veja que a página é carregada assim:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/usuarios-sair/file/"/>
</p>

### Fechando views para aceitarem somente o acesso de usuários autenticados

Agora vá até a pasta da aplicação **"contas"** e abra o arquivo **"views.py"** para edição. Localize esta linha de código:

    from django.core.paginator import Paginator
    

Acrescente esta outra linha logo abaixo dela:

    from django.contrib.auth.decorators import login_required
    

**"login\_required"** é o _decorator_ que obriga o usuário a se autenticar ao acessar uma _view_.

Agora localize esta outra linha:

    def contas(request):
    

Acrescente esta acima dela:

    @login_required
    

Localize esta:

    def conta(request, conta_id, classe):
    

Acrescente novamente aquela mesma linha de código aqui, logo acima dela:

    @login_required
    

Localize esta:

    def conta_pagamento(request, conta_id, classe):
    

Faça o mesmo:

    @login_required
    

Localize esta:

    def contas_por_classe(request, classe, titulo):
    

Novamente:

    @login_required
    

Localize esta:

    def editar_conta(request, classe_form, titulo, conta_id=None):
    

Outra vez:

    @login_required
    

Localize esta:

    def excluir_conta(request, classe, conta_id, proxima='/contas/'):
    

E agora mais outra vez:

    @login_required
    

Ufa! É isso aí.

Salve o arquivo. Feche o arquivo. Agora volte ao navegador e tente acessar esta URL:

    http://localhost:8000/contas/
    

E veja o que acontece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/usuarios-login_required/file/"/>
</p>

Veja que a nossa URL de **Contas** agora está inacessível, pois para ser isso, é necessário que o usuário se autentique. Só que o Django tentou abrir uma URL padrão para a autenticação do usuário, e esta URL não existe.

Para resolver isso, abra o arquivo **"settings.py"** da pasta do projeto e acrescente esta linha ao final:

    LOGIN_URL = '/entrar/'
    

Aproveite e acrescente logo esta também:

    LOGOUT_URL = '/sair/'
    

Salve o arquivo. Feche o arquivo. Agora tente novamente a URL no navegador:

    http://localhost:8000/contas/
    

E o que vemos já é bem mais satisfatório:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/usuarios-login_required-funcionando/file/"/>
</p>

Observe que ao informar seu usuário e senha para se autenticar, ao clicar sobre o botão **"Entrar"** a URL requisitada é carregada de forma direta!

### Organizando a segurança das views

Outra coisa importante agora é ajustar algumas coisinhas nas nossas _views_ para que elas não sejam vulneráveis. Imagine por exemplo um usuário excluindo a conta de outro usuário! Isso é inadmissível, não é?

Pois então vá até a pasta da aplicação **"contas"** e abra o arquivo **"views.py"** para edição. Localize este trecho de código:

    def conta(request, conta_id, classe):
        conta = get_object_or_404(classe, id=conta_id)

Acrescente logo abaixo dele:

        if conta.usuario != request.user:
            raise Http404

Este código faz com que, se o usuário autenticado não for o dono desta conta, uma página de **erro 404** é mostrada. Para que ela funcione corretamente localize esta linha no início do arquivo:

    from django.http import HttpResponseRedirect
    

E a modifique para ficar assim:

    from django.http import HttpResponseRedirect, Http404
    

Agora localize este outro trecho de código:

    def conta_pagamento(request, conta_id, classe):
        conta = get_object_or_404(classe, id=conta_id)

Acrescente abaixo dele:

        if conta.usuario != request.user:
            raise Http404

Localize mais este trecho:

    def editar_conta(request, classe_form, titulo, conta_id=None):
        if conta_id:
            conta = get_object_or_404(classe_form._meta.model, id=conta_id)

Acrescente abaixo dele (tenha cuidado para que o código inserido fique dentro do bloco do **"if"**):

            if conta.usuario != request.user:
                raise Http404

Localize este trecho também:

    def excluir_conta(request, classe, conta_id, proxima='/contas/'):
        conta = get_object_or_404(classe, id=conta_id)

E acrescente este logo abaixo:

        if conta.usuario != request.user:
            raise Http404

Assim, todas as _views_ que indicam uma conta específica estão protegidas. Mas só isso ainda não é suficiente. Precisamos também filtrar outras duas _views_ para mostrar somente as contas do usuário atual, não é?

Localize este trecho de código:

        contas_a_pagar = ContaPagar.objects.filter(
            status=CONTA_STATUS_APAGAR,
            )
        contas_a_receber = ContaReceber.objects.filter(
            status=CONTA_STATUS_APAGAR,
            )

Modifique-o para ficar assim:

        contas_a_pagar = ContaPagar.objects.filter(
            status=CONTA_STATUS_APAGAR, usuario=request.user,
            )
        contas_a_receber = ContaReceber.objects.filter(
            status=CONTA_STATUS_APAGAR, usuario=request.user,
            )

Observe que acrescentamos mais um elemento ao filtro: **"usuario=request.user,"**.

Localize este trecho também:

    def contas_por_classe(request, classe, titulo):
        contas = classe.objects.order_by('status','data_vencimento')

Modifique para ficar assim:

    def contas_por_classe(request, classe, titulo):
        contas = classe.objects.filter(
            usuario=request.user
            ).order_by('status','data_vencimento')

Novamente, trabalhamos no filtro pelo campo **"usuario"**.

Agora salve o arquivo. Feche o arquivo.

Precisamos fazer uma última mudança, coisa antiga... Abra o arquivo **"base.html"** da pasta **"templates"** do projeto para edição e localize esta linha de código:

         <li><a href="{% url views.contato %}">Contato</a></li>
    

E acrescente esta linha logo abaixo dela:

         <li><a href="{% url contas %}">Contas</a></li>
    

Salve o arquivo. Feche o arquivo. Veja no navegador como as nossas páginas estão:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/usuarios-com-aplicacao-contas-no-menu/file/"/>
</p>

### Criando uma view com acesso especial

Agora vamos saber um pouco sobre **permissões de acesso**. Elas serão úteis na nova _view_ que vamos criar agora, para listar todos os usuários do sistema. Isso será acessível somente para quem estiver autenticado e tiver acesso especial para isso.

Na pasta do projeto, abra o arquivo **"urls.py"** para edição e acrescente a seguinte URL:

        (r'^todos_os_usuarios/$', 'views.todos_os_usuarios',
            {}, 'todos_os_usuarios'),

Salve o arquivo. Feche o arquivo.

Ainda na mesma pasta, abra o arquivo **"views.py"** para edição e acrescente as seguintes linhas de código ao final:

    @permission_required('ver_todos_os_usuarios')
    def todos_os_usuarios(request):
        usuarios = User.objects.all()
        return render_to_response(
            'todos_os_usuarios.html',
            locals(),
            context_instance=RequestContext(request),
            )

Viu que trabalhamos uma coisa diferente? Esta:

    @permission_required('contas.ver_todos_os_usuarios')
    

Este _decorator_ determina que somente usuários que tenham a permissão **"ver\_todos\_os\_usuarios"** da aplicação **"contas"** podem acessar a _view_ em questão.

Vá agora ao início do arquivo e acrescente estas linhas de código:

    from django.contrib.auth.models import User
    from django.contrib.auth.decorators import permission_required

Salve o arquivo. Feche o arquivo.

Agora vamos criar o template da _view_ que acabamos de criar. Na pasta **"templates"** do projeto, crie um novo arquivo chamado **"todos\_os\_usuarios.html"** com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% load i18n %}
    
    {% block titulo %}{% trans "Todos os usuarios do sistema" %} - {{ block.super }}{% endblock %}
    {% block h1 %}{% trans "Todos os usuarios do sistema" %}{% endblock %}
    
    {% block conteudo %}{{ block.super }}
    
    <ul>
     {% for usuario in usuarios %}
     <li>{{ usuario }}</li>
     {% endfor %}
    </ul>
    {% endblock conteudo %}

Salve o arquivo. Feche o arquivo. Agora na pasta da aplicação **"contas"**, abra o arquivo **"models.py"** para edição e localize o seguinte trecho de código:

    class Conta(models.Model):
        class Meta:
            ordering = ('-data_vencimento', 'valor')

Acrescente este bloco de código logo abaixo:

            permissions = (
                ('ver_todos_os_usuarios', 'Ver todos os usuarios'),
                )

Isso vai criar uma nova permissão: **"Ver todos os usuarios"**, sob o codinome **"ver\_todos\_os\_usuarios"**.

Salve o arquivo. Feche o arquivo.

Agora falta uma última coisa: atualizar a geração do banco de dados para ter a nova permissão. Para isso, clique duas vezes sobre o arquivo **"gerar\_banco\_de\_dados.bat"** da pasta do projeto e o resultado deve ser este:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/usuarios-geracao-da-nova-permissao/file/"/>
</p>

Não há mensagem esclarecendo o que houve, mas de fato, a permissão é gerada nesse momento. Agora vá ao navegador e carregue a URL para edição de um usuário no **Admin**, como esta por exemplo:

    http://localhost:8000/admin/auth/user/2/
    

Veja como a página é mostrada:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/usuarios-admin-do-usuario-com-nova-permissao/file/"/>
</p>

Note que o último item é a permissão que criamos: **"contas | conta | Ver todos os usuarios"**. Clique sobre ele para selecioná-lo e clique sobre o **ícone azul com uma setinha** à direita para movê-lo para a caixa da direita. Feito isso, save o usuário, e ele terá essa permissão especial!

Agora você pode se autenticar com o usuário que tem tal permissão e carregar a URL protegida, pois ele terá acesso. Do contrário, a página **"Entrar"** será carregada solicitando a autenticação de um usuário que tenha a permissão necessária.

Usuários marcados como **"Superusuário"** não são barrados por permissões, pois possuem todas elas atribuídas por definição, mesmo que não o estejam no cadastro do usuário.

### Alteração de senha

Agora, que tal usar um pouco mais das _generic views_ da aplicação de **autenticação**? Então vamos escolher a alteração de senha para brincar um pouco.

Na pasta do projeto, abra para edição o arquivo **"urls.py"** e acrescente as seguintes URL:

        (r'^mudar_senha/$', 'django.contrib.auth.views.password_change',
            {'template_name': 'mudar_senha.html'}, 'mudar_senha'),
        (r'^mudar_senha/concluido/$', 'django.contrib.auth.views.password_change_done',
            {'template_name': 'mudar_senha_concluido.html'}, 'mudar_senha_concluido'),

Na primeira URL, fazemos referência à _generic view_ **"django.contrib.auth.views.password\_change"**, demos o nome à URL de **"mudar\_senha"** e usamos o template **"mudar\_senha.html"**.

A segunda URL é necessária e trata-se daquela que será chamada após a alteração da senha. Ela faz uso da _generic view_ **"django.contrib.auth.views.password\_change\_done"**

Salve o arquivo. Feche o arquivo. Agora na pasta **"templates"** do projeto, crie um novo arquivo chamado **"mudar\_senha.html"** com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% load i18n %}
    
    {% block titulo %}{% trans "Mudar senha" %} - {{ block.super }}{% endblock %}
    {% block h1 %}{% trans "Mudar senha" %}{% endblock %}
    
    {% block conteudo %}{{ block.super }}
    
    <form method="post">
     <table class="form">
      {{ form }}
    
      <tr>
       <th>&nbsp;</th>
       <td><input type="submit" value="{% trans "Mudar senha" %}"/></td>
      </tr>
     </table>
    </form>
    {% endblock conteudo %}

Nada demais, né?

Salve o arquivo. Feche o arquivo.

Agora crie outro arquivo, chamado **"mudar\_senha\_concluido.html"** na mesma pasta, com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% load i18n %}
    
    {% block titulo %}{% trans "Senha alterada" %} - {{ block.super }}{% endblock %}
    {% block h1 %}{% trans "Senha alterada" %}{% endblock %}
    
    {% block conteudo %}{{ block.super }}
    
    <b>A sua senha foi alterada com sucesso!</b>
    
    {% endblock conteudo %}

Nada demais também.

Salve o arquivo. Feche o arquivo. Agora só precisamos de acrescenter essa opção ao menu e para isso só é preciso abrir o arquivo **"base.html"** da mesma pasta de templates para edição e localizar esta linha de código:

          <li><a href="{% url sair %}">{% trans "Sair" %}</a></li>
    

Acrescente esta outra linha de código acima dela:

          <li><a href="{% url mudar_senha %}">{% trans "Mudar senha" %}</a></li>
    

Salve o arquivo. Feche o arquivo. Agora volte ao navegador, na seguinte URL:

    http://localhost:8000/mudar_senha/
    

E veja a página que é carregada:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/usuarios-mudar-senha/file/"/>
</p>

E aí, melzinho na chupeta, não é mesmo?

## E agora, que tal um pouco mais de testes?

- Visceral! Animal!

Foi a reação de Cartola, que ainda não conhecia bem as _generic views_ da aplicação **"auth"**.

- Cara, isso é muito maneiro!

A cara de Alatazan passou do amarelo habitual para amarelinho, amarelinhozinho, quase-branco-amarelado, branco-de-fato e tornou ao mesmo amarelo habitual fazendo o caminho de volta passando pelo quase-branco-amarelado, amarelinhozinho e amarelinho.

É que a associação entre **"víceras"** e algo realmente **"animal"** era bastante chato do ponto de vista de um estômago um pouco sensível. Mas logo ele se tocou que _"visceral"_ e _"animal"_ são expressões mais _cool_ e com um significado bastante simpático, por sinal.

- Olha só, com exceção da _view_ de **registro de usuário**, o que fizemos aqui foi usar bastante as _generic views_, então vamos dar uma passada rápida no que fizemos, ok?

 - O registro do usuário foi feito usando **ModelForms**, e pra isso nós fizemos uso de métodos de validação do tipo **"clean\_NOME\_DO\_CAMPO"** e também uma idéia bem sacada foi sobrepôr o método **"save()"** e forçar a criptografia da senha com **"set\_password()"**. Muito legal isso!
 - Inclusive a validação é feita usando uma exceção, que ao invés de derrubar todo o site, apenas exibe a mensagem da validação. Ela se chama **"forms.ValidationError()"**;
 - Movemos o formulário de contato para o novo arquivo **"forms.py"** pra dar uma organizada por ali;
 - No mais, foram _generic views_.
 - As duas primeiras que usamos foram as de **"login"** e **"logout"**, definimos templates para elas e babau!
 - Depois definimos algumas _settings_ pra trabalhar bem com essas _views_, como por exemplo a **"LOGIN\_REDIRECT\_URL"** e a **"LOGIN\_URL"**;
 - Fizemos uso também dos decorators **"@login\_required"** e **"@permission\_required"**. O primeiro protege a _view_ para ser acessada somente por quem está autenticado. Já o segundo garante que a _view_ só será acessada por quem tenha uma permissão especial;
 - E falando em permissões, declaramos o atributo **"permissions"** da classe **"Meta"** de uma classe de modelo para criar essa permissão especial, atualizamos o banco de dados e liberamos a permissão para um usuário usando o **Admin**;
 - Também tomamos o cuidado de evitar que um usuário acesse as contas de outros e que ele veja somente as dele, sempre!
 - Por fim, partimos para mais duas _generic views_, usadas para a **alteração de senha**.

- Puxa, Alatazan, a cada dia que passa eu fico mais entusiasmada com o Django... olha só essas outras _generic views_ que tem na contrib **"auth"**:

 - django.contrib.auth.views.logout\_then\_login
 - django.contrib.auth.views.password\_reset
 - django.contrib.auth.views.redirect\_to\_login

- Só de ler os nomes, já dá pra ter uma idéia do que elas fazem, né...

- É, e olha: na verdade o Django tem muitas coisas que não vemos aqui, mas um passeio pela documentação é sempre muito excitante... outro dia, vi umas coisas sobre programação orientadas a testes por lá... é muito, muito maneiro!

No próximo capítulo, vamos conhecer um pouco mais disso: **TDD**, a metodologia de programação dirida a testes aplicada no Django!

