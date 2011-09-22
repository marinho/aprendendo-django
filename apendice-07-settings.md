**ABSOLUTE\_URL\_OVERRIDES**

Dicionário que define URLs do método **get\_absolute\_url()** para classes de modelo.

**ADMIN\_FOR**

Lista de módulos de settings que a **Documentação** do Admin (django.contrib.admindocs) deve analisar para gerar documentação. Útil para situações onde há múltiplos sites em um único projeto.

**ADMIN\_MEDIA\_PREFIX**

Define o caminho-base das URLs de arquivos estáticos do Admin.

**ADMINS**

Lista de Administradores do site e seus respectivos e-mails.

**ALLOWED\_INCLUDE\_ROOTS**

Lista de caminhos do HD que serão varridos quando for utilizada a template tag **{% ssi %}** em templates.

**APPEND\_SLASH**

Define que o Django deve adicionar uma barra comum ( **/** ) ao final da URL caso ela não a tenha.

**AUTHENTICATION\_BACKENDS**

Lista de backends para autenticação no projeto.

**AUTH\_PROFILE\_MODULE**

Classe de modelo que deve ser utilizada pelo método **get\_profile()** da classe de usuário (**User**).

**CACHE\_BACKEND**

Define qual backend de cache é usado pelo projeto. É aqui que se define se o cache deve ser armazenado em memória, banco de dados, arquivo ou outros, com suas respectivas configurações.

**CACHE\_MIDDLEWARE\_KEY\_PREFIX**

Define o prefixo da chave de cache para o **middleware de cache** de páginas.

**CACHE\_MIDDLEWARE\_SECONDS**

Segundos de duração paras as páginas gravadas em cache pelo **middleware de cache** ou pelo decorator **cache\_page()**.

**DATABASE\_ENGINE**

Define qual backend de banco de dados deve ser usado. É aqui que se define se o seu projeto utiliza MySQL, PostgreSQL, SQLite, Oracle ou outro.

**DATABASE\_HOST**

Nome ou IP do servidor de banco de dados. Ignore se seu **DATABASE\_ENGINE** for para o SQLite.

**DATABASE\_NAME**

Nome ou caminho do banco de dados. Se seu **DATABASE\_ENGINE** for para o SQLite, deve indicar o caminho completo do arquivo do banco de dados.

**DATABASE\_OPTIONS**

Define opções de configuração extra para o banco de dados. Varida de acordo com o **DATABASE\_ENGINE**.

**DATABASE\_PASSWORD**

Senha de acesso ao banco de dados. Ignore se seu **DATABASE\_ENGINE** for para o SQLite.

**DATABASE\_PORT**

Porta de acesso ao banco de dados. Ignore se seu **DATABASE\_ENGINE** for para o SQLite.

**DATABASE\_USER**

Username (ou login) de acesso ao banco de dados. Ignore se seu **DATABASE\_ENGINE** for para o SQLite.

**DATE\_FORMAT**

Formato padrão de data para listagens do **Admin**.

**DATETIME\_FORMAT**

Formato padrão de data/hora para listagens do **Admin**.

**DEBUG**

Define se o projeto está em modo de depuração ou não. Utilize **True** se estiver em desenvolvimento ou **False** se estiver em produção.

**DEBUG\_PROPAGATE\_EXCEPTIONS**

Define se, em caso de depuração, o suporte a depuração do Django deve ser suprimido e as exceções sejam tratadas da forma padrão do Python.

**DEFAULT\_CHARSET**

Codificação padrão de caracteres do projeto. É utilizado no momento da geração de templates e retorno de páginas e dados pela classe **HttpResponse** e suas variantes.

**DEFAULT\_CONTENT\_TYPE**

Define o **Content-type** padrão para retorno de páginas e dados pela classe **HttpResponse** e suas variantes.

**DEFAULT\_FILE\_STORAGE**

Define a classe que gerencia o armazenamento de arquivos, para campos **FileField** e **ImageField** de classes de modelo e campos **FileField** de classes de formulários dinâmicos.

**DEFAULT\_FROM\_EMAIL**

Endereço de e-mail padrão, para ser utilizado como **endereço de origem** no envio de e-mails.

**DEFAULT\_TABLESPACE**

Indica o *tablespace* do banco de dados, caso o **DATABASE\_ENGINE** tenha suporte e *tablespaces*.

**DEFAULT\_INDEX\_TABLESPACE**

Indica o *tablespace* do banco de dados para índices, caso o **DATABASE\_ENGINE** tenha suporte e *tablespaces*.

**DISALLOWED\_USER\_AGENTS**

Define uma lista de expressões regulares que identifiquem **User Agents** (navegadores, mecanismos de busca e outros tipos de software de acesso HTTP) que devem ser rejeitados pelo projeto.

**EMAIL\_HOST**

Nome ou IP do servidor de envio de e-mails (SMTP).

**EMAIL\_HOST\_PASSWORD**

Senha de acesso ao servidor de envio de e-mails (SMTP).

**EMAIL\_HOST\_USER**

Username (ou login) de acesso ao servidor de envio de e-mails (SMTP).

**EMAIL\_PORT**

Porta de acesso ao servidor de envio de e-mails (SMTP).

**EMAIL\_SUBJECT\_PREFIX**

**Assunto** padrão para envio de e-mails.

**EMAIL\_USE\_TLS**

Define se o acesso ao servidor de envio de e-mails (SMTP) tem segurança do tipo **TLS**.

**FILE\_CHARSET**

Define a codificação padrão de caracteres para arquivos gerados pelo Django.

**FILE\_UPLOAD\_HANDLERS**

Define a lista das classes que dão suporte ao upload de arquivos.

**FILE\_UPLOAD\_MAX\_MEMORY\_SIZE**

Define o tamanho (em bytes) máximo da memória para upload de arquivos.

**FILE\_UPLOAD\_TEMP\_DIR**

Define o caminho da pasta temporária onde devem ser armazenados os arquivos temporariamente enquanto eles são enviados pelos usuários.

**FILE\_UPLOAD\_PERMISSIONS**

Máscara em formato numérico (do padrão de permissões Unix/Linux) para definir as permissões que os arquivos devem receber quando são enviados por upload.

**FIXTURE\_DIRS**

Define uma lista de pasta onde devem ser procurados arquivos de **fixtures**.

**FORCE\_SCRIPT\_NAME**

Se for definida com valor diferente de None, o valor será atribuído à variável de ambiente **SCRIPT\_NAME**, que é levada na requisição HTTP. Em muitas situações no uso de FastCGI ou WSGI com rewrite, é útil informar essa setting com '' (vazio) ou algum valor diferente para ajustar o funcionamento do site.

**IGNORABLE\_404\_ENDS**

Define a lista de sufixos para URLs que devem ser ignoradas quando o Django não encontrar e iniciar o envio de mensagens de erro aos administradores do site.

**IGNORABLE\_404\_STARTS**

Define a lista de prefixos para URLs que devem ser ignoradas quando o Django não encontrar e iniciar o envio de mensagens de erro aos administradores do site.

**INSTALLED\_APPS**

Lista das aplicações instaladas no projeto.

**INTERNAL\_IPS**

Define uma lista com IPs considerados como parte do desenvolvimento. Se definida com uma lista com itens, então tanto os **comentários de DEBUG** quanto o recebimento de cabeçalhos no middleware **XViewMiddleware** estarão ativos somente para esses IPs.

**JING\_PATH**

Caminho para o executável do **Jing**, o utilitário usado pelo Django para validar o conteúdo valores para campos **XMLField**.

**LANGUAGE\_CODE**

Define o código do idioma padrão do projeto.

**LANGUAGE\_COOKIE\_NAME**

Define o nome do **cookie** que será armazenado no navegador para definir qual foi o idioma escolhido pelo usuário para navegar no site.

**LANGUAGES**

Define uma lista dos idiomas suportados pelo site.

**LOCALE\_PATHS**

Define uma lista de caminhos onde o Django deve procurar por arquivos de tradução (localização). Os caminhos padrão são as pastas **"locale"** na pasta do projeto e nas pastas das aplicações.

**LOGIN\_REDIRECT\_URL**

URL que será carregada depois de o usuário fazer o logon.

**LOGIN\_URL**

URL que será carregada quando o usuário tenta acessar uma página restrita e ele não está autenticado.

**LOGOUT\_URL**

URL que será chamada para efetuar o logout do sistema.

**MANAGERS**

Define a lista de gestores do site e seus respectivos e-mails.

**MEDIA\_ROOT**

Caminho no HD para a pasta de arquivos estáticos.

**MEDIA\_URL**

URL que representa o caminho definido em **MEDIA\_ROOT**.

**MIDDLEWARE\_CLASSES**

Lista de classes de middleware ativos no projeto.

**MONTH\_DAY\_FORMAT**

Define a formatação para datas que tenham somente **dia e mês**, no Admin.

**PREPEND\_WWW**

Define se o Django deve adicionar o prefixo **'www.'** antes do domínio quando este não for informado.

**PROFANITIES\_LIST**

Define uma lista de palavras tidas como "profanas", ou seja, palavras de baixo calão ou de cunho pejorativo, para serem rejeitadas na validação de dados.

**ROOT\_URLCONF**

Caminho separado por pontos ( . ) do módulo Python que possui a raiz das URLs do projeto. O valor mais recomendado para essa setting é **'urls'**.

**SECRET\_KEY**

Chave secreta do projeto, utilizada para a criptografia de senhas do sistema de usuários e outras formas de criptografia. Também é útil para se trabalhar com **Single Sign-On**, conceito de autenticação compartilhado entre vários sites ou servidores.

**SEND\_BROKEN\_LINK\_EMAILS**

Define se o Django deve enviar um e-mail para os administradores do site toda vez que um endereço não é encontrado (erro **404**) no site.

**SERIALIZATION\_MODULES**

Dicionário que define os módulos de serialização suportados pelo projeto.

**SERVER\_EMAIL**

Endereço de e-mail que será usado como endereço **from** de e-mails enviados automaticamente para os administradores do site.

**SESSION\_ENGINE**

Define o caminho da classe que gerencia as sessões do projeto.

**SESSION\_COOKIE\_AGE**

Tempo para expiração, em segundos, para o cookie que armazena o código da sessão no navegador do usuário.

**SESSION\_COOKIE\_DOMAIN**

Domíno do cookie que armazena o código da sessão no navegador do usuário.

**SESSION\_COOKIE\_NAME**

Nome do cookie que armazena o código da sessão no navegador do usuário.

**SESSION\_COOKIE\_PATH**

Caminho de URL do cookie que armazena o código da sessão no navegador do usuário.

**SESSION\_COOKIE\_SECURE**

Define se o cookie que armazena o código da sessão no navegador do usuário deve ser seguro ou não. Se um cookie é definido como **"seguro"**, é exigida criptografia SSL e conexão pelo protocolo HTTPS.

**SESSION\_EXPIRE\_AT\_BROWSER\_CLOSE**

Define se a sessão deve expirar quando o navegador é fechado.

**SESSION\_FILE\_PATH**

Define o caminho no HD onde serão salvas as sessões, caso elas sejam armazenadas em arquivo.

**SESSION\_SAVE\_EVERY\_REQUEST**

Define se a sessão deve ser salva em todas as requests, independentemente se elas foram definidas como modificadas ou não. Esta setting é útil para quando se altera atributos ou chaves de itens da sessão sem alterar a própria sessão em si, causando um engano ao Django. Neste caso, a sessão é forçada a ser salva e garante a modificação feita.

**SITE\_ID**

Código do site ativo no projeto. Este código pode ser encontrado na coluna **"id"** da tabela de **sites** no banco de dados ou na classe **"django.contrib.sites.models.Site"**.

**TEMPLATE\_CONTEXT\_PROCESSORS**

Lista de funções que retornam Template Context Processors.

**TEMPLATE\_DEBUG**

Define se o template de depuração deve ser exibido quando um erro ocorrer no projeto.

**TEMPLATE\_DIRS**

Define uma lista de pastas onde serão localizados templates que não sejam encontrados nas pastas de templates das aplicações.

**TEMPLATE\_LOADERS**

Define uma lista de caminhs para classes que carregam templates.

**TEMPLATE\_STRING\_IF\_INVALID**

Define uma string que será exibida caso uma variável no template não for válida. É útil para se depurar o sistema, mas é recomendável que tenha valor vazio ( '' ) em ambientes de produção.

**TEST\_DATABASE\_CHARSET**

Define a codificação de caracteres para o banco de dados gerado automaticamente para testes.

**TEST\_DATABASE\_COLLATION**

Define a *collation* de caracteres para o banco de dados gerado automaticamente para testes.

**TEST\_DATABASE\_NAME**

Define o nome para o banco de dados gerado automaticamente para testes.

**TEST\_RUNNER**

Define o caminho de uma classe que gerencia a execução de testes no projeto. Com esta setting é possível trabalhar com suítes de testes alternativas.

**TIME\_FORMAT**

Formato padrão de hora para listagens do **Admin**.

**TIME\_ZONE**

Define o fuso horário padrão do projeto.

**URL\_VALIDATOR\_USER\_AGENT**

Define um nome para ser utilizado quando o Django varre sites para validar URLs.

**USE\_ETAGS**

Define se o middleware **"CommonMiddleware"** deve trabalhar com **Etags** no cabeçalho das requisições e respostas. Se definida como True, esta setting diminui o tráfego de dados na rede mas aumenta o custo de processamento.

**USE\_I18N**

Define se o projeto trabalhar com internacionalização, ou seja, se trabalha mais de um idioma em suas páginas.

**YEAR\_MONTH\_FORMAT**

Define a formatação para datas que tenham somente **ano e mês**, no Admin.

