**Generic Views** são _views_ criadas para uso genérico. Com elas é possível declarar uma URL, informar alguns parâmetros, vincular à _generic view_ ter tudo funcionando rapidamente.

## Generic views "simples"

São encontradas no pacote **"django.views.generic.simple"**.

### direct_to_template

Aponta uma URL diretamente para um template.

### redirect_to

Redireciona para outra URL.

## Generic views baseadas em data

Todas elas necessitam de templates para que funcionem adequadamente. São encontradas no pacote **"django.views.generic.date\_based"**.

### archive_index

Exibe uma lista de objetos ordenados por data, em forma de "arquivo". Usado em páginas iniciais de blogs.

### archive_year

Exibe uma lista de **anos** disponíveis em uma lista de objetos. Usado para arquivos de blogs e sites de notícias.

Exemplo: se seu blog possui artigos para os anos 2008 e 2009, então esses dois anos são exibidos para o usuário escolher um deles.

### archive_month

Exibe uma lista de **meses** disponíveis em um ano específico de uma lista de objetos. Usado para arquivos de blogs e sites de notícias.

Exemplo: se no ano de 2008 seu blog possui artigos para os meses "Novembro" e "Dezembro", então esses dois mêses são exibidos para o usuário escolher um deles.

### archive_week

Exibe uma lista de objetos de uma semana específica, de um ano específico. Usado para arquivos de blogs e sites de notícias.

### archive_day

Exibe uma lista de objetos de uma data específica. Usado para arquivos de blogs e sites de notícias.

### archive_today

Semelhante ao **"archive\_day"**, mas se retringe à data atual.

### object_detail

Exibe os detalhes de um objeto. Ideal para páginas de apresentação de um artigo em um blog ou notícia.

## Generic views de visualização

Todas elas necessitam de templates para que funcionem adequadamente. São encontradas no pacote **"django.views.generic.list\_detail"**.

### object_list

Exibe uma lista de objetos.

### object_detail

Semelhante à **"django.views.generic.date\_based.object\_detail"**, mas não se prende à um campo de data/hora.

## Generic views de persistência

Necessitam de templates para que funcionem adequadamente. São encontradas no pacote **"django.views.generic.create\_update"**.

### create_object

Permite a criação simples e rápida de novos objetos.

### update_object

Permite a atualização simples e rápida de objetos já existentes.

### delete_object

Permite a exclusão simples e rápida de objetos já existentes.

## Generic views de autenticação

Necessitam de templates para que funcionem adequadamente. São encontradas no pacote **"django.contrib.auth.views"**.

### login

Permite o _logon_ (entrada) de usuários ao site.

### logout

Permite o _logoff_ (saída) do usuário autenticado do site.

### logout\_then\_login

Permite o _logoff_ para logo em seguida abrir a página para _logon_ novamente. Usada para a função de **"trocar usuário"**.

### password_reset

Inicia o processo de recriar a senha para um usuário que eventualmente esqueceu sua senha. Esta _view_ envia um e-mail ao usuário solicitando que ele confirme a recriação de sua senha, clicando sobre um _link_ de uma URL para confirmação.

Usada para a função de **"esqueci minha senha"** que consta em muitos sites da web.

### password\_reset\_done

Generic view para URL que exibe uma mensagem após o usuário solicitar a recriação de sua senha.

### password\_reset\_confirm

Generic view para URL que confirma se o usuário realmente deseja recriar sua senha.

### password\_reset\_complete

Generic view para URL exibida ao usuário completar o processo de recriar sua senha.

### password_change

Generic view para mudança de senha. Solicita a senha atual e uma nova senha que deve ser digitada também em um campo para confirmação.

### password\_change\_done

Generic view para URL que exibe mensagem após o usuário mudar sua senha.

## Generic views de feeds RSS/Atom

Há apenas uma, encontrada no pacote **"django.contrib.syndication.views"**.

### feed

Usada para retornar um _feed_ RSS ou Atom para uma classe de modelo.

## Generic views para arquivos estáticos

Há apenas uma, encontrada no pacote **"django.views.static"**.

### serve

Serve arquivos estáticos de uma determinada pasta do HD. Não deve ser usada em servidor de produção.

## Generic views de internacionalização

Encontradas no pacote **"django.views.i18n"**

### javascript_catalog

Acrescenta suporte à internacionalização de elementos em JavaScript.

### null\_javascript\_catalog

Trata-se de uma versão "vazia" da generic view **"javascript\_catalog"**, pois retorna elementos para internacionalização de JavaScript que fato não fazem nada.

## Generic views para sitemaps

Encontradas no pacote **"django.contrib.sitemaps.views"**.

### index

Permite que o site tenha um "índice" de sitemaps, que indicam as URLs do site e algumas de suas características como frequência de atualização e grau de importância no conteúdo do site. Este recurso é importante para manter o site atualizado junto a sistemas de buscas como Google, Yahoo, MSN e Ask.

### sitemap

Semelhante à _generic view_ **"index"**, mas permite uma granularidade maior, pois indica apenas um _sitemap_.


