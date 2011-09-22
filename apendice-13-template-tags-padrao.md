### autoescape

Habilita ( **on** ) ou desabilita ( **off** ) o AutoEscaping em um bloco do template.

### block

Determina um bloco no template que pode ser sobreposto por outros templates.

### comment

Determina que um bloco do template deve ser ignorado no momento da renderização.

### cycle

Dentro de um laço **{% for %}**, alterna valores. Pode também ser usado fora do laço para atribuir uma lista de valores a uma nova variável e depois alternar os valores da variáveis a cada vez que é chamada. É muito útil para fazer tabelas com linhas de cores alternadas e outras coisas assim.

### debug

Imprime no HTML uma caixa com diveras informações úteis para **debug (depuração).

### extends

Determina qual template externo este template extenda, ou seja, de qual template ele herda suas informações básicas. Deve ser sempre informado na primeira linha do arquivo de template. É o elemento principal para o recurso de **herança de templates** do Django.

### filter

Aplica um ou mais **Template Filters** a um bloco do template.

### firstof

Renderiza o primeiro item válido (de valor **verdadeiro**) de uma lista.

### for

Aplica um laço de repetição a um bloco. O bloco será repetido uma vez para cada item da lista informada a esta *template tag*.

### empty

Dentro de um laço **{% for %}**, aplica um bloco caso a lista do laço esteja vazia.

### if

Aplica uma condição para que seu bloco seja renderizado no template. A condição suporta operadores **"and"**, **"or"** e **"not"**, mas somente com valores lógicos.

### ifchanged

Determina que seu bloco no template será renderizado somente se a variável informada na template tag sofrer alteração. Útil para a construção de agrupamentos e relatórios.

### ifequal

Determina que seu bloco no template será renderizado somente se as variáveis ou valores informados forem iguais.

### ifnotequal

Semelhante à template tag **{% ifequal %}** mas faz exatamente o **contrário**.

### include

Inclui outro template naquele lugar de seu template. É o elemento principal para o recurso de **composição de templates** do Django.

### load

Carrega um módulo de da pasta **templatetags** de alguma das aplicações do projeto. Esses módulos normalmente possuem **template tags** e **template filters** adicionais às que vêm no contexto padrão do Django.

### now

Renderiza a data/hora atual no formato informado.

### regroup

Cria uma nova variável com o agrupamento da lista informada por um de dos atributos de seus itens. Útil para a construção de agrupamentos e relatórios.

### spaceless

Remove todos os espaços vazios de seu bloco.

### ssi

Aplica as funcionalidades de **SSI** no template.

### templatetag

Renderiza um símbolo especial do sistema de templates do Django. Os símbolos são:

<div class="tabela"><table>
 <tr><td>openblock</td><td>{%</td></tr>
 <tr><td>closeblock</td><td>%}</td></tr>
 <tr><td>openvariable</td><td>{{</td></tr>
 <tr><td>closevariable</td><td>}}</td></tr>
 <tr><td>openbrace</td><td>{</td></tr>
 <tr><td>closebrace</td><td>}</td></tr>
 <tr><td>opencomment</td><td>{#</td></tr>
 <tr><td>closecomment</td><td>#}</td></tr>
</table></div>

### url

Renderiza a URL requivalente ao **caminho da view** ou **nome da URL** informado, com seus respectivos argumentos.

### widthratio

Renderiza a equivalência percentual de um número entre dois outros.

### with

Cria um apelido para um valor ou variável dentro de seu bloco do template.

