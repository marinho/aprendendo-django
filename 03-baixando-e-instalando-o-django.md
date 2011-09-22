# DESCRICAO CURTA

<img src="http://www.aprendendodjango.com/media/img/ilustracao_2.gif" alt="" align="left"/>

Depois de tomar um suco de tangerina, Alatazan faz o download e instala o **Python**, o **Django**, a biblioteca do banco de dados **SQLite** e fica tranquilo ao saber que logo vai fazer seu primeiro projeto.

Ele ainda não sabe exatamente o que fazer, mas idéias não faltam:

- um sistema de controle para suas músicas intergalácticas
- um site de publicação de fotos de unhas retorcidas
- um meio de se comunicar com sua mãe, que deve estar uma arara com ele

Mas isso ele vai ver depois, agora é hora de deixar a máquina funcionando para seu ambiente de criação.

# TEXTO

Quando Alatazan chegou à Terra, sua pequena nave foi enviada da nave maior, e ao chegar, chocou-se ao chão, meio desajeitada, e pareceu acertar alguém que passeava com seu cachorrinho.

Alatazan saiu de dentro um pouco desajeitado e uma velhinha lhe ofereceu um suco.

Ele adorou aquilo, e agora, na casa de Cartola, ele tomava um suco de tangerina pra refrescar um pouco antes de retomar à descoberta do Django.

Do nada, ele soltou essa:

- Quem inventou o suco?

- Sei lá, alguém que estava sentindo calor, há muito tempo atrás...

- Quem foi eu não sei, mas ainda bem que ele contou a receita pra alguém, senão teria ido dessa pra melhor com sua idéia guardada e deixando um monte de gente fadada ao cafezinho.

- A fruta é de preço baixo, fácil de comprar, a receita é livre para qualquer um, é só ter um pouquinho de tempo, água, liquidificador, açúcar, essas coisas... bom demais! Vou levar isso pra Katara e vou ficar rico fazendo sucos...

<img src="http://www.aprendendodjango.com/media/img/ilustracao_2.gif" alt="" align="right"/>

E depois dessas divagações, eles voltaram ao computador.

- Como é que se consegue o Django? - Alatazan já foi logo indagando ao Cartola...

## Como conseguir o Django

O Django é **software livre**. O Python também é **software livre**.

**Software livre** é todo software que sua a receita é livre, pública, acessível para qualquer um. É como se você for a um restaurante e quiser ver como é a cozinha pra ter certeza de que não estão lhe oferecendo gato por frango. É como se você quiser um dia fazer diferente e misturar suco de limão com abacate... parece estranho, mas você pode fazer isso se quiser (e é estranho que algumas pessoas realmente gostem).

Os caras que fizeram o Django acreditam que todo software (ou pelo menos a maioria deles) deveria ser livre. Eles ganham dinheiro usando software livre, mas não impedem que outras pessoas também o usem e ganhem dinheiro da mesma forma, ou até mais.

Mas se o software que você faz será livre ou não, isso também é um direito seu.

Então ficou fácil de resolver. Você pode baixar o Django e não pagar nada por isso. Usá-lo e não pagar nada por isso. E pode vender o seu trabalho com ele, e não repassar nada a eles por isso. Só é preciso que escreva em algum lugar que seu software foi escrito usando Django.

O mesmo vale para o Python e para as bibliotecas adicionais que o Django utiliza.

## Baixando e instalando

Há várias formas de se instalar o Django. Algumas muito fáceis, outras muito educativas. Nós vamos seguir pelo caminho que deve ser o mais difícil deles. Isso é porque é importante passar por essas etapas para abrir sua mente ao universo Python e Django, mas caso queira o caminho mais prático, vá direto ao final deste capítulo e veja como fazer.

### No Windows

Bom, então vamos começar pelo **Python**.

Se você usa **Windows**, vá até a página abaixo e faça o download do executável de instalação (algo como **"Windows installer"**) mais próximo à sua realidade (**recomendo a versão 2.5.2**):

    http://www.python.org/download/

Após baixar o arquivo, clique duas vezes sobre ele e clique no botão **"Next"**.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/instalacao-do-python/file/" alt="Instalando o Python no Windows"/>
</p>

Na próxima página, a imagem abaixo será exibida, guarde o caminho indicado no campo (no caso da imagem abaixo, é **"C:\Python25\"**, pois vamos usá-lo logo após a instalação do Python. Depois dela, siga clicando no botão **"Next"** até finalizar.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/instalacao-do-python-2/file/" alt="Caminho de instalação"/>
</p>

Por fim, é importante fazer isso: o Windows não consegue encontrar sozinho o Python, portanto, é preciso dizer a ele onde o Python está. Isso se faz adicionando o caminho de instalação do Python à variável de ambiente **PATH**, do Windows.

Pressione as teclas **Windows + Pause** (a tecla Windows é a tecla do logotipo do Windows).

Na janela aberta, clique na aba **"Avançado"** e depois disso, no botão *Variáveis de ambiente*:



<p align="center">
<img src="http://www.aprendendodjango.com/gallery/variaveis-de-ambiente/file/" alt="Janela de Propriedades do Sistema"/>
</p>

Na caixa de seleção **"Variáveis do Sistema"** clique duas vezes sobre o item **"Path"**.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/variaveis-do-sistema/file/" alt="Janela de Propriedades do Sistema"/>
</p>

E ao final do campo **"Valor da Variável"**, adicione um ponto-e-vírgula e o caminho que você memorizou da instalação do Python, como está destacado abaixo.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/variavel-path/file/" alt="Editando variável path"/>
</p>

Pronto! O Python está pronto para uso. Agora vamos ao **Django**!

Na página a seguir você encontrará alguns meios para instalar o Django.

    http://www.djangoproject.com/download/
    

Vamos instalar o **tarball** da versão 1.0. Tarball é um arquivo compactado que termina com **".tar.gz"** e precisa de um programa de descompressão para ser aberto.

Para o **Windows**, o **7-Zip** é um software (também livre) que faz isso pra você. Faça o download da seguinte página e instale em sua máquina:

    http://www.7-zip.org/
    

Voltando ao Django, localize **"Django-1.0.tar.gz"** na página de Download do Django citada acima. Faça o download do arquivo e descompacte onde bem quiser.

Dentro da pasta criada - que provavelmente vai se chamar **"Django-1.0"**) você vai criar um novo arquivo, chamado **"instalar.bat"**, edite usando o **Bloco de Notas** e escreva dentro o seguinte código:

    python setup.py install
    pause

Feche, salve, e clique duas vezes para executar.

Feito isso, o Django estará instalado. Agora vamos seguir para o último passo: **instalar a biblioteca do banco de dados SQLite**.

O SQLite é um banco de dados simples, sem muitos recursos. É ideal para o ambiente de criação e desenvolvimento. Não dá pra fazer muitas coisas com ele, mas você dificilmente vai precisar de fazer muitas coisas enquanto cria seu site. Para o Django trabalhar em conjunto com o SQLite, é preciso apenas instalar uma biblioteca, chamada **pySQLite**, e nada mais.

Então, vá até a página seguinte e faça o download do **tarball** (código-fonte, com extensão .tar.gz).

    http://oss.itsystementwicklung.de/trac/pysqlite/#Downloads
    

Se a instalação do pySQLite apresentar **erros de compilação** no Windows, tente instalar com um dos instaladores executáveis disponíveis na página de download acima. Isso às vezes ocorre por consequência de algum tipo de incompatibilidade entre compiladores.

Após o download, faça o mesmo que fez com o Django:

1. descompacte usando o 7-zip
1. crie o arquivo **instalar.bat** com aquele mesmo código dentro
1. execute
1. pronto.

Pois bem, agora você tem um ambiente de desenvolvimento instalado para começar a usar.

### No Linux, Mac e outros sistemas

Se você usa **Linux** ou **Mac**, os passos não serão diferentes dos seguidos para o Windows.

No site do Python são disponíveis os arquivos para download e instalação para esses sistemas. No caso do Django e pySQLite o processo é basicamente o mesmo, com as devidas diferenças comuns entre os sistemas operacionais.

Normalmente o Linux já vem com o Python e o pySQLite instalados.

### Existe um caminho mais fácil?

Sim, existe um caminho bem mais fácil. Como já foi dito lá em cima, o processo acima é mais difícil, mas, o melhor para o seu aprendizado.

Se quiser instalar de forma mais fácil, use o **DjangoStack**. Veja neste site como fazê-lo:

    http://www.marinhobrandao.com/blog/djangostack-facil-para-o-iniciante-aprender-django/
    

Alatazan respirou aliviado. Apesar da sopa de letrinhas, a coisa não parecia tão complicada.

- então tudo se resume a instalar 3 coisas: o Python, o Django e o pySQLite.

<img src="http://www.aprendendodjango.com/media/img/ilustracao_3.gif" alt="" align="right"/>

- que são feitos de forma que os princípios do software livre sejam preservados: usando outros softwares livres - completou Nena

- e então, o que vamos fazer amanhã?

- vamos criar o seu primeiro projeto. Você já tem alguma idéia do que quer fazer?

- sim, quero dar um jeito de mandar mensagens para a minha família, mostrar como tem sido a minha vida por aqui, o que tenho descoberto, algumas fotos dos tênis da Rua 9...

- então o que você quer, é um **blog**. Vamos fazer isso então!

**Próximo capítulo: [Criando um Blog maneiro](http://www.aprendendodjango.com/criando-um-blog-maneiro/)**


