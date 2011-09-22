## Decorators de autenticação

Encontrados no pacote **"django.contrib.auth.decorators"**.

### user\_passes\_test

É útil para definir a lógica para permitir um usuário acessar a uma _view_. Este _decorator_ recebe uma função (que pode ser um objeto _lambda_) que retorna True ou False para permitir o acesso de um usuário a uma _view_.

### login_required

A _view_ que usa este decorator só pode ser acessada se houver um usuário autenticado.

### permission_required

A _view_ que usa este decorator só pode ser acessada se houver um usuário autenticado e ele deve ser super-usuário ou ter a permissão indicada no argumento do decorator.

## Decorators do Admin

Há apenas um, encontrado no pacote **"django.contrib.admin.views.decorators"**.

### staff\_member\_required

A _view_ que usa este decorator só pode ser acessada se houver um usuário autenticado e ele deve ser super-usuário ou ser membro do _staff_.

## Decorators de transações de banco de dados

Decorators úteis para usar recursos de transações no banco de dados. São encontrados no pacote **"django.db.transaction"**.

### autocommit

Determina que na _view_ que usa este decorator, todas as persistências em banco de dados são feitas no modo **"auto-commit"**, ou seja, no mesmo instante em que são executadas.

### commit\_on\_success

Determina que todas as persistências feitas ao longo da _view_ devem ser aplicadas no banco de dados somente ao seu final, caso nenhum erro ocorrer.

### commit_manually

Determina que as persistências feitas ao longo da _view_ serão aplicadas manualmente em código, usando os métodos **"commit()"** e **"rollback()"** do pacote **"django.db.transaction"**

## Decorators para métodos de classes de modelo

Há apenas um, encontrado no pacote **"django.db.models"**.

### permalink

Este decorator facilita a criação de métodos em classes de modelo para retornar URLs.

## Decorators para template filters

Há apenas um, encontrado no pacote **"django.templates.defaultfilters"**.

### stringfilter

Este decorator facilita a criação de **template filters** para obrigar que estes recebam somente valores em formato **Unicode**.

## Decorators de cache

Encontrados no pacote **"django.views.decorators.cache"**.

### cache_page

Determina que o resultado da _view_ deve ser armazenado em _cache_ e determina o tempo de expiração para tal.

### cache_control

Atribui diversos parâmetros para o _cache_ da _view_.

### never_cache

Determina que as URLs derivadas desta _view_ nunca devem usar o _cache_ para armazenar ou retornar resultados.

## Decorators de variação de cabeçalho HTTP

Encontrados no pacote **"django.views.decorators.vary"**.

### vary\_on\_headers

Determina um parâmetro do cabeçalho HTTP para **"variar"** o cache local das URLs desta _view_.

### vary\_on\_cookie

Trata-se de uma variação do **"vary\_on\_cookie"** com uma aplicação comum: a variação dependendo da composição dos _cookies_.

## Decorators de compactação GZip

Há apenas um, encontrado no pacote **"django.views.decorators.gzip"**.

### gzip_page

Determina que o resultado da _view_ deve ser compactado usando **GZip**.

## Decorators de métodos HTTP

Encontrados no pacote **"django.views.decorators.http"**.

### require\_http\_methods

Estabelece uma lista de métodos HTTP aceitos na _view_. Métodos HTTP podem ser **"GET"**, **"POST"**, **"PUT"**, **"HEAD"**, **"DELETE"**, **"TRACE"**, **"OPTIONS"**, **"CONNECT"**.

### require_GET

Trata-se do decorator **"require\_http\_methods"** aplicado a aceitar somente método **"GET"**.

### require_POST

Trata-se do decorator **"require\_http\_methods"** aplicado a aceitar somente método **"POST"**.

