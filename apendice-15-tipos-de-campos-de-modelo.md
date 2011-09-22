### AutoField

Campo de número inteiro que faz auto-incremento. Equivalente ao tipo **INTEGER** (exceto pelo Oracle, que é **NUMBER(11)**), mas com recurso de auto-incremento (variável de acordo com o banco de dados)

### BooleanField

Armazena dois possíveis valores: **True** ou **False** (Verdadeiro ou Falso). Dependendo do banco de dados, equivale ao **BOOL** (PostgreSQL, MySQL e SQLite) ou **NUMBER(1)** (Oracle).

### CharField

Campo para valores de string literal, com largura definida. Equivale ao **VARCHAR**.

### CommaSeparatedIntegerField

Campo de valor numérico inteiro, separado por vírgulas. Internamente trabalha com o tipo **VARCHAR** no banco de dados.

### DateField

Campo que armazena uma **data**. Equivale ao **DATE**.

### DateTimeField

Campo que armazena uma **data/hora**. Equivale ao **DATETIME** (SQLite e MySQL) ou ao **TIMESTAMP** (Oracle e PostgreSQL).

### DecimalField

Campo que armazena valores numéricos flutuantes de largura fixa. Equivale ao **DECIMAL** ou ao **NUMERIC** (Oracle)

### EmailField

Campo que armazena uma endereço de e-mail (correio eletrônico). Equivale ao **VARCHAR** de 75 caracteres.

### FileField

Campo que armazena o caminho para um arquivo em disco rígido e faz todo o controle no que diz respeito a ele. O valor que esse campo carrega é um objeto de arquivo que pode ser usado para ler o arquivo e apontar sua URL, dentre outras coisas. Equivale a **VARCHAR** de 100 caracteres.

### FilePathField

Da mesma forma que o **"FileField"**, este campo armazena o caminho para um arquivo no disco rígido, mas não tem o papel de carregar o arquivo em si mas sim estabelecer regras de formato e máscara do nome do arquivo. Equivale a **VARCHAR** de 100 caracteres.

### FloatField

Campo que armazena valores numéricos flutuantes. Equivale ao **FLOAT**.

### ImageField

Este tipo de campo é muito semelhante ao **"FileField"**, mas acrescente algumas características específicas de imagens. Depende da biblioteca **PIL - Python Imaging Library** para funcionar. Equivale a **VARCHAR** de 100 caracteres.

### IntegerField

Campo que armazena valores numéricos inteiros. Sua capacidade varia de acordo com o banco de dados, mas em geral suporta até valores na casa de bilhões. Equivale ao **INTEGER**.

### IPAddressField

Campo que armazena números de IP. Ele não permite ser preenchido com valores que não sejam IPs válidos (**quatro** números de 0 a 255, separados por sinais de ponto). Equivale a **VARCHAR** de 15 caracteres.

### NullBooleanField

É semelhante ao **"BooleanField"**, mas este armazena permite três valores: **None**, **True** ou **False**. Dependendo do banco de dados, equivale ao **BOOL** (PostgreSQL, MySQL e SQLite) ou **NUMBER(1)** (Oracle).

### PositiveIntegerField

É semelhante ao **"IntegerField"**, mas permite somente valores positivos, ou seja, maiores ou iguais a zero.

### PositiveSmallIntegerField

É semelhante ao **"SmallIntegerField"**, mas permite somente valores positivos, ou seja, maiores ou iguais a zero.

### SlugField

Armazena valores do tipo *slug*, forma simplificada de identificar frases ou títulos que facilitem a vida dos usuários. Equivale ao **VARCHAR** de 50 caracteres.

### SmallIntegerField

Campo que armazena valores numéricos inteiros curtos, ou seja, geralmente na casa de 8 ou 16 bits. A capacidade varia de acordo com o banco de dados. Equivale ao **SMALLINT** (MySQL, PostgreSQL e SQLite) e **NUMBER(11)** (Oracle).

### TextField

Campo que armazena textos sem tamanho máximo definido. Em alguns bancos de dados, também permite recursos de busca (chamados de **Full Text Search**). Equivale ao **LONGTEXT** (MySQL), **TEXT** (PostgreSQL e SQLite) e **NCLOB** (Oracle).

### TimeField

Campo que armazena uma hora no formato **"HH:MM:SS.MS"**. Permite valor automático para a hora atual no momento da gravação. Equivale ao **TIME** (MySQL, PostgreSQL e SQLite) e **TIMESTAMP** (Oracle).

### URLField

Campo que armazena uma URL - endereço na web. Permite somente URLs válidas, a menos que seja definido para não fazer tal verificação. Equivale a **VARCHAR** de 200 caracteres.

### XMLField

Campo que armazena um texto no formato **XML**. Depende do aplicação **RelaxNG** para validar o XML informado. Armazena valores seguindo o mesmo padrão do **"TextField"**.

