### add

Retorna a soma do valor filtrado com o valor informado como parâmetro.

### addslashes

Retorna a string filtrada com barras adicionadas antes de caracteres de aspas.

### capfirst

Retorna a string filtrada com a primeira letra em maíuscula.

### center

Retorna a string filtrada posicionada **no centro** para a largura informada. Isso quer dizer que se uma string **'Livro'** for filtrada para a largura **10**, será retornada uma string **'  Livro   '**.

### cut

Retorna a string filtrada removendo as instâncias do caracter informado como parâmetro.

### date

Retorna a data/hora filtrada na formatação informada como parâmetro. Segue a sintaxe do **apêndice 4**.

### default

Retorna o valor informado como parâmetro se a string filtrada não for valor verdadeiro.

### default_if_none

Retorna o valor informado como parâmetro se a string filtrada for **None**.

### dictsort

Retorna uma lista de dicionários filtrada, ordenada por uma das chaves dos dicionários.

### dictsortreversed

Semelhante ao **dictsort**, mas retorna a ordenação invertida.

### divisibleby

Retorna **True** se o valor filtrado for divisível pelo valor informado como parâmetro. Retorna **False** para o contrário.

### escape

Retorna o *escaping* de caracteres especiais para HTML *entities* da string filtrada. Isso quer dizer que uma string com tags HTML em seu conteúdo será retornada de forma que tal HTML não será renderizado na tela, exibindo o código original. É importante para demonstrações de código e como técnica de segurança. A string '&lt;b&gt;' por exemplo é retornada como u'&amp;lt;b&amp;gt;'.

Caso esteja em uma situação *default* (quando o **auto-escaping** permanece ativado), esta template filter retorna exatamente o valor filtrado sem aplicar suas substituições.

### escapejs

Semelhante ao **escape**. Entretanto, o *escaping* é feito para linguagem **JavaScript** ao invés de HTML. Uma string "alert('a')" por exemplo é retornada como u'alert(\\\\x27a\\\\x27)'.

### filesizeformat

Retorna a formatação amigável com unidades de medida baseadas em **byte** (kilobytes, megabytes, gigabytes, etc.) para o valor filtrado.

### first

Retorna o primeiro item da lista filtrada ou primeiro caracter da string filtrada.

### fix_ampersands

Retorna a string filtrada, substituindo caracteres **'&'** por **'&amp;amp;'**;

### floatformat

Retorna o valor filtrado com formatação de número flutuante para o número de casas decimais informado como parâmetro.

### force_escape

Semelhante ao **escape**. Entretanto, ignora o estado de **auto-escape** e efetua as susbtituições necessária dos caracteres.

### get_digit

Retorna o algarismo na posição informada como parâmetro (da direita para a esquerda), de um valor filtrado. Isso quer dizer que para um valor filtrado **123456** com parâmetro **2**, o retorno será **5**.

### iriencode

Retorna a string filtrada, convertida para o formato **IRI** (Internationalized Resource Identifier). Isso quer dizer que uma URL terá seus caracteres especiais convertidos para códigos de forma que a URL esteja no padrão IRI. Uma string filtrada 'com espaço' retorna u'com%20espa%C3%A7o';

### join

Retorna a lista filtrada como string, fazendo uma junção de seus itens com o caracter informado como parâmetro. Tem o mesmo comportamento do método **''.join()** do Python

### last

Retorna o último item de uma lista filtrada ou o último caracter de uma string filtrada.

### length

Retorna o tamanho da string ou lista filtrada.

### length_is

Retorna **True** se o tamanho da string ou lista filtrada for igual ao parâmetro informado. Retorna **False** para o contrário.

### linebreaks

Retorna a string filtrada substituindo saltos de linha por tags HTML '&lt;br/&gt;' e acrescentando toda a string em um parâgrafo em HTML '&lt;p&gt;&lt;/p&gt;'.

### linebreaksbr

Semelhante ao **linebreaks**. Entretanto não retorna a string em um parágrafo HTML.

### linenumbers

Retorna a string filtrada acrescentando numeração para suas linhas.

### ljust

Retorna a string filtrada posicionada **à esquerda** para a largura informada. Isso quer dizer que se uma string **'Livro'** for filtrada para a largura **10**, será retornada uma string **'Livro     '**.

### lower

Retorna a string filtrada convertando letras maiúsculas em **minúsculas**.

### make_list

Retorna uma lista com os caracteres do valor filtrado convertidos para items dessa lista.

### phone2numeric

Retorna a string filtrada convertendo letras para seus respectivos números segundo os teclados de telefones. Isso quer dizer que para uma string filtrada **'800-GOOG'** será retornada uma string **'800-4664'**.

### pluralize

Retorna a letra **'s'** se o valor filtrado for **maior que 1**.

### pprint

Tem o mesmo comportamento de **pprint.pprint**. Para efeitos de **depuração**.

### random

Retorna um item aleatório da lista filtrada.

### removetags

Retorna a string filtrada, removendo as tags HTML informadas como parâmetros.

### rjust

Retorna a string filtrada posicionada **à direita** para a largura informada. Isso quer dizer que se uma string **'Livro'** for filtrada para a largura **10**, será retornada uma string **'     Livro'**.

### safe

Retorna a string filtrada, garantindo que seu conteúdo não será convertido pelo **auto-escaping**.

### slice

Retorna parte da string filtrada, cortando na posição e largura informadas como parâmetros.

### slugify

Retorna a string filtrada convertida para formato **slug**. Isso quer dizer que para uma string filtrada **'Borboleta azul é linda'** será retornada a string **u'borboleta-azul-e-linda'**.

### stringformat

Retorna a string filtrada, formatada pelo parâmetro informado. Usa a sintaxe do **apêndice 10**, mas deve-se suprimir os caracteres **"%"**.

### striptags

Retorna a string filtrada, removendo **todas as tags HTML**. Isso quer dizer para a string filtrada **'&lt;b&gt;Teste&lt;/b&gt;'**

### time

Retorna a hora filtrada na formatação informada como parâmetro. Segue a sintaxe do **apêndice 4**.

### timesince

Retorna uma formatação amigável para a data/hora filtrada. Isso quer dizer que para uma data/hora informada **1 hora e 2 minutos antes** da data/hora atual, será retornado **"1 hora, 2 minutos"**, de acordo com o idioma currente do projeto. Se a data/hora informada for futura, será retornado **'0 minutos'**.

### timeuntil

Semelhante ao **timesince**, entretando é adequado para data/horas futuras.

### title

Retorna a string filtrada com a primeira letra de cada palavra em **maiúscula** e o restante em **minúsculas**. Isso quer dizer que a string filtrada **'belo hoRIZONte'** será retornado **u'Belo Horizonte'**.

### truncatewords

Retorna a string filtrada truncada pela quantidade de palavras informada como parâmetro, acrescentado com sinal de **reticências** ( ... ) caso necessário. Isso quer dizer que para a string filtrada **'Teste de template tag'** com quantidade **2** será retornado **u'Teste de ...'**.

### truncatewords_html

Semelhante ao **truncatewords**. Entretanto respeita tags HTML adequadamente.

### unordered_list

Retorna a lista filtrada em formato HTML de **lista não-numerada**, usando tags **&lt;ul&gt;** e **&lt;li&gt;**.

### upper

Retorna a string filtrada convertando letras minúsculas em **maiúsculas**.

### urlencode

Semelhante ao **iriencode**, mas com uma diferença fundamental: é utilizado para codificar strings para serem usadas como valor de um **parâmetro** de URL e não como URLs completas. Isso quer dizer que trechos como "://", "?", "&amp;" e outros serão substituídos por códigos respectivos que permitam que toda a string se o valor de um parâemtro.

### urlize

Retorna a string filtrada com potenciais links tratadas para serem links em tags **&lt;a&gt;** de HTML.

### urlizetrunc

Semelhante ao **urlize**. Entretando os links são **truncados** em seu texto de exibição para evitar links longos. A URL linkada permanece da forma original.

### wordcount

Retorna a quantidade de palavras na string filtrada.

### wordwrap

Retorna a string filtrada, efetuando quebras de linha respeitando as palavras na largura informada como parâmetro.

### yesno

Retorna valores **"sim"**, **"não"** ou **"talvez"** para valor verdadeiros, falsos ou None, dependendo de como os argumentos são passados aos parâmetros.

