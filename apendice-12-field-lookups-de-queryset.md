**Field Lookups** são filtros que podem ser aplicados aos métodos **.get()**, **.filter()** e **.exclude()** da QuerySet para aplicar condições ao capturar dados do banco de dados.

### exact

Define que o valor do campo deve ser exatamente igual ao valor atribuído ao parâmetro. Equivale ao **operador de igualdade** ( = ) do SQL (ou **IS** para o caso de valor **NULL**).

### iexact

Semelhante ao **exact**, mas é *case insensitive*, ou seja: aceita a diferença entre letras maiúsculas e minúsculas. Equivale ao operador **ILIKE** do SQL.

### contains

Define que o valor do campo deve **conter** o valor atribuído ao parâmetro. Equivale ao operador **LIKE '%<valor>%'** do SQL.

### icontains

Semelhante ao **contains**, mas é *case insensitive*, ou seja: aceita a diferença entre letras maiúsculas e minúsculas. Equivale ao operador **ILIKE '%<valor>%'** do SQL.

### in

Ao contrário do **contains**, o field lookup **in** define que o valor do campo deve **estar contido** na lista informada ao parâmetro. Equivale ao operador **IN** do SQL, mas dependendo do contexto, é transformado em **IN <subselect>**

### gt

Define que o valor do campo deve ser **maior que** o valor atribuído ao parâmetro. Equivale ao operador **maior que** ( > ) do SQL.

### gte

Define que o valor do campo deve ser **maior que ou igual** ao valor atribuído ao parâmetro. Equivale ao operador **maior que ou igual** ( >= ) do SQL.

### lt

Define que o valor do campo deve ser **menor que** o valor atribuído ao parâmetro. Equivale ao operador **menor que** ( < ) do SQL.

### lte

Define que o valor do campo deve ser **menor que ou igual** ao valor atribuído ao parâmetro. Equivale ao operador **menor que ou igual** ( <= ) do SQL.

### startswith

Define que o valor do campo deve **iniciar com** o valor atribuído ao parâmetro. Equivale ao operador **LIKE '<valor>%'** do SQL.

### istartswith

Semelhante ao **startswith**, mas é *case insensitive*, ou seja: aceita a diferença entre letras maiúsculas e minúsculas. Equivale ao operador **ILIKE '<valor>%'** do SQL.

### endswith

Define que o valor do campo deve **terminar com** o valor atribuído ao parâmetro. Equivale ao operador **LIKE '%<valor>'** do SQL.

### iendswith

Semelhante ao **endswith**, mas é *case insensitive*, ou seja: aceita a diferença entre letras maiúsculas e minúsculas. Equivale ao operador **ILIKE '%<valor>'** do SQL.

### range

Define que o valor do campo deve estar **dentro da faixa** atribuída ao parâmetro. Equivale ao operador **BETWEEN <início> AND <fim>** do SQL.

### year

Define que o **ano** de um campo DATE, DATETIME ou TIMESTAMP deve ser igual ao valor atribuído ao parâmetro. Equivale ao operador **EXTRACT('year' FROM <campo>)** do SQL.

### month

Define que o **mês** de um campo DATE, DATETIME ou TIMESTAMP deve ser igual ao valor atribuído ao parâmetro. Equivale ao operador **EXTRACT('month' FROM <campo>)** do SQL.

### day

Define que o **dia** de um campo DATE, DATETIME ou TIMESTAMP deve ser igual ao valor atribuído ao parâmetro. Equivale ao operador **EXTRACT('day' FROM <campo>)** do SQL.

### isnull

Define que o valor do campo deve ser NULL ou NOT NULL (nulo ou não nulo), dependendo do valor lógico (True ou False) atribuído ao parãmetro. Equivale aos operadores **IS NULL** / **IS NOT NULL** do SQL.

### search

Define que o valor do campo deve obedecer uma *string* atribuída ao parâmetro, que faz uma busca no banco de dados usando recursos de **Full Text Searching**.

Compatível somente com bancos de dados **MySQL** com suporte a **Full Text Searching** habilitado.

### regex

Define que o valor do campo deve **casar** com a **expressão regular** atribuída ao parâmetro. Equivalente aos operadores **REGEXP BINARY** no MySQL, **REGEXP_LIKE** no Oracle, **~** no PostgreSQL e **REGEXP** no SQLite.

### iregex

Semelhante ao **regex**, mas é *case insensitive*, ou seja: aceita a diferença entre letras maiúsculas e minúsculas.

