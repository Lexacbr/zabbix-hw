# Домашнее задание к занятию «Система мониторинга Zabbix» - Савельев Алексей SYS-25


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

---

### Выполнение 1

Что я делал:

1. Это задание выполнял по видеоуроку,немного изменив параметры указанные экспертом. Я устанавливал версию для ОС Ubuntu 22.04 т.к. на локальной машине (на железе) это основная ОС.

![Страница входа](https://github.com/Lexacbr/zabbix-hw/blob/main/screenshots/install_set.png)

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

![Вход в zabbix](https://github.com/Lexacbr/zabbix-hw/blob/main/screenshots/inter.png)
![Dashboard](https://github.com/Lexacbr/zabbix-hw/blob/main/screenshots/dash-zabbix.png)

---

### Задание 2

Что нужно сделать:
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
3. Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
4. Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
5. Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.

Требования к результаты
Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
Приложите в файл README.md текст использованных команд в GitHub

---

### Выполнение 2

1. Я выполнил установку 2-ух Агентов Zabbix на 2-ух хостах. Один хост - это локальная машина с установленным zabbix-server, а второй хост - это ВМ, запущенная на локальном хосте. Для установки на локальной машине я выполнил следующие команды:
2. По-скольку репозиторий на данной машине уже подключен (с установкой zabbix-server), то мне необходимо просто установить сам Агент:

![Выбор установки](https://github.com/Lexacbr/zabbix-hw/blob/main/screenshots/inst-agent.png)

```bash 
sudo apt install zabbix-agent
```
3. Дальше я запускаю перезагрузку сервиса и его включение в автозагрузку:

```bash
systemctl restart zabbix-agent

systemctl enable zabbix-agent
```
4. Нашел конфигурационный файл:
```bash 
sudo find / -name zabbix_agentd.conf 
``` 
и отредактировал строку для подключения Агента к Серверу:
```bash
sudo nano /etc/zabbix/zabbix_agentd.conf
```
![Редактирование](https://github.com/Lexacbr/zabbix-hw/blob/main/screenshots/nano.png)

5. После редактирования файла я снова рестартовал Сервис zabbix.

![Логи подключения](https://github.com/Lexacbr/zabbix-hw/blob/main/screenshots/tail.png)
![Данные](https://github.com/Lexacbr/zabbix-hw/blob/main/screenshots/last-data.png)
![Редактирование](https://github.com/Lexacbr/zabbix-hw/blob/main/screenshots/nano.png)