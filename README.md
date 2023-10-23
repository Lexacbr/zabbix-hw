# Домашнее задание к занятию «Система мониторинга Zabbix» - Саведьев Алексей SYS-25


---
Что нужно сделать:
### Задание 1 

Установите Zabbix Server с веб-интерфейсом.

#### Процесс выполнения
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
3. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
4. Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.

#### Требования к результаты 
1. Прикрепите в файл README.md скриншот авторизации в админке.
2. Приложите в файл README.md текст использованных команд в GitHub.

### Задание 1

Что я делал:

1. Это задание выполнял по видеоуроку,немного изменив параметры указанные экспертом. Я устанавливал версию для ОС Ubuntu 22.04 т.к. на локальной машине (на железе) это основная ОС.
2. Сначала обновил систему
```bash 
sudo apt update
``` 
3. Потом установил Постгрес
```bash
sudo apt install postgresql
```
4. Установил репозиторий Zabbix
```bash
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu22.04_all.deb
dpkg -i zabbix-release_6.0-4+ubuntu22.04_all.deb
apt update
```
5. Установил Zabbix сервер, веб-интерфейс.
```bash
apt install zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-apache-conf zabbix-sql-scripts
```
6. Создал базу данных.
```bash
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
```
7. На хосте Zabbix сервера импортировал начальную схему и данные.
```bash
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```
8. Настроил базу данных для Zabbix сервера
Отредактировал файл /etc/zabbix/zabbix_server.conf
```bash
DBPassword=password
```
9.  Запустил процессы Zabbix сервера.
Запустил процессы Zabbix сервера и настроил их запуск при загрузке ОС.
```bash
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```
10. Открыл браузер и в строке ввел: ' http://192.168.1.71/zabbix/ ' и попал страницу входа. Для входа необходимо указать пользователя " Admin" пароль "zabbix"
![Страница входа](ссылка на скриншот 1)



`При необходимости прикрепитe сюда скриншоты
![Название скриншота 1](ссылка на скриншот 1)`


---

### Задание 2


