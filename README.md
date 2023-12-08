# Домашнее задание к занятию  «Система мониторинга Zabbix»
---

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
1. Скриншот авторизации в админки.
![Снимок экрана от 2023-12-08 16-28-52](https://github.com/kawahaweto/smon-hw-02/assets/150899286/8fa0c2a3-97bc-4da1-bd02-8bcfe2e66de7)
2. Список использованных команд

* Установка PostgreSQL
 ````bash
  apt update
  apt install postgresql
````
* Установак репозитория Zabbix
````bash
  wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4+debian11_all.deb
  dpkg -i zabbix-release_6.0-4+debian11_all.deb
  apt update
````
* Установка Zabbix sertver, веб-интерфейс и zabbix agent 
````bash
  apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
````
* Создаю пользователя БД и саму БД
````bash
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
````
* Импортирую начальную схему
````bash
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
````
* Редактирую и задаю DBPassword в файле /etc/zabbix/zabbix_server.conf

* Запускаю Zabbix server, Zabbix agent и добавляю в автозагрузку
````bash
sudo systemctl restart zabbix-server zabbix-agent  apache2 
sudo systemctl enable zabbix-server zabbix-agent  apache2
````

### Задание 2 

Установите Zabbix Agent на два хоста.

#### Процесс выполнения
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
3. Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
4. Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
5. Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.

#### Требования к результаты 
1. Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
2. Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
3. Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
4. Приложите в файл README.md текст использованных команд в GitHub

---
1. Раздел Configuration > Hosts
![image](https://github.com/kawahaweto/smon-hw-02/assets/150899286/3384c9d1-43ed-4290-8605-dfafc757693a)

2. Логи zabbix agent 
![image](https://github.com/kawahaweto/smon-hw-02/assets/150899286/6f887167-0a20-48e7-a5c3-09a61c3a2797)

3. Раздела Monitoring > Latest data
![image](https://github.com/kawahaweto/smon-hw-02/assets/150899286/3edfb340-1809-4de2-925b-e372b42a6d07)

4. Использованные команды

* Скачиваю репозитории Zabbix`a на хосты
````bash
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4+debian11_all.deb
dpkg -i zabbix-release_6.0-4+debian11_all.deb
apt update
````
* Установка Zabbix-agent
````bash
apt install zabbix-agent
````
* Добавляю адрес zabbix server на хостах
````bash
sudo nano  /etc/zabbix/zabbix_agent.conf
````
* Перезапускаю и добавляю в автозагрузку
````bash
sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent 
````

