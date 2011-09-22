- Clic!

Nada.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_13.gif" align="right"/>

- Cloc! Clic!

Nada.

- Cloc! Clic! Cloc! Clic!

Tudo escuro, nadica de nada.

- Ah, Ballzer, para com isso...

A porta se abriu um pouco mais. Continua tudo escuro.

- Paw! Hoje é dia 9! Eu não podia ter esquecido, hoje não!

Alatazan se sentou na beirada da porta com a cabeça entre as mãos, e Ballzer lhe apareceu do escuro, mastigando uns papéis...

- Vem cá seu pirralho, você ainda está mastigando o resto das minhas contas, está querendo ficar sem água também?

Ballzer não deu a mínima para a questão da água, mas fingiu preocupação...

Já não bastava o dinheiro acabando e o senhorzinho do **"Compra-se Ouro"** cada dia mais desconfiado, agora Alatazan teria também de lidar com a companhia elétrica pra resolver aquele impasse. Mas tudo foi por falta de alguma coisa pra fazê-lo se lembrar do vencimento de suas contas...

... para pagar e receber também, o Romão do parque estava atrasado em 5 dias e só agora ele se lembrava disso.

O que Alatazan precisava era de um sistema simples para controlar suas **contas pessoais**.

## Mergulhando no ORM do Django

Da mesma forma como aconteceu nos capítulos do _deploy_ esta será uma sequência de **4 capítulos**, pois vamos percorrer por diversos aspectos da construção de sistemas no Django e aprofundar um pouco mais o conhecimento em diversas áreas que já conhecemos.

Ao final dessa brincadeira, teremos uma nova aplicação com o seguinte modelo:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-modelo/file/"/>
</p>

O centro da nossa aplicação são as classes **"ContaPagar"** e **"ContaReceber"**. São elas que estão no centro do **negócio** da aplicação.

Mas elas possuem exatamente os mesmos campos, e é muito útil ter **entradas e saídas** de um sistema contas de forma fácil de calcular.

Se você quiser por exemplo saber o saldo de suas **Contas a Pagar** com suas **Contas a Receber** em um mês, basta somar tudo, sendo que os **valores** das Contas a Pagar devem ser invertidos, o que pode ser facilmente resolvido com uma **multiplicação por -1**.

E é por isso que vamos usar um recurso muito bacana do Django: a **herança de modelo**.

### Herança persistente

Na classe **"Conta"** serão concentrados todos os campos e a maior parte da lógica do negócio. Esta classe deve ser **herdada** pelas classes **"ContaPagar"** e **"ContaReceber"**, que possuem somente definições diferentes para o campo da **operação financeira**, que indica se a conta é de **Débito** ou de **Crédito**.

Dessa forma as tabelas no banco de dados serão criadas assim:

    CREATE TABLE "contas_conta" (
        "id" integer NOT NULL PRIMARY KEY,
        "data_vencimento" date NULL,
        "data_pagamento" date NULL,
        "status" varchar(1) NOT NULL,
        "historico_id" integer NOT NULL REFERENCES "contas_historico" ("id"),
        "pessoa_id" integer NOT NULL REFERENCES "contas_pessoa" ("id"),
        "descricao" text NOT NULL,
        "operacao" varchar(1) NOT NULL,
        "valor" decimal NOT NULL
    );
    
    CREATE TABLE "contas_contapagar" (
        "conta_ptr_id" integer NOT NULL PRIMARY KEY REFERENCES "contas_conta" ("id")
    
    );
    
    CREATE TABLE "contas_contareceber" (
        "conta_ptr_id" integer NOT NULL PRIMARY KEY REFERENCES "contas_conta" ("id")
    
    );

Esse tipo de **herança de modelo** se chama **Herança Persistente**, pois há de fato uma tabela para cada uma das **classes de modelo** envolvidas e a classe **Conta** pode ser usada normalmente para o que precisar.

### Herança abstrata

Mas há outra forma de herança, chamada **Herança Abstrata**. E este é o caso das classes **"PagamentoPago"** e **"PagamentoRecebido"**.

Apesar de possuírem campos semelhantes, cada uma dessas classes é **agregada** a uma classe diferente: **"PagamentoPago"** faz parte de **Contas a Pagar**, enquanto **"PagamentoRecebido"** é parte do assunto de **Contas a Receber**.

Por isso ambas são **especializações** da classe **"Pagamento"**, que é uma classe definida como **"abstrata"**, ou seja, ela tem somente o papel de concentrar definições comuns, mas não está presente no banco de dados e só pode ser usada como herança para outras classes.

Nesse caso, as tabelas no banco de dados ficam assim:

    CREATE TABLE "contas_pagamentopago" (
        "id" integer NOT NULL PRIMARY KEY,
        "data_pagamento" date NOT NULL,
        "valor" decimal NOT NULL,
        "conta_id" integer NOT NULL REFERENCES "contas_contapagar" ("conta_ptr_id")
    );
    
    CREATE TABLE "contas_pagamentorecebido" (
        "id" integer NOT NULL PRIMARY KEY,
        "data_pagamento" date NOT NULL,
        "valor" decimal NOT NULL,
        "conta_id" integer NOT NULL REFERENCES "contas_contareceber" ("conta_ptr_id")
    );

Observe que há ali somente **2 tabelas**, pois a classe **"Pagamento"** não é persistente, e seus campos estão declarados redundantemente nas tabelas das classes filhas.

Os pagamentos de uma conta são importantes porque há muitas situações onde se paga uma conta por partes... você se lembra daquela vez que estava devendo mas não tinha a grana toda, então deu a metade e na semana seguinte voltou para pagar o restante? Pois essas classes de **Pagamento** são para aquele tipo de situação.

Além do que já dissemos, há ainda as classes **"Pessoa"** e **"Historico"**, que são auxiliares no negócio, sem grandes pretensões, mas com possibilidades de se expandirem.

E por fim, há dois campos que não são esclarecidos no diagrama, que são os campos **"operacao"** e **"status"** da classe **"Conta"**. Eles são campos do tipo que muitos chamam de _flags_, pois identificam o objeto em um grupo específico. Ambos os campos serão definidos para aceitarem somente algumas opções de **escolha**, você vai ver isso daqui a pouco.

### Criando a aplicação e declarando as classes

Bom, chega de blá-blá-blá e vamos ao que interessa!

Na pasta do projeto, crie a pasta da nova aplicação, chamada **"contas"**, e dentro dela um arquivo vazio, chamado **"\_\_init\_\_.py"**. Depois disso, abra o arquivo **"settings.py"** e acrescente a aplicação à setting **"INSTALLED\_APPS"**:

    'contas',
    

E a setting agora ficou assim:

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
        'contas'
    )

Salve o arquivo. Feche o arquivo.

Agora vá até a pasta da aplicação **"contas"** e crie o arquivo **"models.py"** com o seguinte código dentro:

    # -*- coding: utf-8 -*-
    from django.db import models
    from django.utils.translation import ugettext_lazy as _
    
    class Historico(models.Model):
        class Meta:
            ordering = ('descricao',)
            
        descricao = models.CharField(max_length=50)
    
        def __unicode__(self):
            return self.descricao
    
    class Pessoa(models.Model):
        class Meta:
            ordering = ('nome',)
    
        nome = models.CharField(max_length=50)
        telefone = models.CharField(max_length=25, blank=True)
    
        def __unicode__(self):
            return self.nome

Você pode notar que não há novidades aí. Criamos duas as classes de modelo que servirão às demais, com uma lógica bastante simples.

Salve o arquivo. Feche o arquivo.

Agora vamos criar um segundo arquivo na mesma pasta, chamado **"admin.py"** com o seguinte código dentro:

    from django.contrib import admin
    from django.contrib.admin.options import ModelAdmin
    
    from models import Pessoa, Historico
    
    class AdminPessoa(ModelAdmin):
        list_display = ('nome','telefone',)
    
    class AdminHistorico(ModelAdmin):
        list_display = ('descricao',)
    
    admin.site.register(Pessoa, AdminPessoa)
    admin.site.register(Historico, AdminHistorico)

Também nenhuma novidade, apenas duas classes simples, com seus respectivos campos para a listagem.

Salve o arquivo. Feche o arquivo.

Vá até a pasta do projeto e execute o arquivo **"gerar\_banco\_de\_dados.bat"** para gerar as tabelas no banco de dados, veja o resultado:

    Creating table contas_historico
    Creating table contas_pessoa

Agora execute o projeto, clicando duas vezes sobre o arquivo **"executar.bat"**.

Vá ao navegador, carregue a URL do **Admin** e veja as novas classes de modelo:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-admin-com-duas-classes/file/"/>
</p>

Até aqui tudo bem, certo? Pisando em terreno seguro, nenhuma novidade, chega a ser monótono...

Pois agora volte à pasta da aplicação **"contas"** e abra o arquivo **"models.py"** para edição. Acrescente o seguinte código ao final do arquivo:

    CONTA_OPERACAO_DEBITO = 'd'
    CONTA_OPERACAO_CREDITO = 'c'
    CONTA_OPERACAO_CHOICES = (
        (CONTA_OPERACAO_DEBITO, _('Debito')),
        (CONTA_OPERACAO_CREDITO, _('Credito')),
    )
    
    CONTA_STATUS_APAGAR = 'a'
    CONTA_STATUS_PAGO = 'p'
    CONTA_STATUS_CHOICES = (
        (CONTA_STATUS_APAGAR, _('A Pagar')),
        (CONTA_STATUS_PAGO, _('Pago')),
    )
    
    class Conta(models.Model):
        class Meta:
            ordering = ('-data_vencimento', 'valor')
    
        pessoa = models.ForeignKey('Pessoa')
        historico = models.ForeignKey('Historico')
        data_vencimento = models.DateField()
        data_pagamento = models.DateField(null=True, blank=True)
        valor = models.DecimalField(max_digits=15, decimal_places=2)
        operacao = models.CharField(
            max_length=1,
            default=CONTA_OPERACAO_DEBITO,
            choices=CONTA_OPERACAO_CHOICES,
            blank=True,
            )
        status = models.CharField(
            max_length=1,
            default=CONTA_STATUS_APAGAR,
            choices=CONTA_STATUS_CHOICES,
            blank=True,
            )
        descricao = models.TextField(blank=True)

E agora? Se assustou um pouco mais? Mas não se preocupe, as novidades não são muitas...

O trecho de código abaixo declara algumas variáveis úteis para identificar as opções do campo **"operacao"**. Resumindo, este campo aceita somente a escolha entre as opções **"Débito"** ou **"Crédito"**, representadas internamente pelas letras **"d"** e **"c"** respectivamente. Por exemplo: quando você escolher a opção **"Débito"** na verdade o valor salvo no banco de dados será **"d"**.

    CONTA_OPERACAO_DEBITO = 'd'
    CONTA_OPERACAO_CREDITO = 'c'
    CONTA_OPERACAO_CHOICES = (
        (CONTA_OPERACAO_DEBITO, _('Debito')),
        (CONTA_OPERACAO_CREDITO, _('Credito')),
    )

A sopa de letras em caixa alta pode assustar, mas tratam-se apenas de 2 variáveis de string com as letras **"d"** e **"c"**, e uma terceira variável com uma **tupla** contendo as 2 variáveis anteriores, seguidas por seus respectivos **rótulos**.

Agora o trecho abaixo segue a mesma sintaxe:

    CONTA_STATUS_APAGAR = 'a'
    CONTA_STATUS_PAGO = 'p'
    CONTA_STATUS_CHOICES = (
        (CONTA_STATUS_APAGAR, _('A Pagar')),
        (CONTA_STATUS_PAGO, _('Pago')),
    )

Ou seja, uma Conta pode ter dois **status** diferentes: ou a Conta está **A Pagar**, ou ela já foi **Paga**. Uma conta que não foi **completamente** paga, deve ser considerada **"A Pagar"** até que toda a dívida seja quitada.

O próximo trecho desconhecido para você será este:

        valor = models.DecimalField(max_digits=15, decimal_places=2)
    

O campo **"valor"** é um campo para valor **decimal** de largura definida. Isso significa que este campo pode ser preenchido com valores numéricos flutuantes com até **15 dígitos**, sendo **2 deles depois da vírgula**.

E a seguir temos os dois campos que fazem uso das variáveis que declaramos acima:

        operacao = models.CharField(
            max_length=1,
            default=CONTA_OPERACAO_DEBITO,
            choices=CONTA_OPERACAO_CHOICES,
            blank=True,
            )
        status = models.CharField(
            max_length=1,
            default=CONTA_STATUS_APAGAR,
            choices=CONTA_STATUS_CHOICES,
            blank=True,
            )

Os campos **"operacao"** e **"status"** possuem uma largura de **1 caracter** (pois os valores **"d"**, **"c"**, **"a"** e **"p"** não precisam de mais do que 1 caracter). Cada um possui seu próprio **valor default**, representado por uma daquelas variáveis. E o argumento **"choices"** indica a **tupla** com a lista de opções de escolha.

Salve o arquivo. Feche o arquivo.

Abra novamente o arquivo **"admin.py"** pra edição e localize esta linha:

    from models import Pessoa, Historico
    

Modifique para ficar assim:

    from models import Pessoa, Historico, Conta
    

Agora, localize esta outra linha:

    admin.site.register(Pessoa, AdminPessoa)
    

Acrescente **acima** dela o seguinte trecho de código:

    class AdminConta(ModelAdmin):
        list_display = ('data_vencimento','valor','status','operacao','historico','pessoa',)
        search_fields = ('descricao',)
        list_filter = ('data_vencimento','status','operacao','historico','pessoa',)

E por fim, acrescente esta outra linha ao final do arquivo:

    admin.site.register(Conta, AdminConta)
    

Observe que o que fizemos é apenas o que já conhecemos: o atributo **"list\_display"** define quais colunas devem ser exibidas na listagem de contas, o atributo **"search\_fields"** define em quais campos as buscas devem ser feitas, e o atributo **"list\_filter"** indica quais campos serão opções para filtro na listagem.

Salve o arquivo. Feche o arquivo.

Agora volte à pasta do projeto e crie as novas tabelas no banco de dados, executando o arquivo **"gerar\_banco\_de\_dados.bat"**. O resultado será este:

    Creating table contas_conta
    Installing index for contas.Conta model

Vá ao navegador, no Admin, atualize a página e veja que temos a nova classe **Conta** ali.

Agora volte novamente ao arquivo **"models.py"** da pasta da aplicação **"contas"**. Acrescente as seguintes linhas de código ao final:

    class ContaPagar(Conta):
        def save(self, *args, **kwargs):
            self.operacao = CONTA_OPERACAO_DEBITO
            super(ContaPagar, self).save(*args, **kwargs)
    
    class ContaReceber(Conta):
        def save(self, *args, **kwargs):
            self.operacao = CONTA_OPERACAO_CREDITO
            super(ContaReceber, self).save(*args, **kwargs)

Veja que agora temos algo bastante diferente aqui.

Lembra-se de que falamos que as classes **"ContaPagar"** e **"ContaReceber"** são heranças persistentes da classe **"Conta"**? Pois é isso que esta linha faz:

    class ContaPagar(Conta):
    

E esta também:

    class ContaReceber(Conta):
    

E você deve se lembrar também que conversamos sobre cada uma dessas classes definir valores diferentes para o campo **"operacao"**, pois a classe **"ContaPagar"** deve sempre trabalhar com operação de **Débito** e a classe **"ContaReceber"** deve ser o inverso, trabalhando sempre com operação de **Crédito**.

Pois é exatamente isso que a nossa sobreposição faz aqui:

        def save(self, *args, **kwargs):
            self.operacao = CONTA_OPERACAO_DEBITO
            super(ContaPagar, self).save(*args, **kwargs)

Ou seja, ao salvar, antes de ser efetivamente gravado no banco de dados, o valor do campo **"operacao"** é forçado a ser o que queremos. O mesmo acontece aqui:

        def save(self, *args, **kwargs):
            self.operacao = CONTA_OPERACAO_DEBITO
            super(ContaReceber, self).save(*args, **kwargs)

Salve o arquivo. Feche o arquivo.

Agora abra o arquivo **"admin.py"** para edição e localize esta linha

    from models import Pessoa, Historico, Conta
    

Modifique-a para ficar assim:

    from models import Pessoa, Historico, Conta, ContaPagar, ContaReceber
    

E agora localize esta outra linha:

    admin.site.register(Pessoa, AdminPessoa)
    

E acrescente este trecho de código **acima** dela:

    class AdminContaPagar(ModelAdmin):
        list_display = ('data_vencimento','valor','status','historico','pessoa')
        search_fields = ('descricao',)
        list_filter = ('data_vencimento','status','historico','pessoa',)
        exclude = ['operacao',]
    
    class AdminContaReceber(ModelAdmin):
        list_display = ('data_vencimento','valor','status','historico','pessoa')
        search_fields = ('descricao',)
        list_filter = ('data_vencimento','status','historico','pessoa',)
        exclude = ['operacao',]

Observe que há uma ligeira diferença entre essas duas classes e a classe **"AdminConta"** que está posicionada logo acima delas. Além de não haver o campo **"operacao"** nos atributos **"list\_display"** e **"list\_filter"**, há também um novo atributo, chamado **"exclude"**, que oculta o campo **"operacao"** da página de manutenção dessas duas classes.

E agora ao final do arquivo, acrescente estas outras duas linhas:

    admin.site.register(ContaPagar, AdminContaPagar)
    admin.site.register(ContaReceber, AdminContaReceber)

Salve o arquivo. Feche o arquivo.

Execute o arquivo **"gerar\_banco\_de\_dados.bat"** da pasta do projeto para gerar as novas tabelas. Veja o resultado:

    Creating table contas_contapagar
    Creating table contas_contareceber

Agora volte ao navegador e carregue a **URL do Admin**:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-admin-com-todas-as-classes/file/"/>
</p>

Bom, ao menos nossas classes já estão aí!

Agora crie algumas Contas a Pagar e outras a Receber, assim:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-conta-a-receber-no-admin/file/"/>
</p>

E veja como fica a página de listagem:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-conta-a-receber-no-admin-lista/file/"/>
</p>

Como você pode ver, temos aí um conjunto de filtros bacana, e caso você vá até a página de listagem da classe **"Conta"** no Admin..

    http://localhost:8000/admin/contas/conta/
    

... você pode ver que a mesma conta é exibida, pois de fato ela faz parte também da classe **"Conta"**!

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-admin-da-classe-conta/file/"/>
</p>

Mas ainda faltam duas classes para criarmos: as classes de **Pagamento**.

Abra novamente o arquivo **"models.py"** da pasta da aplicação **"contas"** para edição e acrescente o seguinte trecho de código ao final do arquivo:

    class Pagamento(models.Model):
        class Meta:
            abstract = True
    
        data_pagamento = models.DateField()
        valor = models.DecimalField(max_digits=15, decimal_places=2)
    
    class PagamentoPago(Pagamento):
        conta = models.ForeignKey('ContaPagar')
    
    class PagamentoRecebido(Pagamento):
        conta = models.ForeignKey('ContaReceber')

Bom, a primeira novidade que temos é este trecho de código:

        class Meta:
            abstract = True

É aquele atributo **"abstract = True"** ali que determina que a classe **"Pagamento"** é abstrata, ou seja, não possui uma tabela no banco de dados.

As demais classes **"PagamentoPago"** e **"PagamentoRecebido"** herdam essa classe e implementam cada uma seu próprio campo **"conta"**, que indica um relacionamento com sua classe de agregação.

Salve o arquivo. Feche o arquivo.

Agora abra o arquivo **"admin.py"** da mesma pasta, e vamos fazer uma coisinha diferente...

Localize esta linha:

    from django.contrib.admin.options import ModelAdmin
    

Modifique-a para ficar assim:

    from django.contrib.admin.options import ModelAdmin, TabularInline
    

A classe **"TabularInline"** permite a criação de recursos em **"master/detalhe"**, o que significa que você pode controlar vários itens de uma classe agregada dentro de outra. Isso é bastante divertido!

Agora localize esta outra linha:

    from models import Pessoa, Historico, Conta, ContaPagar, ContaReceber
    

Modifique-a, para ficar assim:

    from models import Pessoa, Historico, Conta, ContaPagar, ContaReceber,\
         PagamentoPago, PagamentoRecebido

Dê um pouco de atenção ao último caractere da primeira linha. É uma barra invertida ( **"\\"** ).

No Python, tudo aquilo que não está dentro de algum delimitador específico, ao fazer um salto de linha para sua continuidade é preciso que o salto de linha seja feito com uma **barra invertida**.

Por exemplo: o seguinte caso não precisa da barra invertida, pois está entre os delimitadores de **parênteses**:

    tupla = ('verinha','waltinho','mychell','mychelle','miltinho',
        'leni','dinha','davi')

Nem mesmo este caso abaixo, que está entre **chaves**:

    lista = ['verinha','waltinho','mychell','mychelle','miltinho',
        'leni','dinha','davi']

E nem mesmo este, entre **colchetes**:

    mychell = {'nome': 'Mychell da Cruz', 'idade': 87, 'cidade': 'Mara Rosa',
        'estado': 'GO'}

Mas neste caso, é preciso, pois não há delimitador:

    from models import Pessoa, Historico, Conta, ContaPagar, ContaReceber,\
         PagamentoPago, PagamentoRecebido

Pois bem, vamos continuar então... localize esta outra linha:

    class AdminContaPagar(ModelAdmin):
    

E acrescente este trecho **acima** dela:

    class InlinePagamentoPago(TabularInline):
        model = PagamentoPago

Esta classe permite a edição ou inclusão de vários objetos da classe **"PagamentoPago"** de uma só vez, mas para isso ser possível, ela precisa ser parte de outra. Neste caso, a "outra" trata-se da classe **"AdminContaPagar"**, portanto, localize este trecho de código:

    class AdminContaPagar(ModelAdmin):
        list_display = ('data_vencimento','valor','status','historico','pessoa')
        search_fields = ('descricao',)
        list_filter = ('data_vencimento','status','historico','pessoa',)
        exclude = ['operacao',]

E acrescente esta linha ao final do bloco:

        inlines = [InlinePagamentoPago,]
    

Precisamos fazer o mesmo para a classe **"PagamentoRecebido"**. Para isso, localize a linha abaixo:

    class AdminContaReceber(ModelAdmin):
    

E acrescente **acima** dela:

    class InlinePagamentoRecebido(TabularInline):
        model = PagamentoRecebido

E a seguir, localize este bloco de código:

    class AdminContaReceber(ModelAdmin):
        list_display = ('data_vencimento','valor','status','historico','pessoa')
        search_fields = ('descricao',)
        list_filter = ('data_vencimento','status','historico','pessoa',)
        exclude = ['operacao',]

E acrescente a seguinte linha abaixo ao final do bloco:

        inlines = [InlinePagamentoRecebido,]
    

Depois de todas essas mudanças, o nosso arquivo **"admin.py"** completo ficou assim:

    from django.contrib import admin
    from django.contrib.admin.options import ModelAdmin, TabularInline
    
    from models import Pessoa, Historico, Conta, ContaPagar, ContaReceber,\
         PagamentoPago, PagamentoRecebido
    
    class AdminPessoa(ModelAdmin):
        list_display = ('nome','telefone',)
    
    class AdminHistorico(ModelAdmin):
        list_display = ('descricao',)
    
    class AdminConta(ModelAdmin):
        list_display = ('data_vencimento','valor','status','operacao','historico','pessoa')
        search_fields = ('descricao',)
        list_filter = ('data_vencimento','status','operacao','historico','pessoa',)
    
    class InlinePagamentoPago(TabularInline):
        model = PagamentoPago
    
    class AdminContaPagar(ModelAdmin):
        list_display = ('data_vencimento','valor','status','historico','pessoa')
        search_fields = ('descricao',)
        list_filter = ('data_vencimento','status','historico','pessoa',)
        exclude = ['operacao',]
        inlines = [InlinePagamentoPago,]
    
    class InlinePagamentoRecebido(TabularInline):
        model = PagamentoRecebido
    
    class AdminContaReceber(ModelAdmin):
        list_display = ('data_vencimento','valor','status','historico','pessoa')
        search_fields = ('descricao',)
        list_filter = ('data_vencimento','status','historico','pessoa',)
        exclude = ['operacao',]
        inlines = [InlinePagamentoRecebido,]
    
    admin.site.register(Pessoa, AdminPessoa)
    admin.site.register(Historico, AdminHistorico)
    admin.site.register(Conta, AdminConta)
    admin.site.register(ContaPagar, AdminContaPagar)
    admin.site.register(ContaReceber, AdminContaReceber)

Como pode ver, o arquivo esticou-se rapidinho.

Salve o arquivo. Feche o arquivo. Execute o arquivo **"gerar\_banco\_de\_dados.bat"** da pasta do projeto para criar as novas tabelas no banco de dados. Veja o resultado:

    Creating table contas_pagamentopago
    Creating table contas_pagamentorecebido
    Installing index for contas.PagamentoPago model
    Installing index for contas.PagamentoRecebido model

Agora com o banco de dados devidamente atualizado, vá até esta URL no Admin:

    http://localhost:8000/admin/contas/contareceber/1/
    

Veja o que acontece:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-conta-a-receber-com-inlines/file/"/>
</p>

Incrível, não? Agora vamos fazer uma coisa a mais...

### Agrupando a listagem do admin por data

Uma coisa muito bacana do Admin é que é possível organizar a listagem por data, de forma que você pode visualizar os dados pelo **ano**, **mês** e **dia** de uma forma amigável.

Para fazer isso, abra o arquivo **"admin.py"** da pasta da aplicação **"contas"** para edição e localize a seguinte linha:

        inlines = [InlinePagamentoPago,]
    

Observe bem que existe outra linha semelhante a ela, mas estamos falando aqui da classe **"AdminContaPagar"**. Acrescente a seguinte linha de código abaixo dela:

        date_hierarchy = 'data_vencimento'
    

Agora localize esta outra:

        inlines = [InlinePagamentoRecebido,]
    

E acrescente esta abaixo dela:

        date_hierarchy = 'data_vencimento'
    

Salve o arquivo. Feche o arquivo. Volte ao navegador, nesta URL do Admin:

    http://localhost:8000/admin/contas/contareceber/
    

Observe que a página mudou levemente, veja:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/contas-admin-agrupado-por-data/file/"/>
</p>

Ao clicar sobre o ano, serão exibidos os meses daquele ano, e assim por diante, sempre limitados somente aos dados disponíveis.

## A última linha é a que mais importa...

Isso facilitou para Alatazan com a nova tarefa que Romão - seu cliente de um pequeno sistema financeiro - pediu. O que ele queria era um sistema, sem grandes complicações, e o Admin casa perfeitamente com isso.

- Humm... então vou anotar aqui um resumo do que aprendi hoje...

 - Há dois tipos de herança de modelos: **Herança Persistente** e **Herança Abstrata**;
 - Uma classe de modelo abstrata não cria tabela no banco de dados, apenas serve de referência para suas tabelas filhas;
 - Uma classe de modelo persistente cria uma tabela e suas filhas apenas fazem um relacionamento com ela e buscam os dados de sua própria tabela. O Django se vira pra resolver isso e trazer tudo em forma de objetos redondinhos e bonitinhos;
 - Existem campos que oferecem apenas algumas opções para **escolha**. Para esses casos, usamos o argumento **"choices"**;
 - Classes filhas podem ajustar os dados das classes herdadas para se adequarem à sua realidade;
 - Para ocultar um ou mais campos da página de manutenção de uma classe no Admin, usa-se o atributo **"exclude"**;
 - **Barra invertida** ao final da linha indica que a linha abaixo será sua continuação, isso é legal pra evitar a escrita de linhas extensas e difíceis de compreender;
 - Para fazer **master/detalhe** no Django, é preciso usar a classe **"TabularInline"** e o atributo **"inlines"** da classe de Admin;
 - Usa-se o atributo **"date\_hierarchy"** para organizar a listagem por um campo de data.

- É.. hoje a minha atenção ficou focada nas heranças e em algumas coisas do Admin, mas amanhã quero criar campos calculados, com somas, sumários e outras coisas que os chefes geralmente gostam...

