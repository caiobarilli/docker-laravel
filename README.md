# Alpine - Nginx - Php 8.3 - Composer - Supervisor - Redis - MySQL

**Nginx**: Servidor web usado para servir o aplicativo Laravel/Lumen e encaminhar solicitações para o PHP-FPM.

**PHP-FPM**: Process Manager para PHP, responsável por executar scripts PHP.

**Composer**: Gerenciador de dependências para PHP, usado para instalar e gerenciar pacotes PHP, incluindo o Laravel/Lumen.

**MySQL**: Banco de dados relacional MySQL para armazenar dados do aplicativo.

**Supervisor**: Processo de controle de sistema usado para gerenciar e monitorar os processos do Nginx e do PHP-FPM.

**Redis**: Utilizado para armazenamento em cache, gerenciamento de sessão e comunicação entre componentes de aplicativos devido à sua alta velocidade e eficiência.

## Requisitos

- Docker instalado
- Docker Compose instalado

## Instalação

Acesse a pasta e faça download das imagens e construa os containers com o comando:

```sh
docker-compose build
```

Suba a primera vez os containers com o comando:

```sh
docker-compose up
```

🔹 No Windows:
Abra um terminal como administrador e execute:

```sh
notepad C:\Windows\System32\drivers\etc\hosts
```

🔹 No Linux/macOS:
Abra um terminal e execute:

```sh
sudo nano /etc/hosts
```

Verifique se existe a linha:

```sh
127.0.0.1 app.laravel.test
```

# Script Bash "run"

Este é um script em bash chamado "run" que pode ser usado para automatizar tarefas comuns em um ambiente de desenvolvimento Laravel/Lumen que utiliza contêineres Docker.

## Uso

O script aceita os seguintes argumentos:

- `bash`: Inicia um shell dentro do contêiner Docker.
- `laravel:install`: Instala um novo projeto Laravel dentro do contêiner Docker.
- `laravel:permissions`: Configura as permissões do projeto Laravel dentro do contêiner Docker.

Exemplo de uso:

```bash
./run bash            # Inicia um shell dentro do contêiner Docker.
./run laravel:install # Instala um novo projeto Laravel dentro do contêiner Docker.
./run laravel:permissions  # Configura as permissões do projeto Laravel dentro do contêiner Docker.
```

Exemplo do .env:

```bash
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=laravel

REDIS_CLIENT=predis
REDIS_HOST=redis
REDIS_PASSWORD=null
REDIS_PORT=6379
```
