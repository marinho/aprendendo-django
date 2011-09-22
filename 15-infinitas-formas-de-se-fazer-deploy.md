Próximo a onde Alatazan viveu sua infância há uma cidade chamada **Konnibagga**, que foi construída dentro de uma ilha, **no meio de um lago**.

Devido à sua população ser extremamente **dinâmica**, através dos séculos **dezenas de pontes** foram construídas para atravessar o lago **até a ilha**. Haviam pontes compridas, pontes curtas, pontes gratuitas, pontes com pedágio, pontes bonitas e feias, resistentes e frágeis, largas e estreitas.

Um dia um homem sugeriu que se fossem atravessadas todas as pontes em sequência, entrando e saindo da cidade, seria possível desenhar um girassol com o traçado do percurso. Acontece que em Katara não existem girassóis e ninguém compreendeu qual era o fundamento para tal sugestão.

O homem foi então contratado pela prefeitura para construir mais pontes por um lucro maior desde que parasse de falar nessa idéia, e assim ele realmente parou com isso, construiu muitas pontes e com o dinheiro fez uma viagem transatlântica - apesar de lá também não haver um Atlântico para atravessar.

## As muitas maneiras de se colocar um site no ar

Há diversos caminhos - talvez infinitos - para se colocar um site no ar. Porque para isso é necessário combinar uma série de passos que todos eles possuem diversas alternativas.

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/componentes-do-deploy-basico/file/"/>
</p>

Na ilustração acima estão dispostos os elementos de um deploy básico. Para cada um desses passos há alternativas variadas para escolher, dependendo da situação e preferência dos desenvolvedores e serviço de hospedagem.

As caixas em verde (**Software de Repositório e Controle de Versões**, **FTP** e **Fabric**) são caminhos os quais você vai usar para levar os arquivos do projeto para o servidor. Cada um deles se adequa a uma realidade diferente.

As caixas em rosa (**Software servidor web**, **Cache** e **Banco de Dados**), são softwares que normalmente são instalados pelo serviço de hospedagem em seu servidor, portanto, caso contrate um serviço de hospedagem é muito provável que esses softwares já vêm instalados e configurados.

As caixas em laranja (**Interface com o Python/Django** e **Python, Django e bibliotecas adicionais**) são softwares que (exceto pelo Python em servidores Linux) normalmente não vêm instalados nos servidores e provavelmente terão de ser instalados e configurados por você mesmo.

E as caixas em azul (**Arquivos estáticos** e **Projeto do site**) são os arquivos de seu projeto. Portanto, pelo menos esses arquivos você terá de levar para o servidor para ter seu site no ar.

### Ambiente de desenvolvimento

Seu ambiente de desenvolvimento (ou de sua equipe) deve estar preparado para fazer testes e desenvolvimento local e uma ferramenta para enviar seus arquivos para o servidor, que pode ser de um dos 3 tipos a seguir.

### Software de Repositório  e Controle de Versões

Quando se trabalha com o projeto em apenas um servidor, especialmente quando há mais de uma pessoa envolvida no desenvolvimento do site, é recomendado que se use uma ferramenta de **repositório e controle de versões**.

Os exemplos mais populares desse tipo de software são:

* Git
* Subversion
* Bazaar
* Mercurial

A recomendação para a maioria dos casos é o **Git**, atualmente a ferramenta com maior flexibilidade e performance dentre as conhecidas no mercado. E além disso, é livre e gratuita.

### FTP

O uso do FTP tem se tornado cada vez menos recorrente devido à falta de controle e dificuldade para se trabalhar em equipe. Ainda assim, é a ferramenta mais simples e presente nos servidores, conhecida pela maior parte dos desenvolvedores.

O fato é que de uma ou outra forma, nunca se deixa de usar o FTP, mas seu uso passa a ser para situações de casos isolados ou emergenciais.

### Fabric

Por último, a mais poderosa opção para fazer deploy de projetos em Django se chama **Fabric**. Você pode conseguir mais detalhes sobre ele em seu site oficial:

    http://www.nongnu.org/fab/
    

Seu uso se resume a construir um pequeno script, chamado de **fabfile**, que determina as regras para se atualizar servidores em grande número, realizando algumas tarefas relacionadas a isso. Não se trata de uma opção vantajosa para a maioria do sites que estão presentes em somente um servidor, mas para ambientes mais complexos se trata de uma grande vantagem.

### Software servidor web

Os servidores web se tratam de softwares que *escutam* por uma porta **HTTP** ou **HTTPS** (geralmente portas **80** e **443**, respectivamente) e servem aos navegadores com arquivos e conteúdo dinâmico. Trata-se de um dos elementos mais fundamentais neste processo. É impossível evitar o uso de algum tipo de servidor.

Mas há diversos tipos, versões e formas de configurá-los. Os mais populares são:

* Apache
* Nginx
* Lighttpd
* Microsoft IIS

Normalmente as diferenças mais comuns que se atribui entre esses servidores são:

* **Apache** é o mais robusto, mais popular e mais compatível deles, especialmente quando combinado com outros softwares livres.

* **Nginx** é relativamente novo, é muito leve e rápido e trabalha com um cache próprio de conteúdo. No entanto é escasso de documentação.

* **Lighttpd** é excelente para servir arquivos estáticos e FastCGI, possui uma forma de configuração bastante flexível e é também muito leve.

* **Microsoft IIS** é o mais compatível com tecnologias da linha Microsoft. Entretanto é o mais limitado quando se refere à combinação com softwares livres, possui uma estrutura fechada e diversos problemas de segurança.

Cada um deles possui uma forma diferente de se configurar para trabalhar com o **Django**. No próximo capítulo vamos trabalhar a configuração do Apache, o mais popular e comum deles.

### Banco de Dados

Existem diversos bancos de dados. O Django é compatível oficialmente com 5:

* MySQL
* PostgreSQL
* SQLite
* Oracle
* Microsoft SQL Server

Os primeiros três são livres, sendo que o MySQL e PostgreSQL são mais indicados para servidores em produção, pois são poderosos e possuem uma performance relativamente superior aos demais. Além disso são plenamente compatíveis com outros softwares livres.

O SQLite é mais indicado para ambientes de desenvolvimento, por ser muito simples e leve, mas não possui uma série de recursos desejáveis para servidores.

Já o Oracle e MS SQL Server são softwares proprietários, que dispõem de versões gratuitas mas limitadas. Também são menos compatíveis com softwares livres.

No próximo capítulo vamos trabalhar na configuração de um servidor com MySQL.

### Cache

O serviço de Cache em um servidor pode variar muito. Muitos servidores já são oferecidos por empresas de hospedagem com um software de cache, como os seguintes:

* Squid
* Varnish
* memcached

Ainda que figurem como ferramentas para cache, os três softwares citados acima possuem características e aplicações diferentes.

Há ainda outras formas de fazer cache no Django, usando **banco de dados** ou uma **pasta do HD**. Mas vamos trabalhar a instalação do próximo capítulo com **memcached**, a mais indicada para esse caso.

### Interface com o Python/Django

Para que o servidor web se comunique com o seu projeto em Django, é preciso ter uma interface, que possibilita ao servidor o **reconhecimento** de software escrito em Python, que é o caso de projetos em Django.

As formas mais comuns de se fazer isso são:

* mod\_python
* WSGI
* FastCGI

Cada um deles possui um método de trabalho diferente, e sua aplicação varia de acordo com o caso.

A maior parte dos serviços de hospedagem **compartilhada** (quando um mesmo servidor é compartilhado por vários clientes) é feita usando **FastCGI**, mas o crescimento do WSGI nesse nicho tem sido uma constante. Neste caso, usa-se o módulo **mod\_rewrite** do Apache ou funcionalidades semelhantes em outros servidores web.

Já a maior parte dos **servidores dedicados** e **servidores virtualizados** são trabalhados usando **mod\_python**, mas da mesma forma, o crescimento do **WSGI** tem substituído o mod\_python pouco a pouco.

Isso se deve à facilidade e flexibilidade que o WSGI oferece e ao ganho de performance que também oferece na maior parte dos casos.

### Python, Django e bibliotecas adicionais


Na maioria das distribuições **Linux**, o **Python** é instalado como parte oficial do sistema operacional. Mas em outros casos é preciso ser feita sua instalação.

É necessário também **instalar o Django**. Não é necessário que ele seja instalado entre os pacotes oficiais do Python, pois pode simplesmente ser colocado na mesma pasta do projeto, mas ainda assim, ele é **necessário**.

É preciso também instalar a **biblioteca de acesso ao banco de dados**. É ela quem faz a ponte entre o Python e o seu banco de dados escolhido.

As bibliotecas de acesso a bancos de dados são estas:

* MySQLdb (para acesso a MySQL)
* pySQLite (para acesso a SQLite)
* cx_Oracle (para acesso a Oracle)
* psycopg (para acesso a PostreSQL)
* psycopg2 (para acesso a PostreSQL)
* PyWin32 (para acesso a Microsoft SQL Server)

### Arquivos estáticos

Os arquivos estáticos são arquivos de **imagens, CSS, JavaScript, Flash Shockwaves** e outros. Não é comum lançar um site sem alguns deles.

Quando se faz o _deploy_ de um projeto, é preciso configurar o servidor web para publicar seus arquivos estáticos de forma independente do Django. O Django não foi construído para servir arquivos estáticos e não vale a pena perder performance com isso.

Outra recomendação importante é a **separação** do servidor web do **projeto** do servidor web de arquivos estáticos. Não é necessária a separação entre máquinas diferentes, o que recomendamos é que as configurações dos servidores web sejam independentes uma da outra.

A cada vez que uma nova pessoa entra em seu site, uma máquina virtual Python é instanciada para ela, e isso geralmente custa em torno de 15Mb a 30Mb de memória. Arquivos estáticos não precisam nem de um décimo disso. Se o seu servidor web for configurado para abrir instâncias com pouca memória, sua máquina virtual Python será prejudicada, e caso seja configurado para ocupar muita memória, os arquivos estáticos serão os prejudicados.

Portanto, sempre que possível, crie dois domínios separados, assim por exemplo: para um site que possui o seguinte domínio:

    http://blog-do-alatazan.com/
    

Tenha um subdomínio assim para arquivos estáticos:

    http://media.blog-do-alatazan.com/
    

Dessa forma você pode configurar o servidor web de formas diferentes, otimizando cada um de acordo com seu caso. Você pode inclusive separar para softwares servidores web diferentes se preferir. Exemplo: **Apache** para o projeto em Django e **Lighttpd** para arquivos estáticos.

### Projeto do site

O projeto é o coração de seu site. Construído em Django, é a única peça de fato específica para o nosso framework. Isso quer dizer que todo o restante do site pode ser aproveitado da mesma forma para **diferentes linguagens e diferentes frameworks**, mas esta é específica para o Django: **seu projeto** e a **configuração** do servidor web para disponibilizá-lo.

### Árvore de pastas ideal

"Árvore de pastas" é a forma como as pastas de seu projeto estão organizadas. Há infinitas formas de se fazer isso.

Uma boa forma para organizar a pastas de projetos em Django é esta:

<p align="center">
<img src="http://www.aprendendodjango.com/gallery/deploy-arvore-de-pastas/file/"/>
</p>

Esta árvore de arquivos está adequada para o sistema de arquivos do Linux, Unix e MacOSX, que juntos formam a maioria dos servidores web disponíveis em serviços de hospedagem. Entretanto é possível construir uma árvore de pastas idêntica no Windows, exceto pelos links simbólicos. Mas isso é possível de contornar para quem tem prática com servidores nesse sistema operacional.

Observe que dentro da pasta **"django"** há duas pastas:

**pluggables**

Você deve colocar nesta pasta as aplicações plugáveis extras do projeto, que veremos mais detalhadamente em um capítulo adiante.

**Aplicações plugáveis** são recursos extras que não são fornecidos pelo Django, mas por **terceiros**. Há centenas delas na Web, a grande maioria é gratuita e livre. Portanto, esta deve ser a pasta onde essas aplicações plugáveis serão instaladas.

Assim, elas podem ser usadas por vários projetos em um único servidor.

**projects**

Nesta pasta serão instaladas as pastas de projetos. Ou seja, até mesmo se você possuir mais de um site em seu servidor, dessa forma suas pastas continuam organizadas.

**Mas atenção:** ao invés de colocar diretamente a pasta do projeto, crie uma pasta para o site e dentro desta pasta, coloque a do projeto, pois um site é mais do que o projeto. É muito provável que você vá querer documentar seu site, adicionar alguns arquivos de histórico e definições, back-ups e outros, portanto, o projeto não se limita aos arquivos-*fonte* em Django.

Agora, dentro da pasta do site do projeto, está a pasta do projeto em Django ( **meu\_blog** ). Dentro dela, além do que já vimos no decorrer dos capítulos anteriores, há ali uma novidade:

**deploy**

Esta pasta é o lugar ideal para se guardar:

- os scripts de execução do projeto (*scripts* **FastCGI e WSGI**, por exemplo);
- e outros scripts e arquivos relacionados ao _deploy_ no servidor, como por exemplo um arquivo com a lista de **cron jobs**, que são tarefas agendadas para servidores **Linux/Unix/MacOS X**.

No mesmo nível da pasta do projeto há outra pasta aqui:

**dependencies**

Esta pasta deve conter um arquivo chamado **README** onde você irá escrever as dependências de seu projeto.

Exemplos: se seu projeto trabalha somente na versão 1.0.2 do Django, escreva isso nesse arquivo. Se seu projeto depende de alguma ferramenta externa como o **ffmpeg**, **mimms** ou **Image Magick**, escreva também. Caso utilize funcionalidades do campo **ImageField**, você vai precisar da biblioteca **PIL**, portanto, escreva isso.

Sempre de forma explícita, informando a versão utilizada, o porquê e como deixar isso funcionando no projeto. Isso é extremamente importante para que não se perca no futuro.

### Porquê usar o inglês nas pastas do servidor?

O inglês é a linguagem universal da atualidade. Ainda mais na informática.

Um dia qualquer, você e sua equipe podem precisar da ajuda de alguém que não conhece o seu idioma. Então essa pessoa irá fazer buscas no servidor por palavras comuns em língua inglesa.

Num mundo cada dia mais globalizado, isso se torna cada dia mais importante.

## E agora, vamos ou não configurar um servidor?

- Sim, vamos... calma Alatazan, o processo de _deploy_ é realmente abrangente, mas você vai perceber que na prática não é tão complexo quanto parece, pois a maioria dessas ferramentas pode ser instalada com grande facilidade ou muitas vezes já vêm instaladas nos serviços de hospedagem, portanto, atente-se aos conceitos, pois eles são importantes de se conhecer.

- Bom... mas como **resumir** todo esse aprendizado teórico de hoje? - os olhos de Alatazan estavam um pouco **vermelhos e ardiam**, pois é muito chato ficar ouvindo outra pessoa falar e não ver **exemplos na prática**.

- O melhor resumo para hoje são os diagramas, observe-os novamente, e não se limite a isso. Servidores são evoluídos e ajustados **ao longo do tempo**, à medida que o administrador e o site amadurecem. E para cada tipo de servidor ou serviço de hospedagem há tutoriais com explicações na web, aí basta fazer uma busca!

- Ok, mas amanhã vamos configurar um servidor Windows com Apache e MySQL né?

- Sim, vamos fazer isso amanhã. Agora é hora de **relaxar, respirar fundo** e procurar alguma coisa _light_ pra fazer.

<span class="no_print">
**Próximo capítulo: [Preparando um servidor com Windows](/preparando-um-servidor-com-windows/)**
</span>

