Os métodos de QuerySet são utilizados para adicionar regras a uma operação no banco de dados. Muitos deles retornam outras QuerySets, o que possibilita o encadeamento de diversos métodos para combinar regras.

### all()

Retorna todos os objetos da QuerySet.

### count()

Retorna o total de objetos da QuerySet.

### create(**kwargs)

Cria um objeto e salva no banco de dados, com os valores dos parâmetros informados.

### dates(field, kind, order='ASC')

Retorna os objetos da QuerySet agrupados por data.

### distinct()

Retorna os objetos da QuerySet aplicando filtro para evitar duplicações.

### exclude(**kwargs)

Aplica filtro negativo à QuerySet. Isso quer dizer que os objetos que combinarem com os parâmetros informados não serão retornados na QuerySet.

### extra(select=None, where=None, params=None, tables=None, order\_by=None, select\_params=None)

Aplica regras avançadas à QuerySet, como *sub-selects*, campos calculados, condições especiais, tabelas adicionais e ordenação por campos calculados.

### filter(**kwargs)

Aplica filtro afirmativo à QuerySet. Isso quer dizer que somente os objetos que combinarem com os parâmetros informados serão retornados na QuerySet.

### get(**kwargs)

Retorna um único objeto que obedeça aos parâmetros informados. Caso seja encontrado mais de um objeto, uma exceção do tipo **MultipleObjectsReturned** é levantada. Por outro lado, se nenhum objeto for encontrado, uma exceção do tipo **DoesNotExist** é levantada.

### get\_or\_create(**kwargs)

Verifica se já existe um objeto que case com os parâmetros informados, se não houver, o objeto é criado.

### in\_bulk(id\_list)

Retorna os objetos da QuerySet em um dicionário onde a chave é seu **id**.

### iterator()

Retorna um iterador para os objetos a QuerySet. Útil para QuerySets que retornam uma grande quantidade de objetos.

### latest(field\_name=None)

Retorna o último objeto de acordo com o campo definido pelo parâmetro ou pelo atributo **get\_latest\_by** da classe **Meta** da classe de modelo.

### none()

Retorna uma QuerySet vazia ( **EmptyQuerySet** ).

### order\_by(*fields)

Ordena a QuerySet por um ou mais campos.

### reverse()

Inverte a ordenação da QuerySet.

### select\_related()

Retorna não só os objetos da QuerySet, mas também seus objetos relacionados.

### values(*fields)

Retorna uma tupla com os valores dos campos informados agrupados em dicionários. Útil para situações onde se deseja capturar do banco de dados os valores de alguns campos específicos e ganhar em performance.

### values\_list(*fields)

Semente ao método **"values"**, mas ao invés da tupla ser composta por dicionários, ela é composta por outras tuplas.

