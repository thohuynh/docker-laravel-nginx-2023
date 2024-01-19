## Requirement
- OS: *nix (Macos, Ubuntu…). Some commands in this document maybe not working in Windows. If you are Windows user, you should do some action by manual (git clone, copy env local…)
- Docker: version 1.13.0+
- Download example database: (TODO)
- Bitbucket Repository Access: You should contact your manager to add your email to all repository.
- Personal SSH Key: Make sure that your personal ssh key added to bitbucket: https://bitbucket.org/account/settings/ssh-keys/

## Clone repository and Build
This document will use ~/code/market as ROOT directory. You totally use custom path yourself but in this case, you must change env variable APP_PATH before building docker image.

NOTE: ROOT directory contain ALL service of project: o2o, gateway, wellet...

Go to ROOT directory and run command clone this repository and create .env file.

```shell
git clone git@bitbucket.org:ttutamarket/ttuttamarket_local_development.git development && cp deployment/.env{.example,}
```

Clone o2o service

```shell
git clone git@bitbucket.org:ttutamarket/o2o_service.git o2o && cp o2o/.env{.example,}
```

Clone gateway service

```shell
git clone git@bitbucket.org:ttutamarket/ttuttamarket_gateway_api.git gateway && cp gateway/.env{.example,}
```

Go to `development` project.

```shell
cd development
```

If you use custom path as ROOT directory, config APP_PATH in `deployment/.env`. Make sure Docker has access permission to your path.

```shell
APP_PORT=1080
APP_PATH=~/code/market
POSTGRES_PORT=15432
REDIS_PORT=16379
LOCALSTACK_PORT=14566
COMPOSE_PROJECT_NAME=market
```

Create `data` folder for postgres service

```shell
mkdir docker/postgres/data
```

Build and run docker compose

```shell
docker-compose up -d --build
```

Go to container

```shell
docker-compose exec app /bin/sh
```

Composer install o2o

```shell
cd o2o
composer install
```

Composer install gateway

```shell
cd gateway
composer install
```

## Connect and Import Database
- Connect pgsql from SQL Tool

```shell
Host: localhost
Port: 15432
User: postgres
Password: secret
Database: market
```

- Import example database which you downloaded in Requirement section (TODO)

## Setup Postman
(TODO)

## Conclusion
That’s all. You already to complete setup market project in local. Let’s make amazing code.