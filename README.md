# Playbook для установки Clickhouse и Vector

## Требования к ОС

Данный плейбук создан под Centos/7 в рамках обучения.
Он выполняет два плея для установки Clickhouse и Vector соответственно.

## Переменные 

В файле group_var/clickhouse/vars.yml описаны версии сервисов, которые можно поменять по необходимости, а также перечислен список packages clickhouse для установки в цикле.

## Inventory

В файле inventory/prod.yml описано на каком хосте будет выполнен данный playbook.

## Playbook

### Первый play

Handler "Start clickhouse service" создан для запуска сервиса.

Task "Get clickhouse distrib" получает дистрибутив с packages.clickhouse.com

Task "Install clickhouse packages" устанавливает скачанные дистрибутивы

Task "Create database" создает базу данных "logs" 

### Второй play

Handler "Start vector service" создан для запуска сервиса.

Task "Get vector distrib" получает дистрибутив с packages.timber.io

Task "Install vector packages" устанавливает скачанный дистрибутив