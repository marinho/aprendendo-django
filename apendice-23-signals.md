## Signals do core

São signals relacionados a elementos fundamentais do _framework_. São encontrados no pacote **"django.core.signals"**.

### request_started

É disparado toda vez que uma requisição é iniciada no sistema.

### request_finished

É disparado toda vez que uma requisição chega ao final de seu processamento.

### got\_request\_exception

É disparado toda vez que uma exceção é levantada enquanto estiver processando uma requisição.

## Signals de classes de modelo

São signals relacionados a classes de modelo. Alguns deles são equivalentes às _triggers_ de bancos de dados. São encontrados no pacote **"django.db.models.signals"**.

### class_prepared

Disparado quando uma classe de modelo está sendo preparado pelo _framework_. Usado para fazer ajustes na classe de modelo.

### pre_init

Disparado no início da inicialização de uma instância de uma classe de modelo. Usado para acrescentar atributos e métodos, além de outros ajustes ao objeto.

### post_init

Disparado ao final da inicialização de uma instância de uma classe de modelo. Usado para acrescentar atributos e métodos, além de outros ajustes ao objeto.

### pre_save

Equivale às _triggers_ **"BEFORE INSERT"** e **"BEFORE UPDATE"** do banco de dados, pois é disparado exatamente antes que um objeto seja salvo no banco de dados.

### post_save

Equivale às _triggers_ **"AFTER INSERT"** e **"AFTER UPDATE"** do banco de dados, pois é disparado exatamente após um objeto ser salvo no banco de dados.

### pre_delete

Equivale à _trigger_ **"BEFORE DELETE"** do banco de dados, pois é disparado exatamente antes que um objeto seja excluído do banco de dados.

### post_delete

Equivale à _trigger_ **"BEFORE DELETE"** do banco de dados, pois é disparado exatamente após um objeto ser excluído do banco de dados.

### post_syncdb

Disparado no momento da geração do banco de dados.

## Signals de testes

Há apenas um, encontrado no pacote **"django.test.signals"**.

### template_rendered

Disparado somente em situação de teste, quando um template é renderizado.

## Signals da aplicação "comments"

São signals relacionados ao comportamento da aplicação contrib **"comments"**. São encontrados no pacote **"django.contrib.comments.signals"**.

### comment\_will\_be\_posted

Disparado exatamente antes de um comentário ser salvo no banco de dados.

### comment\_was\_posted

Disparado exatamente após um comentário ser salvo no banco de dados.

### comment\_was\_flagged

Disparado exatamente após um comentário receber uma classificação de **"flag"**.

