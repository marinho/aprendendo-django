Classes de retorno baseadas na classe **"HttpRequest"** são encontradas no pacote **"django.http"**.

### HttpResponse

Classe básica de retorno HTTP. Esta classe é extendida pelas demais e também usada com certa frequência para retornar todo tipo de dados ao navegador.

### HttpResponseRedirect

Retorna o redirecionamento para outra URL, usando código de status **302**, que significa **"redirecionamento temporário"**.

### HttpResponsePermanentRedirect

Retorna o redirecionamento para outra URL, usando código de status **301**, que significa **"redirecionamento permanente"**.

### HttpResponseNotModified

Retorna a indicação de que a URL não teve modificação em seu conteúdo ao navegador, forçando-o a usar o conteúdo armazenado em cache local. Usa código de status **304**.

### HttpResponseBadRequest

Retorna erro apontando requisição "ruim". Usa código de status **400**.

### HttpResponseNotFound

Retorna erro apontando **"página não encontrada"**. Usa código de status **404**.

### HttpResponseForbidden

Retorna erro indicando que algo teve seu **acesso negado**. Usa código de status **403**.

### HttpResponseNotAllowed

Retorna erro indicando a URL em questão **não é permitida**. Usa código de status **405**.

### HttpResponseGone

Retorna erro indicando a **resposta foi perdida**. Usa código de status **410**.

### HttpResponseServerError

Retorna erro indicando **ocorreu um erro no servidor**. Usa código de status **500**.

