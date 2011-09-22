## Middlewares padrão

### django.middleware.common.CommonMiddleware

Middleware responsável por alguns comportamentos comuns e pequenos do Django, destacando-se:

- Bloqueio de acesso para navegadores definidos na _settings_ **"DISALLOWED\_USER\_AGENTS"**;
- Redirecionamento baseado nas _settings_ **"APPEND\_SLASH"** e **"PREPEND\_WWW"**;
- Cálculo de **ETags** com base na _setting__ **"USE\_ETAGS"**.

### django.middleware.transaction.TransactionMiddleware

Habilita o estado de transação nas _views_. Isso significa que a persistência em banco de dados feita nas _views_ é feita com base no recurso de _transactions_.

### django.middleware.cache.UpdateCacheMiddleware

Armazena em **cache** o resultado da URL após sua renderização (caso esta URL não esteja parametrizada para evitar _caching_).

### django.middleware.cache.FetchFromCacheMiddleware

Carrega do cache o conteúdo da URL se este existir.

### django.middleware.cache.CacheMiddleware

Trata-se da soma dos middlewares **"UpdateCacheMiddleware"** e **"FetchFromCacheMiddleware"**.

### django.middleware.doc.XViewMiddleware

Adiciona o cabeçalho **"X-View"** para requisições do tipo **"HEAD"**.

### django.middleware.locale.LocaleMiddleware

Acrescenta suporte à internacionalização.

### django.middleware.gzip.GZipMiddleware

Comprime o resultado da URL em formato **GZip** para navegadores que tenham suporte a isso. É uma boa opção para melhorar a performance em tempo de resposta, mas por outro lado aumenta a exigência do processador do servidor.

### django.middleware.http.ConditionalGetMiddleware

Acrescenta suporte a operações condicionais de requisições **"GET"**. Isso aumenta a performance em navegadores que tenham suporte a cache local e operações condicionais. Na prática, o Django passa a responder ao navegador se a URL foi modificada ou não depois da sua última atualização e assim evita retornar código redundante.

## Middlewares de aplicações _contrib_

### django.contrib.auth.middleware.AuthenticationMiddleware

Acrescenta o suporte padrão de autenticação da aplicação **"auth"**.

### django.contrib.csrf.middleware.CsrfMiddleware

Acrescenta proteção aos formulários contra ataques do tipo **Cross Site Request Forgery**. É um composto de outros dois middlewares: **"CsrfViewMiddleware"** e **"CsrfResponseMiddleware"**.

### django.contrib.flatpages.middleware.FlatpageFallbackMiddleware

Acrescenta suporte à aplicação **"flatpages"** que permite a criação dinâmica de páginas planas.

### django.contrib.redirects.middleware.RedirectFallbackMiddleware

Acrescenta suporte a redirecionamentos da aplicação **"redirects"**.

### django.contrib.sessions.middleware.SessionMiddleware

Acrescenta suporte a **sessões**.

