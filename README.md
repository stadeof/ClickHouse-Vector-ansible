# Playbook для установки Clickhouse, Vector, Nginx, Lighthouse

## Требования к ОС

Данный плейбук создан под Centos/7 в рамках обучения

Он выполняет четыре плея для установки Clickhouse, Vector, Nginx, Lighhouse

## Переменные 

В файле group_var/*/vars.yml описаны версии сервисов, которые можно поменять по необходимости

## Templates

В каталоге ./templates содержатся конфигурации nginx

## Inventory

В файле inventory/prod.yml описано на каких хостах будет выполнен данный playbook

## Playbook

### Первый play "Install Clickhouse"

Handler "Start clickhouse service" создан для запуска сервиса

Task "Get clickhouse distrib" получает дистрибутив с packages.clickhouse.com

Task "Install clickhouse packages" устанавливает скачанные дистрибутивы

Task "Create database" создает базу данных "logs" 

### Второй play "Install Vector"

Handler "Start vector service" создан для запуска сервиса

Task "Get vector distrib" получает дистрибутив с packages.timber.io

Task "Install vector packages" устанавливает скачанный дистрибутив

### Третий play "Install Nginx"

Handler "start-nginx" создан для запуска Nginxa

Handler "reload-nginx" создан для перезагрузки конфигурации Nginx'a

Task "install EPEL repo" и "Install nginx for yum family" устанавливает репо и загружает Nginx

Task "Install vector packages" устанавливает скачанный дистрибутив

Task "Create general config" копирует конфигурацию nginx'a из каталога templates

### Четвертый play "Install Lighthouse"

Handler "reload-nginx" создан для перезагрузки конфигурации Nginx'a

Pre task "Install lighthouse dependency" устанавливает git

Task "Create general config" копирует конфигурацию nginx'a из каталога templates

Task "Lighthouse create config" копирует конфигурацию nginx для lighthouse в /etc/nginx/conf.d/

### Vagrant 

Выполнение данного playbook было проведено на виртуалках с Centos/7 с помощью Vagrant:

```v
# -*- mode: ruby -*-
# vi: set ft=ruby :

hosts = {
  "host0" => "192.168.56.10",
  "host1" => "192.168.56.11",
  "host2" => "192.168.56.12"
}

Vagrant.configure("2") do |config|
  hosts.each do |name, ip|
    config.vm.define name do |machine|
      machine.vm.box = "centos/7"
      machine.vm.hostname = "%s" % name
      machine.vm.network :private_network, ip: ip
      machine.vm.provider "virtualbox" do |v|
          v.name = name
          v.customize ["modifyvm", :id, "--memory", 256]
      end
    end
  end
end
```