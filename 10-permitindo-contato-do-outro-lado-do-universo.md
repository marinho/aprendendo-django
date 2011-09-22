<img src="http://www.aprendendodjango.com/media/img/ilustracao_5.gif" align="right"/>

- Shhhéeeeght! Pzzt! SShhháptt! Tzóing! Pzzziiitt! Pam-pararampam. Pam! Pam

Lá estava Alatazan em sua nave, uns 500 km acima do chão, já quase no limite da atmosfera, tentando regular o canal para fazer contato com seu planeta.

O sistema de comunicação de sua nave utiliza **dobradura de nível 6** no espaço, com envergadura de **70º a 90º** e isso pode ser seriamente afetado quando há uma tempestade de asteróides nas redondezas. Não faz muito tempo que Katara descobriu o **rádio** como uma simples solução para comunicações a grandes distâncias, e foi também nesta época que eles souberam da existência do Planeta Terra e de nossa Internet.

Nessa hora ficou ainda mais evidente que Alatazan não podia viver isolado em seu espaço, **é preciso ter um meio para receber contato** de outras pessoas, **não importa quão distante elas estejam**.

Então, naquele dia eles criaram:

## Uma página para Contato

A página de Contato pode variar de um site para outro, mas não tem erro: no mínimo ela precisa ter **quem** está fazendo contato e qual é a sua **mensagem**. Essas informações são transformadas em um texto e **enviadas por e-mail** do servidor para a sua caixa de mensagens, sem precisar expôr seu e-mail para estranhos.

Aliás, esse era o medo que Alatazan tinha quando escreveu sua nada amigável frase "Por favor não faça contato. Obrigado pela atenção.". Ele não queria expôr seu e-mail para qualquer um.

Antes de mais nada, lembre-se de executar seu projeto, clicando duas vezes sobre o arquivo **"executar.bat"** da pasta do projeto.

Agora, na pasta **"templates"** do projeto, abra o arquivo **"rodape.html"** e o modifique, para ficar assim:

    <div class="rodape">
    <a href="{% url views.contato %}">Para fazer contato comigo, clique aqui.</a>
    </div>

Salve o arquivo. Feche o arquivo. Agora vamos ao navegador para ver como fica a página principal do site:

    http://localhost:8000/
    

Veja:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contato-url-nao-existe-para-reverse/file/"/>
</p>

Puxa vida, mas que tamanho de erro é este? Mas a parte mais importante dessa mensagem de erro é isto:

    NoReverseMatch: Reverse for 'meu_blog.views.contato' with arguments '()' and keyword arguments '{}' not found.
    

Bastante evidente, certo? **Não existe uma URL** para a view localizada em **'meu_blog.views.contato'*, é isso que ele está dizendo.

Vamos criar essa URL então: abra o arquivo **"urls.py"** da pasta do projeto para edição e insira esta linha ao final da chamada **patterns()**:

        (r'^contato/$', 'views.contato'),
    

Salve o arquivo. Feche o arquivo. De volta ao navegador, pressione **F5** e olha o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contato-view-nao-existe-para-url/file/"/>
</p>

Bom, continuamos com uma mensagem de erro, mas agora ela mudou, veja:

    ViewDoesNotExist: Could not import views. Error was: No module named views
    

Em outras palavras: *não é possível importar** o módulo **views**, pois ele **não existe**. Pois então vamos criá-lo. Na **pasta do projeto** ( **meu\_blog** ), crie um novo arquivo chamadado **views.py** e escreva o seguinte código dentro:

    from django.shortcuts import render_to_response
    from django.template import RequestContext
    
    def contato(request):
        return render_to_response(
            'contato.html',
            locals(),
            context_instance=RequestContext(request),
            )

Salve o arquivo. Feche o arquivo.

Você deve ter estranhado que **criamos um novo** arquivo **views.py** na pasta do projeto, ao invés de usar o arquivo **views.py** da pasta da aplicação **blog**. É que esta não é uma funcionalidade do blog, e sim do site como um todo. Devemos lembrar que o projeto pode ter mais coisas no futuro, além do seu blog. Portanto, um arquivo **views.py** na pasta do projeto para funções que são específicas do projeto faz bastante sentido.

Mas então, de volta ao navegador, pressione **F5** e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/pagina-principal-com-link-para-contato/file/"/>
</p>

Agora o nosso link funcionou, apesar de ficar **um azul perdido no verde**. Vamos ajustar o estilo de nossa página para melhorar isso.

Na pasta do projeto, vá à pasta **"media"**, abra o arquivo **"layout.css"** para edição e acrescente esse trecho de código ao final do arquivo:

    .rodape {
      border: 1px solid black;
      background-color: white;
      color: gray;
      margin-top: 10px;
      padding: 10px;
    }

Salve o arquivo. Feche o arquivo. Volte ao navegador, pressione **F5** e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/pagina-principal-com-link-para-contato-2/file/"/>
</p>

Agora sim! E agora, ao clicar sobre o link que criamos, veja o que acontece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contato-template-nao-existe/file/"/>
</p>

Bom, nada mais claro: a template **"contato.html"** não existe.

Agora, vá à pasta **"templates"** do projeto e crie um novo arquivo: **"contato.html"**, com o seguinte código dentro:

    {% extends "base.html" %}
    
    {% block titulo %}Contato - {{ block.super }}{% endblock %}
    
    {% block h1 %}Contato{% endblock %}
    
    {% block conteudo %}
    {% if mostrar %}<h3>{{ mostrar }}</h3>{% endif %}
    
    Informe seus dados abaixo:
    
    <form method="post">
      <table>
        {{ form }}
    
        <tr>
          <th>&nbsp;</th>
          <td><input type="submit" value="Enviar contato"/></td>
        </tr>
      </table>
    </form>
    {% endblock conteudo %}

Salve o arquivo. Feche o arquivo.

Ufa! Bastante coisa, não é verdade? O que fizemos aí foi declarar, além dos **blocks** habituais, um formulário para o envio da mensagem de contato. De volta ao navegador, veja como ficou a página de contato:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contato-formulario-sem-campos/file/"/>
</p>

Mas como notou, só há o botão **"Enviar contato"**. De volta ao código, veja que escrevemos uma variável **{{ form }}**. É esta variável que vai trazer a mágica para dentro do template. E como ela não foi declarada na nossa **view**, é bastante natural que aqui não apareça nada além do que vemos.

### Trabalhando com os formulários dinâmicos do Django

Agora, abra o arquivo **"views.py"** da pasta do projeto novamente, e localize a seguinte linha:

    def contato(request):
    

Abaixo dela, acrescente a seguinte:

        form = FormContato()
    

Mas isso não basta, afinal, quem é o **FormContato** ali? Então agora, localize esta linha:

    from django.template import RequestContext
    

E acrescente este trecho de código:

    from django import forms
    
    class FormContato(forms.Form):
        nome = forms.CharField(max_length=50)
        email = forms.EmailField(required=False)
        mensagem = forms.Field(widget=forms.Textarea)

Agora o arquivo ficou assim:

    from django.shortcuts import render_to_response
    from django.template import RequestContext
    from django import forms
    
    class FormContato(forms.Form):
        nome = forms.CharField(max_length=50)
        email = forms.EmailField(required=False)
        mensagem = forms.Field(widget=forms.Textarea)
    
    def contato(request):
        form = FormContato()
        return render_to_response(
            'contato.html',
            locals(),
            context_instance=RequestContext(request),
            )

O que fizemos foi criar um **formulário**. Este formulário tem três campos: **nome**, **email** e **mensagem**. O campo **"nome"** é do tipo para receber *caracteres*, numa **largura máxima de 50 caracteres**. O campo **"email"** é do tipo para receber *e-mail*, mas **não é requerido**. Por fim, o campo **"mensagem"** é de um tipo livre, que será exibido como um **Textarea**, ou seja, uma caixa de texto maior que a habitual, que aceita muitas linhas livremente.

Salve o arquivo. Volte ao navegador e pressione **F5** para atualizar a página de contato. Veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contato-formulario-com-campos/file/"/>
</p>

Veja o quanto esse recurso de formulários dinâmicos do Django é fantástico! Temos o nosso formulário sendo exibido em um piscar de olhos! Mas isso não basta, pois ele ainda não está funcionando *de fato*.

Pois então voltemos ao arquivo **"views.py"** para localizar a seguinte linha:

        form = FormContato()
    

**Substitua** essa linha pelo seguinte trecho de código:

        if request.method == 'POST':
            form = FormContato(request.POST)
            
            if form.is_valid():
                form.enviar()
                mostrar = 'Contato enviado!'
        else:
            form = FormContato()

Esse nosso código faz o seguinte:

* Se o método dessa **request** é do tipo **POST**, entende-se que o usuário clicou sobre o botão **"Enviar contato"**. Isso faz sentido porque o carregamento padrão de páginas utiliza o método do tipo **GET**. Ou seja, **somente** quando o usuário clica no botão, e desde que a tag **&lt;form&gt;** do template possui um argumento **method="post"**, essa linha será válida;
  * Neste caso, a variável **"form"** recebe a instância do formulário **"FormContato"**, contendo os valores enviados no **request.POST**. Esses valores são aqueles que o usuário informou no formulário.
  * Feito isso, o formulário passa por uma validação ( **form.is\_valid()** ). É nesse momento que ele verifica se os campos **requeridos** foram todos preenchidos, e se todos os campos foram preenchidos corretamente de acordo com seus tipos (por exemplo: um endereço de e-mail não pode ter um formato diferente do habitual que conhecemos);
  * Se a validação seguir com sucesso, é chamado o método **form.enviar()**, que enviará a mensagem;
    - Por fim, é mostrada uma mensagem que avisa ao usuário que a mensagem **foi enviada** com sucesso.

No caso de qualquer coisa dessas acima não sair da forma esperada, as devidas mensagens de erro ou validação serão exibidas.

Salve o arquivo. Volte ao navegador. E faça o seguinte:

* Prencha **apenas** o campo **"E-mail"**, com um **e-mail inválido**
* Clique sobre o botão **"Enviar contato"** sem preencher os demais campos.

Veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contato-formulario-com-validacao/file/"/>
</p>

Você pode notar que os campos **requeridos** foram exigidos, e o formato do campo de **E-mail** não foi aceito.

A estética fica por conta do seu **CSS**.

Agora preencha todos os campos corretamente:

* Nome: **Mamãe**
* E-mail: **mamae.do.alatazan@katara.gov**
* Mensagem: **Olá filhinho, sou eu usando o e-mail do trabalho indevidamente**

E clique sobre **"Enviar contato"**. Veja o que acontece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contato-metodo-enviar-nao-existe/file/"/>
</p>

Puxa, mas o que é isso agora?

Anime-se! Isso é um bom sinal, pois significa que chegamos ao ponto no código onde tudo foi validado e o método **form.enviar()** não foi encontrado.

### Enviando o e-mail do contato

De volta ao arquivo **"views.py"**, localize a seguinte linha:

        mensagem = forms.Field(widget=forms.Textarea)
    

Agora acrescente abaixo dela:

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
    
Agora substitua o trecho **'alatazan@gmail.com'** pelo seu e-mail.

Mas afinal, o que esse método gigantesco faz?

* Ele define um **título** para o e-mail, que será enviado para você;
* Ele define que o **destino** desse e-mail será o seu e-mail;
* Ele define o **texto** desse e-mail com as informações preenchidas pelo usuário, usando a sintaxe de **formatação de strings** do Python;
* E por fim, ele envia um e-mail ( função **send\_mail()** ) com esses dados.

No **Python**, para informar uma string com mais de uma linha, você pode usar **três aspas** duplas ou simples, como por exemplo **"""esta aqui"""**.

Mas o método **send\_mail** não foi devidamente importado. Portanto, vá ao início do arquivo e acrescente a seguinte linha:

    from django.core.mail import send_mail
    

Agora, o arquivo **"views.py"** completo ficou assim:

    from django.shortcuts import render_to_response
    from django.template import RequestContext
    from django import forms
    from django.core.mail import send_mail
    
    class FormContato(forms.Form):
        nome = forms.CharField(max_length=50)
        email = forms.EmailField(required=False)
        mensagem = forms.Field(widget=forms.Textarea)
    
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
    
    def contato(request):
        if request.method == 'POST':
            form = FormContato(request.POST)
            
            if form.is_valid():
                form.enviar()
                mostrar = 'Contato enviado!'
                form = FormContato()
        else:
            form = FormContato()
            
        return render_to_response(
            'contato.html',
            locals(),
            context_instance=RequestContext(request),
            )

Salve o arquivo. Feche o arquivo. Volte ao navegador e pressione **F5**. Ele vai fazer uma pergunta sobre **processar dados**, devido ao método dessa requisição ter sido do tipo **POST**. Confirme a pergunta e veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contato-erro-de-conexao/file/"/>
</p>

Bom, avançamos, mas o que essa mensagem quer dizer?

    (10061, 'Connection refused')
    

Isso acontece por que é preciso configurar seu projeto com as informações de qual servidor **SMTP** ele deve usar para enviar esta mensagem de e-mail.

### Configurações de envio de e-mail

Para isso, abra o arquivo **"settings.py"** da pasta do projeto para edição, vá ao final do arquivo e acrescente o seguinte trecho de código dentro:

    EMAIL_HOST = 'seu endereço de SMTP'
    EMAIL_HOST_USER = 'seu endereço de e-mail'
    EMAIL_HOST_PASSWORD = 'sua senha'
    EMAIL_SUBJECT_PREFIX = '[Blog do Alatazan]'

Se caso seu servidor de e-mail utilizar **conexão segura**, acrescente esta linha:

    EMAIL_PORT = 587
    

Se caso seu servidor de e-mail utilizar conexão do tipo **TLS**, acrescente esta linha também:

    EMAIL_USE_TLS = True
    

Agora voltando ao navegador e pressionando **F5**, veja o que acontece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contato-enviado-com-sucesso/file/"/>
</p>

Pronto! Mais uma página virada em nossa caminhada!

## Partindo para outra forma de receber contato...

- Caramba! Quando eu vi o formulário com os campos na página depois de escrever menos de 10 linhas de código, fiquei espantado!

- Sim, e veja, o que você fez, num resumo rápido foi isso:

 - **Declarou um link** para a página de contato usando a template tag **{% url %}**;
 - Criou uma **view** para a página de contato;
 - Criou um **novo template** para a view de contato. Nele foi criado um **&lt;form&gt;**;
 - Declarou uma classe de **Form** e seus campos;
 - Definiu na view uma **condição** que verifica se o usuário clicou sobre o **botão de enviar** para efetuar o envio do e-mail;
 - Declarou um método **"enviar"**, que dá a ação que queremos ao form;
 - Esse método **formata o texto** da mensagem e **envia o e-mail**, usando uma função do Django para isso;
 - E configurou as **settings** para envio de e-mails.

- Ao todo foram **8 passos** para cumprir a nossa missão de hoje! E de quebra ainda fez um ajuste no visual no **rodapé**.

Alatazan estava em êxtase, seu coração da frente clareava o peito, e isso tinha a ver com sua alegria em chegar até aqui no estudo do Django.

- Bom, já que aprendemos tanto assim hoje, amanhã vamos criar recursos para seus usuários enviarem **Comentários** em seu blog. **Cada artigo** vai ter sua própria lista de comentários!

- Show de bola!

<span class="no_print">
**Próximo capítulo: [Deixando os comentários fluírem](http://www.aprendendodjango.com/deixando-os-comentarios-fluirem/)**
</span>

