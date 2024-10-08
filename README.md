# Стасенко Г.М. Домашнее задание к занятию 5. «Практическое применение Docker»

## Задача 0

1.Убедитесь что у вас НЕ(!) установлен docker-compose, для этого получите следующую ошибку от команды docker-compose --version
![image](https://github.com/user-attachments/assets/ac14ed0f-619e-4314-8a2b-c7c954bd93f2)

В случае наличия установленного в системе docker-compose - удалите его.

2. Убедитесь что у вас УСТАНОВЛЕН docker compose(без тире) версии не менее v2.24.X, для это выполните команду docker compose version
![image](https://github.com/user-attachments/assets/5d1871f9-bb5a-414f-923d-b92c87a2ed9c)

## Задача 1

1. Сделайте в своем github пространстве fork репозитория. Примечание: В связи с доработкой кода python приложения. Если вы уверены что задание выполнено вами верно, а код python приложения работает с ошибкой то используйте вместо main.py файл old_main.py(просто измените CMD)
  ![image](https://github.com/user-attachments/assets/948d354e-0b68-40d4-bd03-effa1be9f274)
 ![image](https://github.com/user-attachments/assets/ba4ac25f-472c-45cf-9685-eabbe14a868b)
https://github.com/Nightnek/shvirtd-example-python

2. Создайте файл с именем Dockerfile.python для сборки данного проекта(для 3 задания изучите https://docs.docker.com/compose/compose-file/build/ ). Используйте базовый образ python:3.9-slim. Обязательно используйте конструкцию COPY . . в Dockerfile. Не забудьте исключить ненужные в имадже файлы с помощью dockerignore. Протестируйте корректность сборки.
   ![image](https://github.com/user-attachments/assets/44334819-4bc8-4de7-8b9a-58fb03b4d895)

3. (Необязательная часть, *) Изучите инструкцию в проекте и запустите web-приложение без использования docker в venv. (Mysql БД можно запустить в docker run).
   ![image](https://github.com/user-attachments/assets/bc2f9cf1-3dd1-49ca-88b1-3c8889e303eb)

4. (Необязательная часть, *) По образцу предоставленного python кода внесите в него исправление для управления названием используемой таблицы через ENV переменную.
![image](https://github.com/user-attachments/assets/1b13fc4c-d07f-4c7c-86f9-9d91a7c9ee0d)
![image](https://github.com/user-attachments/assets/9c03a004-2699-438f-a3ed-051e1397bc04)
![image](https://github.com/user-attachments/assets/940da2e5-6e61-4b32-97b7-50675392626b)

## Задача 2 (*)

1. Создайте в yandex cloud container registry с именем "test" с помощью "yc tool" . Инструкция
2. Настройте аутентификацию вашего локального docker в yandex container registry.
3. Соберите и залейте в него образ с python приложением из задания №1.
   ![image](https://github.com/user-attachments/assets/0af00885-6f1f-4cad-aa07-8aa1c9006cc1)

4. Просканируйте образ на уязвимости.
5. В качестве ответа приложите отчет сканирования.
   ![image](https://github.com/user-attachments/assets/49bbc3eb-9e61-480e-ab1b-cf0bbfe9c4a4)


## Задача 3
1. Изучите файл "proxy.yaml"
2. Создайте в репозитории с проектом файл ```compose.yaml```. С помощью директивы "include" подключите к нему файл "proxy.yaml".
3. Опишите в файле ```compose.yaml``` следующие сервисы: 

- ```web```. Образ приложения должен ИЛИ собираться при запуске compose из файла ```Dockerfile.python``` ИЛИ скачиваться из yandex cloud container registry(из задание №2 со *). Контейнер должен работать в bridge-сети с названием ```backend``` и иметь фиксированный ipv4-адрес ```172.20.0.5```. Сервис должен всегда перезапускаться в случае ошибок.
Передайте необходимые ENV-переменные для подключения к Mysql базе данных по сетевому имени сервиса ```web``` 

- ```db```. image=mysql:8. Контейнер должен работать в bridge-сети с названием ```backend``` и иметь фиксированный ipv4-адрес ```172.20.0.10```. Явно перезапуск сервиса в случае ошибок. Передайте необходимые ENV-переменные для создания: пароля root пользователя, создания базы данных, пользователя и пароля для web-приложения.Обязательно используйте уже существующий .env file для назначения секретных ENV-переменных!

2. Запустите проект локально с помощью docker compose , добейтесь его стабильной работы: команда ```curl -L http://127.0.0.1:8090``` должна возвращать в качестве ответа время и локальный IP-адрес. Если сервисы не стартуют воспользуйтесь командами: ```docker ps -a ``` и ```docker logs <container_name>``` . Если вместо IP-адреса вы получаете ```NULL``` --убедитесь, что вы шлете запрос на порт ```8090```, а не 5000.
![image](https://github.com/user-attachments/assets/7a005cc8-b597-41db-99e7-b2b9b84e3f2c)

''' В Windows не работает метод network_mode: host '''

5. Подключитесь к БД mysql с помощью команды ```docker exec <имя_контейнера> mysql -uroot -p<пароль root-пользователя>```(обратите внимание что между ключем -u и логином root нет пробела. это важно!!! тоже самое с паролем) . Введите последовательно команды (не забываем в конце символ ; ): ```show databases; use <имя вашей базы данных(по-умолчанию example)>; show tables; SELECT * from requests LIMIT 10;```.
![image](https://github.com/user-attachments/assets/66135e50-bf92-4738-b3f3-8a8c978b727a)
![image](https://github.com/user-attachments/assets/eb9ec9d2-c096-4fee-b917-68e7e9c60c3c)

6. Остановите проект. В качестве ответа приложите скриншот sql-запроса.
![image](https://github.com/user-attachments/assets/ab5a4ecc-827d-4079-b503-a724c9fc7f04)

## Задача 4
1. Запустите в Yandex Cloud ВМ (вам хватит 2 Гб Ram).
2. Подключитесь к Вм по ssh и установите docker.
3. Напишите bash-скрипт, который скачает ваш fork-репозиторий в каталог /opt и запустит проект целиком.
5. Зайдите на сайт проверки http подключений, например(или аналогичный): ```https://check-host.net/check-http``` и запустите проверку вашего сервиса ```http://<внешний_IP-адрес_вашей_ВМ>:8090```. Таким образом трафик будет направлен в ingress-proxy. ПРИМЕЧАНИЕ:  приложение(old_main.py) весьма вероятно упадет под нагрузкой, но успеет обработать часть запросов - этого достаточно. Обновленная версия (main.py) не прошла достаточного тестирования временем, но должна справиться с нагрузкой.
6. (Необязательная часть) Дополнительно настройте remote ssh context к вашему серверу. Отобразите список контекстов и результат удаленного выполнения ```docker ps -a```
7. В качестве ответа повторите  sql-запрос и приложите скриншот с данного сервера, bash-скрипт и ссылку на fork-репозиторий.
![image](https://github.com/user-attachments/assets/028e932b-b3db-49ee-93fa-ae9570d77bbf)

```
cd /opt
git clone https://github.com/Nightnek/shvirtd-example-python.git
cd shvirtd-example-python
docker compose up -d

```
https://github.com/Nightnek/shvirtd-example-python

## Задача 5 (*)
1. Напишите и задеплойте на вашу облачную ВМ bash скрипт, который произведет резервное копирование БД mysql в директорию "/opt/backup" с помощью запуска в сети "backend" контейнера из образа ```schnitzler/mysqldump``` при помощи ```docker run ...``` команды. Подсказка: "документация образа."
2. Протестируйте ручной запуск
3. Настройте выполнение скрипта раз в 1 минуту через cron, crontab или systemctl timer. Придумайте способ не светить логин/пароль в git!!
4. Предоставьте скрипт, cron-task и скриншот с несколькими резервными копиями в "/opt/backup"

## Задача 6
Скачайте docker образ ```hashicorp/terraform:latest``` и скопируйте бинарный файл ```/bin/terraform``` на свою локальную машину, используя dive и docker save.
Предоставьте скриншоты  действий .
![image](https://github.com/user-attachments/assets/4ed3cbb1-74f8-4b6b-a167-9f7e035006b8)
![image](https://github.com/user-attachments/assets/adcbd467-e82c-47ef-a51a-937841042c06)
![image](https://github.com/user-attachments/assets/cfe3464c-8cc4-4ee6-bd95-c5ed1be8fd89)
![image](https://github.com/user-attachments/assets/27dccbd7-9ff5-4545-a7aa-c79deb006b9d)
![image](https://github.com/user-attachments/assets/14306aa6-66a1-4bbb-aa5b-8831cb059a26)


## Задача 6.1
Добейтесь аналогичного результата, используя docker cp.  
Предоставьте скриншоты  действий .
![image](https://github.com/user-attachments/assets/38a54788-645f-4b43-9654-653d33fa1f0f)

## Задача 6.2 (**)
Предложите способ извлечь файл из контейнера, используя только команду docker build и любой Dockerfile.  
Предоставьте скриншоты  действий .

## Задача 7 (***)
Запустите ваше python-приложение с помощью runC, не используя docker или containerd.  
Предоставьте скриншоты  действий .
