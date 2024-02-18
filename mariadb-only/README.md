# Stack

### Laravel

Laravel é um framework PHP elegante e expressivo para desenvolvimento de aplicativos web, conhecido por sua simplicidade, expressividade e rica ecossistema de pacotes.

### MariaDB

MariaDB é um sistema de gerenciamento de banco de dados relacional de código aberto, amplamente utilizado em aplicativos web como uma alternativa ao MySQL.


# Script Bash (run)

o arquivo `.\run` é um script bash que foi criado para facilitar o gerenciamento de serviços relacionados ao desenvolvimento com Laravel e MariaDB usando Docker Compose. Ele verifica a presença de PHP e Docker Compose, exibe informações sobre suas versões, e permite iniciar e parar serviços Laravel e MariaDB conforme necessário.

## Pré-requisitos

Certifique-se de ter os seguintes requisitos instalados em seu sistema:

- **PHP**: Para executar o Laravel.
- **Composer**: Para gerenciar os pacotes do PHP.
- **Docker Compose**: Para gerenciar os contêineres Docker.

## Uso

Execute o script da seguinte maneira:

```bash
sh ./run [laravel|mariadb] [up|down]
