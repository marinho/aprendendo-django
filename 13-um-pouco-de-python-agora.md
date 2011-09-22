Cartola estava excitado com o fato do robô ter sido programado em Python, mas estava achando estranhas algumas linhas de código que tinham erros de lógica incomuns.

Alatazan já desconfiou logo:

- **Ballzer**, você andou futricando na programação desse robô? Vai me dizer que já está me arrumando encrenca?

Ballzer baixou seus olhos, tratou de se esconder um pouco mais atrás da cama de Cartola e assentiu arrependido.

- Você está me dizendo que esse chimpanzé metálico em forma de polvo, programa em **Python**? - Cartola perguntou um pouco espantado.

- Programar não programa, sabe... mas ele adora mexer nas mesmas coisas que eu estou mexendo...

- Coitadinho, não fala assim com ele, ele é seu maior fã, Alatazan.

Nena disse isso mas Alatazan reagiu com aquela cara de "não piore as coisas, por favor".

- Bom, então vamos aproveitar esse momento pra falar sobre algumas **coisas fundamentais do Python**. É importante saber essas coisas antes de continuar com o estudo do Django, sabia?

## Python, uma linguagem maior que o Django...

Um dia desses Cartola comentou com Alatazan que **"o Python é a linguagem que manda os paradigmas para o espaço"**. É bem provável que Ballzer ouviu isso, pois *"mandar as coisas para o espaço"* é uma expressão particularmente especial para ele. O que acontece é que Ballzer não compreende muito bem essa coisa de metáforas, figuras, interpretações e tal, e deve ter entendido que aquela era a ferramenta ideal para um *oba-oba*.

Não, não... não é isso. O Python não é exatamente uma linguagem recomendada para quem gosta de gambiarras, muito pelo contrário, ela é indicada a pessoas que gostam de se divertir programando, mas fazendo as coisas do jeito certo.

Mas de fato, o Python rompe paradigmas, e que pode ser chamada também de **"linguagem multi-paradigmas"**.

### Orientada a objetos ou funcional?

Python é as duas, mas também permite orientação a aspectos, programação procedural e outras práticas de outros paradigmas.

Para desenvolver em Django usa-se muito funções isoladas (paradigma procedural), definição e instanciamento de classes (orientação a objetos), *decorators* (orientação a aspectos e funcional), etc.

Mas ainda assim, o principal paradigma que o Django trabalha no Python, é de fato a programação **orientada a objetos**.

### Interpretada ou Compilada?

O Python também trabalha com ambos os paradigmas. Quer proteger o código? Leve somente os arquivos **.pyc**. Isso vai dificultar o suficiente para quem quiser explorar suas regras de negócio. Quer trabalhar diretamente nos arquivos de código-fonte? Basta levar os arquivos **.py**.

Um código escrito em Python pode ser interpretado de várias formas:

* a forma tradicional, tecnicamente chamada **CPython**, é a forma como temos trabalhado neste estudo;

* o shell interativo do Python, é uma interpretação dinâmica em *shell* semelhante ao *prompt* do MS-DOS, onde é possível declarar objetos, classes, funções e fazer outras coisas de forma *on-line*;

* a compilação para classes **Java** ( arquivos **.class** ), usando **Jython**, para ser executado em máquina virtual **JVM**;

* a compilação para *bytecode* **.Net**, usando **IronPython**, para ser executado em máquina virtual .Net;

* e outras formas de compilação e/ou interpretação para **dispositivos de eletrônica e celulares**.

### Windows, Linux ou Mac?

Python também é multi-paradigma nesse sentido. Ele roda em Windows, Linux, MacOS, Unix, Solaris e outros sistemas operacionais menos conhecidos.

Para saber de outras plataformas onde o Python roda, veja esta página da Web:

    http://www.python.org/download/other/

Aqui, é preciso manter alguns cuidados para evitar problemas ao, por exemplo, executar em um servidor Linux um programa criado e testado em Windows.

As diferenças não são grandes, mas é importante ficar de olho nelas. Um caso clássico é o do separador de caminhos de arquivos. No Windows o separador é uma **barra invertida** ( \ ), nos demais é uma **barra comum** ( **/** ). Neste caso, você pode usar sempre a barra comum ou o elemento **os.sep**.

Mas raramente essas diferenças chegam a dar dores de cabeça, pois o Python já faz a maior parte para você.

### Codificação de caracteres do arquivo

Em uma pasta qualquer, crie um novo arquivo com o nome **"testar\_caracteres.py"** com o seguinte código dentro:

    def imprimir_duas_vezes(texto):
        # Imprime o texto uma vez
        print texto
    
        # É aqui que imprime outra vez
        print texto
    
    imprimir_duas_vezes('Tarsila do Amaral')

Salve o arquivo. Vá **Prompt do MS-DOS** (ou no **Console** do Linux), localize a pasta onde você salvou o arquivo e execute:

    python testar_caracteres.py
    

Será exibida a seguinte mensagem de erro:

    File "testar_caracteres.py", line 5
    SyntaxError: Non-ASCII character '\xc9' in file testar_caracteres.py on line 5,
    but no encoding declared; see http://www.python.org/peps/pep-0263.html for details

Isso acontece porque o seu comentário da linha 5 possui um **"É"** acentuado, veja:

        # É aqui que imprime outra vez
    

Mesmo que salve o arquivo no formato **UTF-8** (o que já é recomendável), isso não irá se resolver. É preciso adicionar o seguinte código ao arquivo, na primeira linha:

    # -*- coding: utf-8 -*-
    

Ou seja, agora o arquivo ficou assim:

    # -*- coding: utf-8 -*-
    
    def imprimir_duas_vezes(texto):
        # Imprime o texto uma vez
        print texto
    
        # É aqui que imprime outra vez
        print texto
    
    imprimir_duas_vezes('Tarsila do Amaral')

Salve o arquivo. Execute novamente:

    python testar_caracteres.py
    

Agora veja o resultado:

    Tarsila do Amaral
    Tarsila do Amaral

Isso é importante, pois o Python foi criado em solo **americano** por um **holandês** e é utilizado por pessoas do mundo inteiro, o que inclui **brasileiros**, **gregos**, **russos**, **chineses**, **japoneses**, **coreanos**, **indianos**, **isrealenses** e muitos outros, cada qual com seus caracteres particulares e outros em comum.

Portanto, o Python deve suportar a todos eles, suas particularidades e aquilo que há em comum entre eles.

### Identação

Outra característica fundamental do Python, é que ele foi pensado **para ser compreendido** pelo maior número possível de pessoas. Isso fez com que algumas definições *milenares* fossem chutadas para o espaço em busca de algo prático, claro e que ajudasse tudo isso de forma simpática (neste momento, **Ballzer** soltou um sorriso tão largo que quase separou seu corpo em dois). A grande sacada foi a **identação** padronizada para todo o código-fonte.

A identação da linha muda quando esta faz parte de um **bloco** declarado na linha anterior.

**Blocos** são sempre iniciados por uma linha que termina com o sinal de **dois-pontos** ( **:** ).

Veja este código novamente, por exemplo:

    # -*- coding: utf-8 -*-
    
    def imprimir_duas_vezes(texto):
        # Imprime o texto uma vez
        print texto
    
        # É aqui que imprime outra vez
        print texto
    
    imprimir_duas_vezes('Tarsila do Amaral')

O Python segue as linhas do arquivo de cima para baixo até encontrar a seguinte linha:

    def imprimir_duas_vezes(texto):

Esta linha define uma nova função, e inicia o bloco de código que **faz parte** dessa nova função. Daí em diante, as linhas são identadas com **4 espaços** da esquerda para a direita, o que dá uma leitura agradável do código para quem estava lá fora tomando um ar e chegou ali, coçando a orelha direita, enquanto mastiga um palito de dentes ainda do almoço.

A súbita sensação é esta:

- Sim, claro, já entendi o que esse código faz.

As linhas continuam identadas até que uma delas volta à identação anterior, veja:

    imprimir_duas_vezes('Tarsila do Amaral')
    

Ou seja, **nenhum espaço à esquerda**. Isso quer dizer que aquele bloco acabou, aquele trecho de código da função durou enquanto a identação estava lá, com **4 espaços** ou mais à esquerda, mas agora que a identação  voltou ao estado inicial, *voltamos* ao assunto anterior.

Compreendido?

Use sempre múltiplos de **4 espaços** (4, 8, 12, etc.). Nunca utilize **tabulação**, pois isso seria desastroso para o cara do palito de dentes, para o seu substituto nas férias ou para você mesmo, alguns dias depois. **Use sempre 4 espaços para identar de bloco em bloco**.

### Conhecendo o Python interativo

Agora, vá ao **Menu Iniciar** e escolha a opção **"Executar Programa"**. Escreva "python" no campo e clique em **"Ok"**.

No Linux, você pode executar o **Console**, digitar **"python"** e teclar ENTER.

A janela de modo texto do **MS-DOS** (ou do **Console**, no Linux) será aberta com um prompt assim:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/ipython/file/"/>
</p>

Pois bem, este é o **Python interativo**. É importante conhecê-lo, pois constantemente fazemos tarefas de administração de projetos usando esta ferramenta, que é muito prática também para o aprendizado.

### Tipagem

No Python, a tipagem dos dados e objetos é **dinâmica** e **forte**.

**"Dinâmico"** quer dizer que ao uma nova variável, ou um parâmetro de uma função, não é preciso informar qual é o seu tipo, nem mesmo defini-la antes de usá-la. Basta atribuir um valor a ela quando necessário.

No entanto, internamente todo valor é baseado em algum **tipo de dado**, que por sua vez é uma **classe**. Uma vez que um valor é atribuído a uma variável, ela passa a se comportar da forma que a classe daquele valor foi definida para se comportar. Por isso, ela é **"forte"**.

Para ver como isso funciona, volte ao **shell interativo** e digite a sequência de comandos abaixo. Escreva somente as linhas iniciadas por **">>> "**, e claro, ignore o **">>> "**, digitando somente o restante da linha.

    >>> nome = 'Leticia'
    >>> nome
    'Leticia'
    >>> nome.__class__
    <type 'str'>
    >>> type(nome)
    <type 'str'>

Nas linhas de código acima, nós fizemos o seguinte:

1. declaramos uma variável **"nome"** com uma *string* **'Leticia'**;
1. escrevemos o nome da variável para que seu valor original seja exibido na linha de baixo, ou seja: **'Leticia'**;
1. na linha seguinte, informamos o atributo interno do Python que retorna a classe daquele objeto, e ele mostrou que o valor contido na variável **"nome"** é um tipo básico de string ( **"&lt;type 'str'&gt;"** );
1. e na última linha usamos uma outra forma de fazer a mesma coisa da linha anterior.

Agora faça algo semelhante, só que desta forma:

    >>> idade = 28
    >>> idade
    28
    >>> idade.__class__
    <type 'int'>

Veja que o valor **"28"** foi interpretado como número inteiro ( **"&lt;type 'int'&gt;"** ).

Veja esses outros:

    >>> nota = 8.7
    >>> nota
    8.6999999999999993
    >>> nota.__class__
    <type 'float'>
    >>> mulher = True
    >>> mulher
    True
    >>> mulher.__class__
    <type 'bool'>
    >>> nota * idade
    243.59999999999997
    >>> idade * nota
    243.59999999999997

### Import explícito e namespaces

Agora, depois de ver alguns **tipos de dados básicos**, vamos partir para outro um pouquinho mais complexo. No **shell interativo**, digite a seguinte sequência de comandos:

    >>> from datetime import date
    >>> hoje = date.today()
    >>> hoje
    datetime.date(2008, 11, 24)
    >>> hoje.__class__
    <type 'datetime.date'>

Como pode ver, para trabalhar com datas, é preciso **importar** um pacote chamado **"date"**, de dentro de outro pacote, chamado **"datetime"**.

**Importar** um pacote ou objeto trata-se de fazer o Python saber que quando aquele elemento for citado, ele deve ser localizado em uma biblioteca diferente do módulo em que você está agora, e diferente também da biblioteca *built-in* do Python, que é carregada automaticamente.

Há outras formas de se importar um pacote ou objeto, veja:

    >>> import datetime
    >>> datetime.date.today()
    datetime.date(2008, 11, 24)

Observe que aqui, nós importamos o pacote **"datetime"** inteiro. Então, para usá-lo, precisamos informar todo o caminho para a função **today()**.

    >>> datetime = __import__('datetime')
    >>> datetime.date.today()
    datetime.date(2008, 11, 24)

Neste outro bloco, importamos o pacote **"datetime"** da mesma forma, só que agora seu nome foi informado dentro de uma string. Isso é útil para se importar pacotes quando seus nomes serão conhecidos somente em tempo de execução.

    >>> from datetime import *
    >>> date.today()
    datetime.date(2008, 11, 24)

Já neste último bloco, importamos todos os elementos de **"datetime"**, o que inclui também o elemento **"date"**, então o usamos logo em seguida.

Acontece, que Alatazan sabe que quando se traz uma manada de elefantes de uma região da floresta, e outra manada de elefantes de outra região da floresta, mas não se usa nem um papelzinho para dizer que onde vieram, então a bagunça está definitivamente... **feita**!

E quando se usa esse famigerado **"from algum\_lugar import \*"** e outro **"from outro\_lugar import *"**, acontece a mesma coisa: você não sabe quem veio, e também não sabe de onde um elemento específico veio, o que é muito ruim.

Portanto não é aconselhável o uso de **"import \*"**, a menos que seja em um caso específico e simples.

Isso se chama **"namespace"**.

### Declaração de classes

Vamos agora **declarar uma classe** no **shell**? Pois então digite as linhas de código abaixo. As linhas iniciadas com **"... "** são blocos, portanto, use os **4 espaços** para identar o bloco.

    >>> class Pessoa:
    ...     nome = 'Leticia'
    ...     idade = 28
    ...     nota = 8.7
    ...     mulher = True
    ...     # nao se esqueca da identação para continuar o bloco
    ...     def andar(self, passos=10):
    ...         if self.idade > 120:
    ...             print 'Nao foi possivel'
    ...         else:
    ...             print 'Andou %d passos!'%passos
    ...
    >>>

Puxa, fizemos um bocado de coisas aí.

Primeiro, você deve ter notado que a cada início de um novo bloco, mais 4 espaços devem acrescentar à identação e o contrário também é verdadeiro.

Em segundo, declaramos alguns atributos para a classe (**nome**, **idade**, **nota** e **mulher**) já com valores iniciais. Isso não é obrigatório, mas dá uma **clareza** aliviadora para **aquele mesmo cara do palito**.

Em terceiro, você deve notar que, no **shell interativo**, quando um bloco tem um espaço em branco entre suas linhas (para manter a estética e facilidade de leitura), este deve seguir a mesma identação, justamente porque senão ele vai pensar que você acabou de fechar o bloco, o que não é a sua intenção naquele momento. Mas essa preocupação não é necessária quando se está escrevendo código em um arquivo **.py**.

### Self explícito na declaração de métodos

Por último, declaramos o método **andar(self, passos=10)**. Para ser um método é preciso ter **self** como primeiro parâmetro e ser uma função. O parâmetro **passos** possui um valor *default*, ou seja, caso não seja especificado, ele assumirá o valor **10**.

Agora escreva as seguintes linhas de código na sequência:

    >>> mychell = Pessoa()
    >>> mychell.nome = 'Mychell'
    >>> mychell.idade = 15
    >>> mychell.nome
    'Mychell'
    >>> mychell.idade
    15
    >>> mychell.andar()
    Andou 10 passos!
    >>> mychell.andar(15)
    Andou 15 passos!

Primeiro, instanciamos a classe **Pessoa** e atribuímos essa instância à variável **"mychell"**.

Depois atribuímos valores a seus atributos, para modificar os valores iniciais.

E por último, executamos o método **andar**.

Na primeira execução do método, não informamos um valor para o parâmetro **passos**, então o valor assumido foi **10**. Veja:

    >>> mychell.andar()
    Andou 10 passos!

Na segunda execução, informamos um valor para o parâmetro. Observe que o parâmetro **self** foi completamente ignorado. Isso é porquê ele deve ser informado somente na declaração, e não na execução do método.

    >>> mychell.andar(15)
    Andou 15 passos!

## Atribuição de método e inicializador de classe

Pois agora vamos brincar um pouco com programação **funcional** e atribuir um método especial de **inicialização** à classe **Pessoa**, veja:

    >>> def inicializador(self, nome, idade, nota=None, mulher=None):
    ...     self.nome = nome
    ...     self.idade = idade
    ...     self.nota = nota or self.nota
    ...     self.mulher = mulher is not None or self.mulher
    ...
    >>> Pessoa.__init__ = inicializador

Veja só:

Primeiro, declaramos uma nova função, chamada **"inicializador"**, com os atributos **self**, **nome**, **idade**, **nota** e **mulher**. Os dois últimos possuem valores *default*.

A seguinte linha diz, em outras palavras: "o atributo **"self.nota"** deve receber o valor do parâmetro **"nota"** somente se este tiver um **valor válido**, do contrário, recebe o seu próprio valor para continuar tudo como está".

Neste caso, um **valor válido** é um valor **verdadeiro**. Para o Python, os valores **0**, **False**, **None**, **''**, **()**, **[]** e **{}** não são verdadeiros.

    ...     self.nota = nota or self.nota
    

Já na linha abaixo, a coisa é bem parecida, mas o valor do parâmetro **"mulher"** só será atribuído ao atributo **"self.mulher"** caso ele **não seja None**, ou seja, seu valor *default*.

    ...     self.mulher = mulher is not None or self.mulher
    

É importante perceber aqui, que o que estamos fazendo é atribuir os valores informados para os parâmetros **opcionais** somente se estes foram realmente informados, pois **None** é seu valor default, e indica que **ele não foi informado**.

Na segunda parte, atribuímos a função à classe, como se fosse um atributo, de nome **\_\_init\_\_**, mas como ela é uma função, o Python a atribui como **método**.

Agora veja a seguir:

    >>> ana = Pessoa()
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: inicializador() takes at least 3 arguments (1 given)

No código acima, ao instanciar a classe **Pessoa**, desta vez não foi possível, pois o seu **inicializador** obriga que pelo menos os parâmetros **sem valor _default_** sejam informados.

    >>> ana = Pessoa('Ana Paula', 8)
    

Agora fizemos do jeito certo, atribuindo o **nome 'Ana Paula'** e a **idade 8**.

Ao imprimir os valores dos atributos na tela, eles são exibidos, até mesmo o atributo **"mulher"**, que permaneceu com seu valor *default*.

    >>> ana.nome
    'Ana Paula'
    >>> ana.idade
    8
    >>> ana.mulher
    True

Para finalizar essa sequência, você deve saber que poderia informar o **inicializador** com qualquer outro nome, ou poderia - o que é o mais indicado sempre - simplesmente declarar o método da forma convencional, assim por exemplo:

    class Pessoa:
        nome = 'Leticia'
        idade = 28
        nota = 8.7
        mulher = True
        
        def __init__(self, nome, idade, nota=None, mulher=None):
            self.nome = nome
            self.idade = idade
            self.nota = nota or self.nota
            self.mulher = mulher is not None or self.mulher
        
        def andar(self, passos=10):
            if self.idade > 120:
                print 'Nao foi possivel'
            else:
                print 'Andou %d passos!'%passos

### O resto também deve ser explícito

O Python segue a idéia de que tudo deve ser posto às caras, livre, e muito, muito explícito. Use nomes de variáveis, classes e funções, todos explícitos. Comentários explícitos, etc... tudo deve ser claro... e falando nessas coisas, também é recomendável...

### Padrão de nomenclatura

... que as variáveis tenham seus nomes em caixa baixa (letras minúsculas), com as palavras separadas por *underscore* ( \_ ), assim:

    nome = 'Marta'
    data_nascimento = None

... que funções e métodos sejam verbos, assim:

    def andar_para_frente():
        print nome, 'andou'

... e que as classes tenham nomes no singular, com a primeira letra de cada palavra em caixa alta, assim:

    class PessoaFisica:
        nome = 'Leticia'

### Comentários

... e que comentários podem ser feitos assim:

    # linha comentada que sera ignorada da mesma forma
    # que esta outra linha

ou assim:

    """linha comentada que sera ignorada da mesma forma
    que esta outra linha"""

E é muito aconselhável que todas as funções e classes tenham um comentário no início do bloco, assim:

    class Pessoa:
        """Classe para atribuicao a pessoas do sistema"""
        nome = None
        idade = 20

Pois ao chamar a função **help()** para aquela classe ou função no **shell interativo**, aquele comentário será interpretado como a **documentação** da classe. Veja com seus próprios olhos:

    >>> help(Pessoa)
    Help on class Pessoa in module __main__:
    
    class Pessoa
     |  Classe para atribuicao a pessoas do sistema
     |
     |  Data and other attributes defined here:
     |
     |  nome = None
     |  idade = 20

**Não é fantástico?**

### Funções dir() e help()

Aliás, falando em função **help()**, ela é uma forma muito bacana de aprender Python, veja:

    >>> import datetime
    >>> help(datetime)
    Help on built-in module datetime:
    
    NAME
        datetime - Fast implementation of the datetime type.
    
    FILE
        (built-in)
    
    CLASSES
        __builtin__.object
            date
                datetime
            time
    
    ---- cortamos aqui pois a documentação deste pacote é extensa

Ou este:

    >>> import urllib
    >>> help(urllib)
    Help on module urllib:
    
    NAME
        urllib - Open an arbitrary URL.
    
    FILE
        c:\python25\lib\urllib.py
    
    DESCRIPTION
        See the following document for more info on URLs:
        "Names and Addresses, URIs, URLs, URNs, URCs", at
        http://www.w3.org/pub/WWW/Addressing/Overview.html
    
        See also the HTTP spec (from which the error codes are derived):
        "HTTP - Hypertext Transfer Protocol", at
    
    ---- cortamos denovo

Isso é muito útil e ajuda bastante.

E tão útil quanto a função **help()** é a função **dir()**, veja

    >>> dir(Pessoa)
    ['__doc__', '__init__', '__module__', 'andar', 'idade', 'mulher', 'nome', 'nota']

A função **dir()** retorna em uma lista todos atributos e métodos daquela instância, e isso vale também para classes.

Portanto, se você quer saber quais atributos e métodos a string **'Beatles'** possui, faça isto:

    >>> dir('Beatles')
    ['__add__', '__class__', '__contains__', '__delattr__', '__doc__', '__eq__', '__ge__', 
    '__getattribute__', '__getitem__', '__getnewargs__', '__getslice__', '__gt__', '__hash__', '__init__', 
    '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', 
    '__repr__', '__rmod__', '__rmul__', '__setattr__', '__str__', 'capitalize', 'center', 'count', 
    'decode','encode', 'endswith', 'expandtabs', 'find', 'index', 'isalnum', 'isalpha', 'isdigit', 'islower', 
    'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'partition', 'replace', 'rfind', 
    'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 
    'swapcase', 'title', 'translate', 'upper', 'zfill']

E agora, sabendo dos atributos e métodos dessa string, volte ao **help()**:

    >>> help('Beatles'.upper)
    Help on built-in function upper:
    
    upper(...)
        S.upper() -> string
    
        Return a copy of the string S converted to uppercase.

Novamente: **o Python é fantástico**!

## Com muita empolgação, rumo ao *deploy*

Alatazan ficou fascinado, e Ballzer, como era de se esperar, começou a formular suas aventuras futuras.

- Puxa, evidentemente o Python é mais do que isso, mas só em saber disso muito do que já fizemos e vamos fazer nos próximos dias se esclareceu de forma tão... **_pythonica_**!

- Meu camarada, é muito, muito bacana programar em Python. E o mais interessante é que as coisas são realmente simples, veja que é muito fácil casar classes com funções de formas variadas. É por isso que existem tantos *frameworks* criados em Python!

Cartola ajustou o volume de seu iPod e prosseguiu:

- Vamos resumir a parada:

 - Python é multi-paradigma: **orientado a objetos, orientado a aspectos, procedural, funcional e outros**;
 - pode ser compilado ou interpretado;
 - roda em qualquer sistema operacional que seja popular;
 - Python é unicode, por isso, é preciso informar a codifição do arquivo se for usar caracteres especiais;
 - Python constrói blocos por identação, sempre de 4 em **4 espaços**;
 - O **shell interativo** é uma mão-na-roda em muitas situações;
 - a tipagem de dados no Python é **dinâmica e forte**;
 - faça tudo **explícito** e use *namespaces*;
 - classes também são objetos, e métodos devem ser declarados sempre com **self** sendo o primeiro parâmetro;
 - funções comuns podem ser transformadas em métodos;
 - comentários em string no início do bloco são a **documentação** daquela função ou classe;
 - as funções **help()** e **dir()** abrem as cortinas para o esclarecimento.

Pronto! Agora com conceitos básicos sobre HTML, CSS e Python, podemos seguir adiante. No próximo capítulo, vamos fechar a primeira iteração e partir para mais aventuras!

<span class="no_print">
**Próximo capítulo: [Ajustando as coisas para colocar no ar](/ajustando-as-coisas-para-colocar-no-ar/)**
</span>

