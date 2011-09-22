# DESCRICAO CURTA

<img src="http://www.aprendendodjango.com/media/img/ilustracao_6.gif" align="right"/>

Neste capítulo, algumas coisas começam a ficar mais claras para Alatazan.

Ele compreende como é o fluxo das requisições e respostas no Django, como elas saem do navegador, vão até seu ponto final e o Django retorna uma resposta ao navegador. Geralmente uma bela página HTML colorida.

E pela primeira vez, Alatazan compreende o que é o **MVC** e como ele acontece dentro do Django.

Isso tranquilizou Alatazan, e ele segue pisando em terreno firme.

# TEXTO

Quando ainda era criança, Alatazan andava pela rua alegremente quando zuniu
uma bola de metal cheia de braços perto de sua cabeça.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_6.gif" align="right"/>

Na verdade não eram muitos braços, eram apenas dois mesmo.

A bola de metal era uma espécie de robô de modelo antigo. Seus circuítos
pareciam não funcionar muito bem, mas ele emanava um sorriso arregalado, num
jeito espalhafatoso que Alatazan gostou.

Um homem gritou:

- Vai **Ballzer**! Vai **Ballzer**!

O robô seguiu seu caminho, voando a menos de 2 metros de altura em direção à praça.

Na praça havia uma mesa comprida. Antes da mesa estava um **menino vestido de 
despachante**, segurando um carimbo, e sentados ao longo da mesa haviam robôs de 
modelos diferentes, cada um com alguns apetrechos. Um deles tinha objetos de
barbearia, outro tinha colas, outro tinha tintas com pincéis, e assim por diante.

Ballzer entregou um cubo de borracha ao menino, ele gritou algo como "seção 42!"
e passou o cubo ao primeiro robô da mesa, que fez alguma coisa no cubo e o passou 
ao próximo robô, que o cheirou e não fez nada, depois passou adiante.

Do outro lado da mesa havia uma mulher alta, magra e de pele rosada que recebeu o
cubo do último robô, cheirou, abriu, tirou um papel de dentro e leu, depois guardou,
pegou **uma coisa** em sua bolsa e devolveu ao robô, que refez o ritual às avessas,
passando a coisa para o próximo robô e assim até chegar ao **Ballzer**, que voltou
em velocidade para entregar a coisa a um senhor do outro lado da rua, próximo
do muro.

Alatazan ouviu o senhor dizer:

- Muito bem **Ballzer**, muito bem.

Isso se repetiu por mais vezes. Às vezes o senhor sorria, às vezes
não. Pegava outro cubo, entregava ao Ballzer e assim se seguia...

Eles estavam jogando **Bhallz**, um complexo jogo muito praticado nas escolas
para adolescentes.

## Voltando ao Django...

Na web, as coisas são parecidas.

Imagine que o senhor do outro lado da rua é o **seu usuário**, usando o navegador.

O menino é o **URL dispatcher**.

Os robôs à mesa são **middlewares**.

A moça rosada, ao final da mesa, é uma **view**.

O cubo é uma **requisição** (ou **HttpRequest**).

A coisa que a moça devolveu aos robôs é uma **resposta** (ou **HttpResponse**).

E por fim, o maluco e imprevisível **Ballzer**, é a própria **Internet**.

Ou seja, cada qual à sua vez, fazendo seu papel na linha de produção, para **receber um pedido do usuário e retornar uma resposta equivalente**, que na maioria das vezes, é uma página bonita e colorida.

### Entendendo a requisição

Na requisição (ou **HttpRequest**) há diversas informações. Uma das informações é a URL, que é composta pelo protocolo, o domínio, a porta (às vezes), o caminho e os parâmetros (às vezes também).

Exemplo:

    http://localhost:8000/admin/?nome=Mychell&idade=15
    

- Protocolo: **http**
- Domínio: **localhost**
- Porta: **8000**
- Caminho: **/admin/**
- Parâmetros: **nome = Mychell** e **idade = 15**

Quando a porta não é citada, então você deve entender que se trata da **porta 80**, a porta padrão do protocolo **HTTP**.

Outras informações que se encontram na requisição são aquelas sobre o navegador e computador do usuário, como seu **IP**, **sistema operacional**, **idiomas suportados**, **cookies** e diversas outras coisas.

### O Handler

A primeira coisa que acontece quando a requisição chega ao Django, está no handler. Essa parte **nunca é vista ou notada por você**, mas é o primeiro passo antes da requisição chegar aos middlewares, e também é o último passo antes da resposta voltar do Django para o usuário final.

### Middlewares

Middlewares são pequenos trechos de código que analisam **a requisição na entrada e a resposta na saída**. A requisição é analisada por eles.

**Você pode determinar quais middlewares estarão na fila à mesa para analisar a requisição e pode até criar os seus próprios middlewares.**

Um dos middlewares faz com que toda a **segurança e autenticação de usuários** seja feita. Outro adiciona a função de **sessão**, uma forma de memorizar o que o usuário está fazendo para atendê-lo melhor. Há ainda outro middleware, que memoriza as respostas no **Cache**, pois se caso a próxima requisição tenha o mesmo **caminho** e os mesmos **parâmetros**, ele retorna a resposta memorizada, evitando o trabalho de ser processada novamente pela **view**.

Sendo assim, após passar por todos esses middlewares, a requisição passa a ter outras informações, como **qual usuário está autenticado**, **sessão atual** e outras coisas.

### O URL Dispather

Depois de passar pelos middlewares, a requisição é analisada pelo URL Dispatcher, um importante componente do Django que verifica o **endereço** - especialmente a parte do **caminho** - e verifica o arquivo **urls.py** do projeto para apontar qual **view** será chamada para dar a resposta.

### Ah, a view

A view é uma função escrita em Python, e na maioria das vezes, escrita por você.

Ela faz isso: recebe uma requisição (**HttpRequest**) e retorna uma resposta (**HttpResponse**).

É aqui que entra seu trabalho diário, que é analisar uma requisição, fazer algo no **banco de dados** e retornar uma página.

O Django possui algumas *views* prontas para coisas que sempre funcionam do mesmo jeito. Elas são chamadas **Generic Views** e estão espalhadas por toda parte. Há *generic views* úteis para blogs, para cadastros em geral, para lembrar senha, autenticar, sair do sistema, redirecionar, enfim, há muitas delas.

**Você não é obrigado a usar generic views. Elas estão lá para você usá-las se assim o desejar.**

As views escritas por você devem ser escritas em um arquivo chamado **views.py**.

### Os arquivos de templates

As páginas no Django são em geral guardadas em **Templates**, que são arquivos HTML (ou outro formato) que possuem sua própria lógica em interpretar àquilo que lhes é passado e no final renderizar um HTML, que é passado como resposta ao usuário. No entanto, esta não é uma regra, pois muitas vezes não será retornado um HTML como resposta. Mas isso será muito mais esclarecido nos próximos capítulos.

### A camada de Modelo

Para armazenar e resgatar informações do **banco de dados**, não é necessário ir até ele e conhecer a linguagem dele (o bom, velho, e cada vez mais sumido **SQL**). Você usa uma ferramenta do Django chamada **ORM**, que interpreta o seu código, leva aquilo até o banco de dados, e depois devolve as informações desejadas.

A parte do código onde você configura quais são seus modelos de dados e que tipo de informações eles devem armazenar, é um arquivo chamado **models.py**.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/fluxo-no-mvc/file/"/>
</p>

### Então, tudo se resume em MVC

**MVC** é a sigla que resume tudo isso:

- Modelo (Model)
- Visão (View)
- Controle (Controller)

**Modelo** é onde estão as definições dos dados, como eles devem ser armazenados e tratados. É lá que você diz quais campos uma tabela deve ter, seus tipos e valores padrão e se eles são obrigatórios ou não. Dentre outras coisas.

**Visão** são as funções que **recebem requisições e retornam respostas**, ao usuário, a outro computador, a uma impressora ou qualquer outra coisa externa. Ou seja, as views.

E **Controle** são todas as coisas que ficam no meio do caminho, como o **handler**, os **middlewares** e o **URL dispatcher**.  A maior parte dessas coisas é feita pelo próprio Django, e você deve se preocupar pouco ou nada com isso.

E onde entram o **banco de dados** e os **templates**?

O banco de dados é a camada de **persistência**, e não faz parte do MVC, pois não faz parte do Django em si.

Os templates são arquivos utilizados pelas views, são apenas auxiliares, como uma bolsa utilitária ao lado, que você usa se quiser ou se precisar.

## Passando adiante...

- Fascinante tudo isso, não? Meu pai conta que nas fábricas de software mais antigas trabalhavam com outro conceito, chamado **Cliente/Servidor**, mas isso nunca caiu bem na Web, era muito lento...

Os olhos de Nena até brilhavam. Ela adorava essas coisas de conceitos e arquitetura.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_1.gif" align="right"/>

- Bom, não é muito diferente de jogar **Bhallz**. Cada pedacinho tem um papel simples a cumprir. Complicado é pegar a coisa toda de uma vez, mas se você jogar numa posição de cada vez, vai ver que é fácil de jogar... - respondeu um Alatazan mais aliviado.

Cartola estava um pouco disperso, entretido com seu *gadget*. Ele o havia configurado para receber os podcasts do [**This Week In Django**](http://thisweekindjango.com/) sempre que houvesse novidades, e havia acabado de chegar um episódio novo.

No próximo capítulo, vamos colocar o **RSS** para funcionar no Blog, e entender como é fácil levar suas novidades às pessoas sem que elas precisem vir até você.

**Próximo capítulo: [O RSS é o entregador fiel](http://www.aprendendodjango.com/o-rss-e-o-entregador-fiel/)**

