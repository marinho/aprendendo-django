Alatazan estava curioso com o desempenho de _Neninha_, sua modelo predileta.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_7.gif" align="right"/>

- E aí, como foi lá, Nena?

- Puxa, foi mó legal, parece que eles gostaram dela!

- É? E ela foi contratada?

- Ahhh, você sabe como é o papai né... categórico, inseguro, todo certinho... ainda vai estudar melhor...

- Mas como isso funciona, essa coisa das passarelas e tal? Tem que começar tão novinha assim?

- É sim. Olha, é como uma obra de arte, sabe... quando uma modelo abre as cortinas e sai pela passarela, ela aparece e ganha os cliques, mas o fato é que sem um **empresário** no _backstage_, desde pequena, pra acertar as coisas e dar todo o suporte, ela vai ser só mais um rosto bonitinho por trás de um balcão de uma loja de roupas...

- Sei...

- E tem a **estilista** também, sabe... toda espirituosa! Ela produz, dá vida à modelo... é ela quem faz a arte. A modelo só representa o papel.

E então? Neninha estava perto de ser a nova contratada de uma dessas agências de modelo, mas daria pra confiar naquele _Manager_? E para quem ela representaria? Bom, vamos vendo...

## Buscando os números finais

No capítulo anterior, criamos uma aplicação de **Contas Pessoais** - a pagar e a receber.

Nós falamos muito sobre heranças e algumas funcionalidades bacanas do Admin. Mas hoje, vamos aprofundar um pouco mais ainda no ORM do Django.

Como você já sabe, o ORM é o **Mapeador Objeto/Relacional**. Trata-se de uma parte do Django que traduz as coisas de objetos Python para expressões em linguagem SQL, de forma a persistir as informações no banco de dados.

O ORM possui uma **trindade** fundalmental:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/model-manager-queryset/file/"/>
</p>

Através dos vários capítulos que se passaram, falamos em muitas ocasiões sobre classes de modelo, aquelas que herdam da classe **"django.db.models.Model"**, mas nada falamos sobre o Manager e a QuerySet.

Pois vamos falar agora!

### O Manager

Toda classe de modelo possui um Manager padrão. Ele se trata do atributo **"objects"** da classe, veja aqui um exemplo onde o Manager está presente e você nem sabia disso:

    def albuns(request):
        lista = Album.objects.all()
        return render_to_response(
            'galeria/albuns.html',
            locals(),
            context_instance=RequestContext(request),
            )

Observe ali esta linha:

        lista = Album.objects.all()
    

Sim, aquele **"objects"** ali se trata do Manager da classe **"Album"**!

E sabe o que ele faz? Ele é o ponto de apoio da classe. Veja, na grande maioria das situações em que usamos a classe de modelo diretamente, ele está lá, veja:

Criando um objeto...

    novo_album = Album.objects.create(nome='Meu novo album')
    
Carregando um objeto:

    album = Album.objects.get(id=1)
    

Carregando ou criando um objeto:

    novo_album, novo = Album.objects.get_or_create(nome='Meu novo album')
    

Carregando uma lista de objetos:

    lista = Album.objects.all()
    

Veja que lá está o Manager... nem um pouco popular e aparecido quanto o Model, mas ele está sempre lá, dando todo o suporte necessário e dando liga às coisas.

### A QuerySet

E existe também a QuerySet! Ela é de fato quem dá vida ao Modelo, sempre atendendo prontamente aos pedidos do Manager.

A QuerySet efetiva as **persistências** e **consultas** no banco de dados, e é ela quem de fato dá ação à coisa e faz a arte na comunicação com o banco de dados... veja novamente os exemplos acima... ela também está lá!

O método **"create()"** é passado do Manager **"objects"** para sua QuerySet padrão:

    novo_album = Album.objects.create(nome='Meu novo album')
    

Aqui novamente: o método **"get()"** é passado à QuerySet padrão do Manager:

    album = Album.objects.get(id=1)
    

A mesma coisa: o método **"get\_or\_create()"** também é passado à QuerySet padrão do Manager:

    novo_album, novo = Album.objects.get_or_create(nome='Meu novo album')
    

E aqui mais uma vez:

    lista = Album.objects.all()
    



Você pode notar que na maioria das operações que tratam uma classe de modelo **não instanciada** em sua relação com o banco de dados, lá estarão presentes também a organização do **Manager** e a arte da **QuerySet**.

As QuerySets são extremamente **criativas e excitantes** porque elas possuem métodos que podem ser encadeados uns aos outros, de forma a criar uma complexa e comprida sequência de filtros e regras, que somente quando é de fato requisita, é transformada em uma expressão SQL e efetivada ao banco de dados.

Agora vamos ver essas coisas na prática?

### Criando o primeiro Manager

Para começar, vá até a pasta da aplicação **"contas"** e abra o arquivo **"models.py"** para edição. Localize a seguinte linha:

    class Historico(models.Model):
    

**Acima** dela, acrescente o seguinte trecho de código:

    class HistoricoManager(models.Manager):
        def get_query_set(self):
            query_set = super(HistoricoManager, self).get_query_set()
    
            return query_set.extra(
                select = {
                    '_valor_total': """select sum(valor) from contas_conta
                                      where contas_conta.historico_id = contas_historico.id""",
                    }
                )

Opa! Mas espera aí, que tanto de novidades de uma vez só, vamos com calma né!

Ok, então vamos linha por linha...

Observe bem esta linha abaixo. Estamos criando ali um **Manager** para a classe **"Historico"**. Mas porquê criar um manager se ela já possui um?

É que o manager padrão tem um comportamento padrão, e nós queremos mudar isso. Sabe o que estamos indo fazer aqui? Nós vamos criar um novo **campo calculado**, que irá carregar o **valor total** das **contas a pagar e a receber** ligadas ao objeto de **Histórico**!

Todo Manager deve herdar a classe **"models.Manager"**:

    class HistoricoManager(models.Manager):
    

Este método que declaramos, **"get\_query\_set"** retorna a QuerySet padrão toda vez que um método do Manager é chamado para ser passado a ela. Estamos aqui novamente mudando as coisas padrão, para ampliar seus horizontes:

        def get_query_set(self):
    

E a primeira coisa que o método faz é chamar o mesmo método da classe herdada, mais ou menos aquilo que fizemos em um dos capítulos anteriores, lembra-se?

            query_set = super(HistoricoManager, self).get_query_set()
    

Logo adiante, nós retornamos a **QuerySet**, encadeando o método **"extra()"** à festa.

O método **"extra()"** tem o papel de acrescentar informações que nenhuma das outras acrescentam. Aqui podemos acrescentar campos calculados adicionais, condições especiais e outras coisinhas assim...

            return query_set.extra(
    

E o argumento que usamos no método **"extra()"** é o **"select"**, que acrescenta um campo calculado ao resultado da QuerySet, veja:

                select = {
                    '_valor_total': """select sum(valor) from contas_conta
                                      where contas_conta.historico_id = contas_historico.id""",
                    }

Com esse novo campo, chamado **"\_valor\_total"**, podemos agora exibir a soma total do campo **"valor"** de todas as **Contas** vinculadas ao Histórico.

Está um pouco confuso? Não se preocupe, vamos seguir adiante e aos poucos as coisas vão se encaixando... você não queria entender como funciona o **mundo da moda** em apenas algumas poucas palavras né?

Agora localize esta linha, ela faz parte da classe **"Historico"**:

    descricao = models.CharField(max_length=50)
    

Acrescente abaixo dela:

    objects = HistoricoManager()

    def valor_total(self):
        return self._valor_total


Veja o que fizemos:

Primeiro, efetivamos o Manager que criamos - **"HistoricoManager"** - para ser o Manager padrão da classe **"Historico"**.

Vale ressaltar que uma classe de modelo pode possuir quantos Managers você quiser, basta ir criando outros atributos e atribuindo a eles as instâncias dos managers que você criou...

    objects = HistoricoManager()
    

E aqui nós criamos uma função para retornar o valor do campo calculado **"\_valor\_total"**.

Sabe porquê fizemos isso? Porque há algumas funcionalidades no Django que exigem atributos declarados diretamente na classe, e como o campo **"\_valor\_total"** é calculado, ele "aparece" na classe somente quando esta é instanciada, resultando de uma QuerySet.

Então o que fizemos? Nós já criamos o campo com um caractere **"\_"** como prefixo, e o encapsulamos por trás de um método criado manualmente...

    def valor_total(self):
        return self._valor_total

Ok, vamos agora fazer mais uma coisinha, mas não aqui.

Salve o arquivo. Feche o arquivo.

Na mesma pasta, abra agora o arquivo **"admin.py"** para edição, e localize este trecho de código:

    class AdminHistorico(ModelAdmin):
        list_display = ('descricao',)

Modifique a segunda linha para ficar assim:

    class AdminHistorico(ModelAdmin):
        list_display = ('descricao','valor_total',)

Você notou que acrescentamos o campo **"valor\_total"** à listagem da classe **"Historico"**?

Salve o arquivo. Feche o arquivo.

Agora execute o projeto, clicando duas vezes sobre o arquivo **"executar.bat"** e vá até o navegador, carregando a seguinte URL:

    http://localhost:8000/admin/contas/historico/
    

Veja como ela aparece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-admin-com-campo-de-valor-total/file/"/>
</p>

Uau! Isso é legal, gostei disso! Mas vamos dar um jeito de tirar aquele **"None"** dali e deixar um **zero** quando a soma retornar um valor vazio? OK, vamos lá!

Na pasta da aplicação **"contas"**, abra o arquivo **"models.py"** para edição e localize estas linhas de código:

    def valor_total(self):
        return self._valor_total

Modifique a segunda linha, para ficar assim:

    def valor_total(self):
        return self._valor_total or 0.0

Isso vai fazer com que, caso o valor contido no campo calculado **"\_valor\_total"** seja inválido, o valor **"0.0"** retorne em seu lugar.

Salve o arquivo.

Agora vamos fazer o mesmo campo calculado para a classe **"Pessoa"**. Localize a seguinte linha:

    class Pessoa(models.Model):
    

Acima dela, acrescente este trecho de código:

    class PessoaManager(models.Manager):
        def get_query_set(self):
            query_set = super(PessoaManager, self).get_query_set()
    
            return query_set.extra(
                select = {
                    '_valor_total': """select sum(valor) from contas_conta
                                      where contas_conta.pessoa_id = contas_pessoa.id""",
                    '_quantidade_contas': """select count(valor) from contas_conta
                                             where contas_conta.pessoa_id = contas_pessoa.id""",
                    }
                )

Veja que dessa vez fomos um pouquinho além, nós criamos dois campos calculados: um para **valor total** das contas e outro para sua **quantidade**. Observe que as _subselects_ SQL para ambos são semelhantes, com a diferença de que a do **valor total** usa a agregação **"SUM"**, enquanto que a de **quantidade** usa **"COUNT"**.

Agora localize esta outra linha:

        telefone = models.CharField(max_length=25, blank=True)
    

E acrescente abaixo dela:

        objects = PessoaManager()
    
        def valor_total(self):
            return self._valor_total or 0.0
    
        def quantidade_contas(self):
            return self._quantidade_contas or 0

Você pode notar que a definimos o Manager da classe Pessoa e em seguida declaramos as duas funções que fazem acesso aos campos calculados que criamos.

Mas qual é a diferença entre retornar **"0.0"** e retornar **"0"**?

Quando um número possui uma ou mais **casas decimais**, separadas pelo ponto, o Python entende que aquele é um **número flutuante**, e já quando não possui, é um número inteiro. O **valor total** das contas sempre será um valor flutuante, mas a **quantidade** de contas sempre será em número inteiro. É isso.

Salve o arquivo. Feche o arquivo. Agora é preciso ajustar o Admin da classe **"Pessoa"**.

Abra o arquivo **"admin.py"** da mesma pasta para edição e localize o seguinte trecho de código:

    class AdminPessoa(ModelAdmin):
        list_display = ('nome','telefone',)

Modifique-o para ficar assim:

    class AdminPessoa(ModelAdmin):
        list_display = ('nome','telefone','valor_total','quantidade_contas')

Salve o arquivo. Feche o arquivo.

Volte ao navegador e carregue esta URL:

    http://localhost:8000/admin/contas/pessoa/
    

E veja como ela se sai:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-admin-de-pessoa-com-campos-calculado/file/"/>
</p>

Bacana, não é? Agora vamos criar uma **conta a pagar** para ver como a coisa toda funciona, certo?

Vá a esta URL:

    http://localhost:8000/admin/contas/contapagar/add/
    

E crie uma **Conta a Pagar** como esta abaixo:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-nova-conta-a-pagar/file/"/>
</p>

Salve a conta e volte à URL da classe **"Pessoa"** no Admin:

    http://localhost:8000/admin/contas/pessoa/
    

E veja agora como ela está:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-pessoas-com-valores-sem-sinal/file/"/>
</p>

Isso não ficou legal... o campo de **quantidade de contas** mostra seu valor corretamente, mas veja o campo de **valor total**: se lançamos uma **Conta a Pagar**, ela deveria estar em **valor negativo**.

Então para resolver isso, abra o arquivo **"models.py"** da pasta da aplicação **"contas"** para edição e localize esta linha. Ela está dentro do método **"get\_query\_set()"** da classe **"HistoricoManager"**:

                    '_valor_total': """select sum(valor) from contas_conta
                                      where contas_conta.historico_id = contas_historico.id""",

Modifique-a para ficar assim:

                    '_valor_total': """select sum(valor * case operacao when 'c' then 1 else -1 end)
                                       from contas_conta
                                       where contas_conta.historico_id = contas_historico.id""",

Observe que fazemos uma condição ali, e **multiplicamos** o campo **"valor"** por **1** ou **-1** caso a **operação financeira** seja de **crédito** ou não (de **"débito"**, no caso), respectivamente.

Agora vamos fazer o mesmo com a classe **"PessoaManager"**:

                    '_valor_total': """select sum(valor) from contas_conta
                                      where contas_conta.pessoa_id = contas_pessoa.id""",

Modifique para ficar assim:

                    '_valor_total': """select sum(valor * case operacao when 'c' then 1 else -1 end)
                                       from contas_conta
                                       where contas_conta.pessoa_id = contas_pessoa.id""",

Salve o arquivo. Feche o arquivo. Volte ao navegador, pressione **F5** para atualizar, e veja como ficou:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-pessoas-com-valores-com-sinal/file/"/>
</p>

Satisfeito agora?

### Totalizando campos

Bom, agora que você já teve um contato razoável com um Manager e sua QuerySet padrão, que tal fazermos uma pequena mudança no Admin das **Contas**?

Vamos fazer assim: na pasta da aplicação **"contas"**, crie uma nova pasta, chamada **"templates"** e dentro dela outra pasta chamada **"admin"**.

Diferente? Pois vamos criar mais pastas: dentro da nova pasta **"admin"**, crie outra, chamada **"contas"** e dentro dela mais uma, chamada **"contapagar"**.

Agora dentro desta última pasta criada, crie um novo arquivo, chamado **"change\_list.html"**. Deixe-o vazio por enquanto.

Agora feche a janela do Django **em execução** (aquela janela do MS-DOS que fica sempre _pentelhando_ por ali) e execute novamente, para que a nossa nova pasta de **templates** faça efeito. Para isso, clique duas vezes no arquivo **"executar.bat"** da pasta do projeto.

Agora vá à URL da classe **"ContaPagar"** no Admin, assim:

    http://localhost:8000/admin/contas/contapagar/
    

Veja:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-conta-a-pagar-vazia/file/"/>

</p>

Gostou? Lógico que não, mas o que houve de errado?

O arquivo **"change\_list.html"** que criamos e deixamos vazio fez esse estrago. Sempre quando existe uma pasta de **templates** com uma pasta **"admin"**, dentro dela uma pasta com o nome da aplicação ( **"contas"** ) e dentro dela uma pasta com o nome da classe de modelo ( **"contapagar"** ), o Admin do Django tenta encontrar ali um arquivo chamado **"change\_list.html"** para usar como **template da listagem** daquela classe. E o arquivo está vazio, o que mais você queria que acontecesse?

Pois então abra esse arquivo e escreva o seguinte código dentro:

    {% extends "admin/change_list.html" %}
    
    {% block result_list %}
    <p>
      Quantidade: {{ total }}<br/>
      Soma: {{ soma|floatformat:2 }}
    </p>
    {% endblock result_list %}

Observe que no início de tudo, o template **"admin/change\_list.html"** é herdado. Esse template faz parte do próprio Django e deve ser usado como base para nossas modificações. Ele é o template original que o Django sempre usa para listagens no Admin.

    {% extends "admin/change_list.html" %}
    

E logo depois, nós extendemos o bloco **"result\_list"** e colocamos ali duas linhas: uma para exibir a **quantidade** e outra, a **soma** das contas a pagar:

    {% block result_list %}
    <p>
      Quantidade: <b>{{ total }}</b><br/>
      Soma: <b>{{ soma|floatformat:2 }}</b>
    </p>
    {% endblock result_list %}

Salve o arquivo e volte ao navegador, atualize com **F5** e veja:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-conta-a-pagar-sem-lista-de-objetos/file/"/>
</p>

Opa, cadê as minhas contas? Não se sinta aliviado, suas contas sumiram mas continuam existindo... é que nós nos esquecemos de uma coisa importante.

Localize esta linha no template que estamos editando:

    {% block result_list %}
    

E modifique para ficar assim:

    {% block result_list %}{{ block.super }}
    

Isso vai evitar que perdamos o que já está lá, funcionando bonitinho.

Salve o arquivo. Feche o arquivo. Atualize o navegador com **F5** e veja:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-conta-a-pagar-sem-valores-no-sumario/file/"/>
</p>

Pronto, voltamos ao normal, com o nosso **sumário** ali, ainda que com valores vazios.

Agora vá até a pasta da aplicação **"contas"** e abra o arquivo **"admin.py"** para edição. Localize este trecho de código nele:

    class AdminContaPagar(ModelAdmin):
        list_display = ('data_vencimento','valor','status','historico','pessoa')
        search_fields = ('descricao',)
        list_filter = ('data_vencimento','status','historico','pessoa',)
        exclude = ['operacao',]
        inlines = [InlinePagamentoPago,]
        date_hierarchy = 'data_vencimento'

Aí está a classe de Admin da classe **"ContaPagar"**. Acrescente este bloco de código abaixo dela:

        def changelist_view(self, request, extra_context={}):
            qs = self.queryset(request)
            
            extra_context['soma'] = sum([i['valor'] for i in qs.values('valor')])
            extra_context['total'] = qs.count()
            
            return super(AdminContaPagar, self).changelist_view(request, extra_context)

Observe que se trata de um novo método para a classe de Admin da classe **"ContaPagar"**. Este método é a _view_ da URL de listagem do Admin desta classe de modelo. Na segunda linha, nós carregamos a **QuerySet** para ele, usando sua **request** (requisição):

        def changelist_view(self, request, extra_context={}):
            qs = self.queryset(request)
    

A QuerySet que carregamos para a variável **"qs"** já é devidamente filtrada e organizada pela chamada ao método **"self.queryset(request)"**, mas ela vai ser útil para nós.

Na declaração do método há um argumento chamado **"extra\_context"** e o nosso trabalho aqui é dar a ele duas novas variáveis:

            extra_context['soma'] = sum([i['valor'] for i in qs.values('valor')])
            extra_context['total'] = qs.count()

Uau! Veja, o método **"values()"** de uma QuerySet retorna **somente** os valores dos campos informados como argumentos, em uma lista de dicionários, assim:

    >>> qs.values('valor')
    [{'valor': Decimal("44.53")}, {'valor': Decimal("1.0")}]

Nós temos ali uma _list comprehension_, criada para retornar somente o conteúdo do campo valor, assim:

    >>> [i['valor'] for i in qs.values('valor')]
    [Decimal("44.53"), Decimal("1.0")]

E por fim, a função **"sum"** é uma função padrão do Python que soma todos os valores de uma lista, assim:

    >>> sum([i['valor'] for i in qs.values('valor')])
    Decimal("45.53")

Então, a soma do campo **"valor"** será atribuída ao item **"soma"** do dicionário **"extra\_context"**.

E veja que na linha seguinte nós retornamos a **quantidade** de contas a pagar:

            extra_context['total'] = qs.count()
    

O método **"count()"** da QuerySet retorna a quantidade total dos objetos.

Por fim, a última linha é esta:

            return super(AdminContaPagar, self).changelist_view(request, extra_context)
    

Como já existe um método **"changelist\_view()"** na classe herdada, nós invocamos o **super()** para trazer o tratamento do método original, mas acrescentamos o dicionário **"extra\_context"**, que vai levar as variáveis **"soma"** e **"quantidade"** ao contexto dos templates.

Salve o arquivo. Feche o arquivo. Volte ao navegador e pressione **F5**. Veja o resultado:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-conta-a-pagar-com-sumario-funcionando/file/"/>
</p>

Muito bom! Agora crie outras contas a pagar, e observe como a **quantidade** e a **soma** dos valores são atualizados nessa página.

## Agora, por quê não um front-end mais amigável para isso?

Alatazan mal concluiu essa sequência e já começou a ter idéias...

- Porquê não criar uma seção no site para outras pessoas organizarem suas contas pessoais?

- Bacana, camarada, como seria isso?

- Sei lá, algo assim: o cara entra, faz um cadastro e começa a organizar suas **Contas Pessoais**. Simples assim...

- Alatazan, gostei da idéia, mas vamos resumir o estudo de hoje e deixar isso para amanhã?

Nena tinha razão... havia uma coisa em comum entre eles naquele momento: **a fome roncando na barriga**...

- Ok, vamos lá:

 - Não existe Modelo sem Manager, e não existe Manager sem QuerySet. E as QuerySets trabalham principalmente com a classe de Modelo, é um ciclo que nunca se acaba;
 - A classe de modelo define como é sua **estrutura** e sua instância representa um **objeto** resultante dessa estrutura;
 - A classe de Manager **organiza** as coisas, é como o empresário de jogadores de futebol... mas o nosso Manager é justo e ético;
 - A classe de QuerySet é o **artista** da coisa, faz as coisas acontecerem de forma muito elegante;
 - Toda classe de modelo possui um Manager padrão, que possui uma QuerySet padrão;
 - O Manager padrão é atribuído ao atributo **objects** da classe de modelo, já a QuerySet padrão é retornada pelo método **"get\_query\_set()"** do Manager;
 - A QuerySet pode receber inúmeros métodos encadeados, como **filter()**, **exclude()**, **extra()**, **count()**, **values()**, etc. Somente quando for requisitada ela vai gerar uma expressão **SQL** como resultado disso e irá enviar ao banco de dados, da forma mais otimizada e leve possível;
 - Para interferir no funcionamento da listagem do Admin, devemos sobrepôr o método **"changelist_view()"** da classe de Admin;
 - E para mudar o template da listagem, criamos o template **"change\_list.html"** dentro de uma árvore de pastas que começa da pasta de **templates**, depois uma para o próprio **admin**, outra para o **nome da aplicação** e mais uma para o **nome da classe de modelo**.
 - Algo mais?

- Ahh, camarada... eu já ia esquecendo de dizer uma coisa importate: quando uma QuerySet é requisitada ao banco de dados, o resultado do que ela resgatou de lá ficar em um **cache interno**. E caso ela seja requisitada novamente, ela será feita direto na memória, sem precisar de ir ao banco de dados de novo...

- Então vamos comer né gente!

Nos próximos capítulos vamos criar uma interface para a aplicação de **Contas Pessoais** e permitir que outros usuários trabalhem com ela.

