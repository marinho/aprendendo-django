Widgets são componentes de para entrada de dados em formulários dinâmicos. Um campo de formulário dinâmico tem um tipo e é representado por um widget, que pode ser um dos citados a seguir:

## Widgets padrão

Os widgets padrão do Django são encontrados no pacote **"django.forms"** e são os mais populares e mais usados.

### Widget

Classe básica que é herdada por todos os demais widgets.

### Input

Widget básico para usar a tag HTML **&lt;input&gt;**.

### Textarea

Widget básico para usar a tag HTML **&lt;textarea&gt;**, que permite a entrada de texto livre.

### CheckboxInput

Widget que exibe uma caixa de verificação equivalente à tag HTML **&lt;input type="checkbox"/&gt;**.

### Select

Widget básico para usar a tag HTML **&lt;select&gt;**, que permite a escolha dentre uma lista de opções.

### MultiWidget

Permite a construção de seus próprios widgets compostos de vários outros.

### SplitDateTimeWidget

Widget dividido em dois: um para a entrada de data e outro para a entrada de hora.

### SplitHiddenDateTimeWidget

Semelhante ao **"SplitDateTimeWidget"**, mas não é visível ao usuário.

### TextInput

Widget que exibe um campo para entrada de texto equivalente à tag HTML **&lt;input type="text"/&gt;**.

### PasswordInput

Widget que exibe um campo para entrada de texto equivalente à tag HTML **&lt;input type="password"/&gt;**, que protege a visão para a entrada de senhas.

### HiddenInput

Widget equivalente à tag HTML **&lt;input type="hidden"/&gt;**, para valores ocultos.

### MultipleHiddenInput

Semelhante ao **"HiddenInput"**, mas permite múltiplos valores.

### FileInput

Widget para envio de arquivos, equivalente à tag HTML **&lt;input type="file"/&gt;**. Seu uso necessita que a tag **&lt;form&gt;** em HTML tenha o parâmetro enctype="multipart/form-data". O arquivo enviado pelo usuário é armazenado no dicionário "request.FILES".

### DateTimeInput

Permite ao usuário informar um valor de data/hora.

### TimeInput

Permite ao usuário informar um valor de hora.

### NullBooleanSelect

Exibe um campo de seleção para a escolha entre três valores possíveis: **"Verdadeiro"**, **"Falso"** e **"Nenhum"**.

### SelectMultiple

Permite a múltipla seleção dentre uma lista de opções.

### RadioSelect

Widget que permite ao usuário que escolha uma dentre várias opções em campos de seleção do tipo **"radio"**, equivalente à tag HTML **&lt;input type="radio"/&gt;**.

### CheckboxSelectMultiple

Semelhante ao **"SelectMultiple"**, mas a seleção é feita por caixas de verificação (equivale a múltiplas tags HTML **&lt;input type="checkbox"/&gt;**).

## Widgets extras

Só há um widget extra, localizado no pacote **"django.forms.extra.widgets"**.

### SelectDateWidget

Permite a seleção de uma data de forma amigável, com campos separados para ano, mês e dia.

## Widgets do Admin

Os widgets do Admin foram criados específicos para serem usados na interface de administração, mas podem ser usados em formulários dinâmicos comuns. São encontrados no pacote **"django.contrib.admin.widgets"**.

### FilteredSelectMultiple

Semelhante ao **"SelectMultiple"**, mas possui códigos em JavaScript que permite o filtro das opções de forma amigável.

Todos eles necessitam da declaração do atributo **"media"** do formulário, de preferência no cabeçalho do HTML. Todos eles também necessitam da habilitação do pacote de internacionalização de JavaScript (pacote **"jsi18n"**).

### AdminDateWidget

Exibe um calendário amigável para a seleção da data.

### AdminTimeWidget

Exibe um campo amigável para seleção de hora.

### AdminSplitDateTime

Equivale à soma dos widgets **"AdminDateWidget"** e **"AdminSplitDateTime"**.

### AdminRadioSelect

Semelhante ao **"RadioSelect"**, mas é renderizado em listas não-numeradas.

### AdminFileWidget

Semelhante ao **"FileInput"**, mas exibe o caminho e um link para o arquivo enviado caso haja um valor já informado ali.

### ForeignKeyRawIdWidget

Permite a seleção de um objeto relacionado usando apenas seu código. É útil para campos de seleção onde existem centenas ou milhares de opções, o que torna imposível um campo de seleção comum.

### ManyToManyRawIdWidget

Semelhante ao **"ForeignKeyRawIdWidget"**, mas permite múltipla seleção.

### RelatedFieldWidgetWrapper

Usado para exibir um ícone para adicionar um novo objeto ao lado de um widget do tipo **"ForeignKeyRawIdWidget"**.

### AdminTextareaWidget

Semelhante ao **"Textarea"**, mas acrescenta uma classe CSS a mais para direcionar seu comportamento em JavaScript.

### AdminTextInputWidget

Semelhante ao **"TextInput"**, mas acrescenta uma classe CSS a mais para direcionar seu comportamento em JavaScript.

### AdminURLFieldWidget

Semelhante ao **"TextInput"**, mas acrescenta uma classe CSS a mais para direcionar seu comportamento em JavaScript (para valores de URLs).

### AdminIntegerFieldWidget

Semelhante ao **"TextInput"**, mas acrescenta uma classe CSS a mais para direcionar seu comportamento em JavaScript (para números inteiros).

### AdminCommaSeparatedIntegerFieldWidget

Semelhante ao **"TextInput"**, mas acrescenta uma classe CSS a mais para direcionar seu comportamento em JavaScript (para números inteiros separados por vírgula).

