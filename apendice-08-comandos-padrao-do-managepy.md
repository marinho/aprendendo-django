**cleanup**

Limpa dados marcados para exclusão do banco de dados.

**compilemessages**

Compila arquivos de internacionalização **.po** para arquivos **.mo**.

**createcachetable**

Cria uma tabela no banco de dados para armazenar dados de cache.

**createsuperuser**

Cria um usuário com poderes de administrador.

**dbshell**

Abre o *shell* do banco de dados do projeto.

**diffsettings**

Mostra as diferenças entre o arquivo **settings.py** para as settings padrão do Django.

**dumpdata**

Cria arquivo serializado com dados do banco de dados para XML, JSON, Python ou YAML.

**flush**

Remove todos os dados mantendo somente o esquema de tabelas e outras entidades no banco de dados.

**inspectdb**

Cria classes de modelo a partir de um banco de dados.

**loaddata**

Popula o banco de dados com dados serializados de um arquivo de formato XML, JSON, Python ou YAML.

**makemessages**

Cria arquivos de internacionalização **.po** a partir dos arquivos do projeto.

**reset**

Executa comandos de DROP TABLE e CREATE TABLE que removem todas as tabelas do projeto do banco de dados e depois as cria novamente, para as aplicações informadas.

**runfcgi**

Inicia serviço de servidor para FastCGI.

**runserver**

Inicia serviço de servidor de desenvolvimento.

**shell**

Abre um shell interativo de Python (ou o shell do **IPython**) com as configurações do projeto já iniciadas.

**sql**

Imprime na tela os comandos de CREATE TABLE para as aplicações informadas.

**sqlall**

Imprime na tela os comandos de CREATE TABLE e de inserção de dados de inicialização de projeto para as aplicações informadas.

**sqlclear**

Imprime na tela os comandos de DROP TABLE para as aplicações informadas.

**sqlcustom**

Imprime na tela os o SQL customizado das aplicações informadas. Cada aplicação pode ter SQL customizado em um arquivo nomeado da seguinte forma:

    <pasta da aplicação>/sql/<classe de modelo>.sql
    

**sqlflush**

Imprime na tela os comandos de remoção para todos os dados mantendo somente o esquema de tabelas e outras entidades no banco de dados.

**sqlindexes**

Imprime na tela os comandos de CREATE INDEX SQL para geração de índices no banco de dados, das aplicações informadas.

**sqlreset**

Imprime na tela os comandos de DROP TABLE e CREATE TABLE que removem todas as tabelas do projeto do banco de dados e depois as cria novamente, para as aplicações informadas.

**sqlsequencereset**

Imprime na tela os comandos que resetam os contadores de sequência para campos de auto-numeração, das aplicações informadas.

**startapp**

Cria uma nova pasta com os arquivos básicos (**\_\_init\_\_.py**, **models.py** e **views.py**) de uma nova aplicação.

**startproject**

Este comando deve ser utilizado com o utilitário **django-admin.py** ao invés do **manage.py**.

Cria uma pasta com os arquivos básicos (**\_\_init\_\_.py**, **settings.py**, **urls.py** e **manage.py**) como um novo projeto.

**syncdb**

Gera as tabelas das classes de modelo que ainda não existem no banco de dados.

**test**

Executa suítes de **testes unitários** e testes textuais (**doctests**) das aplicações informadas.

**testserver**

Inicia um servidor de desenvolvimento para tests com dados gerados a partir de arquivos serializados.

**validate**

Valida todos as classes de modelo do projeto e imprime as inconsistências na tela.

Fonte: [http://docs.djangoproject.com/en/dev/ref/django-admin/#ref-django-admin](http://docs.djangoproject.com/en/dev/ref/django-admin/#ref-django-admin)

