# Домашнее задание к занятию 5. «Практическое применение Docker»

## Инструкция к выполнению

Для выполнения заданий обязательно ознакомьтесь с инструкцией по экономии облачных ресурсов. Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.
Своё решение к задачам оформите в вашем GitHub репозитории.
В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.
Сопроводите ответ необходимыми скриншотами.

## Примечание: Ознакомьтесь со схемой виртуального стенда по ссылке

## Задача 0

Убедитесь что у вас НЕ(!) установлен docker-compose, для этого получите следующую ошибку от команды docker-compose --version
Command 'docker-compose' not found, but can be installed with:

```
sudo snap install docker          # version 24.0.5, or
sudo apt  install docker-compose  # version 1.25.0-1

See 'snap info docker' for additional versions.
```

В случае наличия установленного в системе docker-compose - удалите его
2. Убедитесь что у вас УСТАНОВЛЕН docker compose(без тире) версии не менее v2.24.X, для это выполните команду docker compose version

## Своё решение к задачам оформите в вашем GitHub репозитории!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!*

## Ответ
![docker-compose--version.jpg](https://github.com/OshchepkovDP/devops-netology/blob/main/img/shvirtd-example-python/docker-compose--version.jpg)

## Задача 1

Сделайте в своем GitHub пространстве fork репозитория.

Создайте файл Dockerfile.python на основе существующего Dockerfile:

Используйте базовый образ python:3.12-slim
Обязательно используйте конструкцию COPY . . в Dockerfile
Создайте .dockerignore файл для исключения ненужных файлов
Используйте CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "5000"] для запуска
Протестируйте корректность сборки
(Необязательная часть, *) Изучите инструкцию в проекте и запустите web-приложение без использования docker, с помощью venv. (Mysql БД можно запустить в docker run).

(Необязательная часть, *) Изучите код приложения и добавьте управление названием таблицы через ENV переменную.

ВНИМАНИЕ!
!!! В процессе последующего выполнения ДЗ НЕ изменяйте содержимое файлов в fork-репозитории! Ваша задача ДОБАВИТЬ 5 файлов: Dockerfile.python, compose.yaml, .gitignore, .dockerignore,bash-скрипт. Если вам понадобилось внести иные изменения в проект - вы что-то делаете неверно!

## Ответ
[fork_репозитория](https://github.com/OshchepkovDP/shvirtd-example-python)
[Dockerfile.python](https://github.com/OshchepkovDP/shvirtd-example-python/blob/main/Dockerfile.python)
[.dockerignore](https://github.com/OshchepkovDP/shvirtd-example-python/blob/main/.dockerignore)

## Задача 2 (*)
Создайте в yandex cloud container registry с именем "test" с помощью "yc tool" . Инструкция
Настройте аутентификацию вашего локального docker в yandex container registry.
Соберите и залейте в него образ с python приложением из задания №1.
Просканируйте образ на уязвимости.
В качестве ответа приложите отчет сканирования.

## Ответ
![yc_fork.jpg](https://github.com/OshchepkovDP/devops-netology/blob/main/img/shvirtd-example-python/yc_fork.jpg)
![scan_report.json](https://github.com/OshchepkovDP/shvirtd-example-python/blob/main/scan_report.json)

## Задача 3
Изучите файл "proxy.yaml"
Создайте в репозитории с проектом файл compose.yaml. С помощью директивы "include" подключите к нему файл "proxy.yaml".
Опишите в файле compose.yaml следующие сервисы:
web. Образ приложения должен ИЛИ собираться при запуске compose из файла Dockerfile.python ИЛИ скачиваться из yandex cloud container registry(из задание №2 со *). Контейнер должен работать в bridge-сети с названием backend и иметь фиксированный ipv4-адрес 172.20.0.5. Сервис должен всегда перезапускаться в случае ошибок. Передайте необходимые ENV-переменные для подключения к Mysql базе данных по сетевому имени сервиса web

db. image=mysql:8. Контейнер должен работать в bridge-сети с названием backend и иметь фиксированный ipv4-адрес 172.20.0.10. Явно перезапуск сервиса в случае ошибок. Передайте необходимые ENV-переменные для создания: пароля root пользователя, создания базы данных, пользователя и пароля для web-приложения.Обязательно используйте уже существующий .env file для назначения секретных ENV-переменных!

Запустите проект локально с помощью docker compose , добейтесь его стабильной работы: команда curl -L http://127.0.0.1:8090 должна возвращать в качестве ответа время и локальный IP-адрес. Если сервисы не стартуют воспользуйтесь командами: docker ps -a  и docker logs <container_name> . Если вместо IP-адреса вы получаете информационную ошибку --убедитесь, что вы шлете запрос на порт 8090, а не 5000.

Подключитесь к БД mysql с помощью команды docker exec -ti <имя_контейнера> mysql -uroot -p<пароль root-пользователя>(обратите внимание что между ключем -u и логином root нет пробела. это важно!!! тоже самое с паролем) . Введите последовательно команды (не забываем в конце символ ; ): show databases; use <имя вашей базы данных(по-умолчанию example)>; show tables; SELECT * from requests LIMIT 10;.

Остановите проект. В качестве ответа приложите скриншот sql-запроса.
![sql-comand.jpg](https://github.com/OshchepkovDP/devops-netology/blob/main/img/shvirtd-example-python/sql-comand.jpg)

## Задача 4
Запустите в Yandex Cloud ВМ (вам хватит 2 Гб Ram).
Подключитесь к Вм по ssh и установите docker.
Напишите bash-скрипт, который скачает ваш fork-репозиторий в каталог /opt и запустит проект целиком.
Зайдите на сайт проверки http подключений, например(или аналогичный): https://check-host.net/check-http и запустите проверку вашего сервиса http://<внешний_IP-адрес_вашей_ВМ>:8090. Таким образом трафик будет направлен в ingress-proxy. Трафик должен пройти через цепочки: Пользователь → Internet → Nginx → HAProxy → FastAPI(запись в БД) → HAProxy → Nginx → Internet → Пользователь
(Необязательная часть) Дополнительно настройте remote ssh context к вашему серверу. Отобразите список контекстов и результат удаленного выполнения docker ps -a
Повторите SQL-запрос на сервере и приложите скриншот и ссылку на fork.

##Ответ:
[fork_yazndex-cloud](https://console.yandex.cloud/folders/b1g2426oq802iot2pt34/container-registry/registries/crpg11md2m361m0i9bog/overview/shvirtd-example-python/image)
![SQL_yandex_cloud.jpg](https://github.com/OshchepkovDP/devops-netology/blob/main/img/shvirtd-example-python/SQL_yandex_cloud.jpg)
![check-host.jpg](https://github.com/OshchepkovDP/devops-netology/blob/main/img/shvirtd-example-python/check-host.jpg)
#К сожалению, пункт с названием репозитория "test" я выполнил не верно, но не хотелось бы всё переделывать из-за такой мелочи, если это возможно.

Задача 5 (*)
Напишите и задеплойте на вашу облачную ВМ bash скрипт, который произведет резервное копирование БД mysql в директорию "/opt/backup" с помощью запуска в сети "backend" контейнера из образа schnitzler/mysqldump при помощи docker run ... команды. Подсказка: "документация образа."
Протестируйте ручной запуск
Настройте выполнение скрипта раз в 1 минуту через cron, crontab или systemctl timer. Придумайте способ не светить логин/пароль в git!!
Предоставьте скрипт, cron-task и скриншот с несколькими резервными копиями в "/opt/backup"
**пропустил это задание**


## Задача 6
Скачайте docker образ hashicorp/terraform:latest и скопируйте бинарный файл /bin/terraform на свою локальную машину, используя dive и docker save. Предоставьте скриншоты действий .

##Ответ:
![interface dive.jpg](https://github.com/OshchepkovDP/devops-netology/blob/main/img/shvirtd-example-python/interface%20dive.jpg)
![docker_pull_hashicorp.jpg](https://github.com/OshchepkovDP/devops-netology/blob/main/img/shvirtd-example-python/docker_pull_hashicorp.jpg)
![copy_terraform_localhost.jpg](https://github.com/OshchepkovDP/devops-netology/blob/main/img/shvirtd-example-python/copy_terraform_localhost.jpg)

## Задача 6.1
Добейтесь аналогичного результата, используя docker cp.
Предоставьте скриншоты действий .

## Ответ:
![docker_cp_terraform_localhost.jpg](https://github.com/OshchepkovDP/devops-netology/blob/main/img/shvirtd-example-python/docker_cp_terraform_localhost.jpg)


## Задача 6.2 (**)
Предложите способ извлечь файл из контейнера, используя только команду docker build и любой Dockerfile.
Предоставьте скриншоты действий .
**пропустил это задание**

## Задача 7 (***)
Запустите ваше python-приложение с помощью runC, не используя docker или containerd.
Предоставьте скриншоты действий .
**пропустил это задание**
