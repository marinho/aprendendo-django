A maior parte dos tipos de campos para formulários dinâmicos são equivalentes aos de mesmo nome nas classes de modelo. Dessa forma, um campo do tipo **"CharField"** numa classe de modelo é transposto para um formulário dinâmico com o tipo **"CharField"** do pacote **"django.forms"**.

### Field

Campo genérico para formulário. Todos os demais se extendem deste.

### CharField

Usado para informar _strings_ livres com largura definida.

### IntegerField

Permite somente valores numéricos inteiros.

### FloatField

Permite valores numéricos flutuantes. Usado principalmente para valores onde não se sabe a quantidade de casas decimais definida.

### DecimalField

Permite valores numéricos flutuantes com número máximo de dígitos e de casas decimais. Muito usado para valores monetários e quantidades.

### DateField

Permite a informação de uma data, no formato **"YYYY-MM-DD"**.

### TimeField

Permite a informação de uma hora, no formato **"HH:MM:SS"**.

### DateTimeField

Permite a informação de uma data/hora, no formato **"YYYY-MM-DD HH:MM:SS"**.

### RegexField

Permite qualquer valor, desde que case com uma expressão regular definida.

### EmailField

Permite a informação de um endereço de e-mail.

### FileField

Usado para fazer upload de arquivos. Seu uso necessita que a tag **&lt;form&gt;** em HTML tenha o parâmetro **enctype="multipart/form-data"**. O arquivo enviado pelo usuário é armazenado no dicionário **"request.FILES"**.

### ImageField

É semelhante ao **"FileField"**, com a diferença de que só permite o envio de arquivos de imagens.

### URLField

Permite a informação de um endereço URL, ou seja, endereços de sites e serviços na Web.

### BooleanField

Permite valores lógicos: Verdadeiro ou Falso. Por padrão, exibe uma caixa de verificação para o usuário marcar **"X"**.

### NullBooleanField

Permite valores lógicos mas também permite um valor neutro: **"None"**. Por padrão não exibe uma caixa de verificação, mas sim um caixa de seleção com os três valores possíveis.

### ChoiceField

Permite valores de uma lista de opções. Usado para se escolher entre uma lista de opções, como por exemplo: **"Sexo: Masculino ou Feminino"** ou **"Nível: Iniciante, Intermediário ou Avançado"** e assim por diante.

### TypedChoiceField

Semelhante ao **"ChoiceField"**, mas permite definir uma função para forçar um tipo de dado específico.

### MultipleChoiceField

Semelhante ao **"ChoiceField"**, mas permite a seleção de várias opções de uma só vez (múltipla escolha).

### ComboField

Permite a composição de vários outros campos, para validação em conjunto.

### MultiValueField

Permite criar uma combinação de vários valores de tipos diferentes em um único campo. Usado para campos de valores compostos que devem ser informados como partes de um só campo.

### FilePathField

Permite que o usuário escolha um arquivo de um caminho do disco rígido.

### SplitDateTimeField

Campo de valor composto que separa a data e a hora em campos de entrada separados.

### IPAddressField

Permite a informação de endereços de IP.

### SlugField

Permite a informação valores do tipo **slug**. Isso significa que somente caracteres em caixa-baixa (letras minúsculas), números e sinal de traço são permitidos.

