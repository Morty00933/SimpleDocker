Docker Practice Project

Этот проект представляет собой практическую работу по освоению Docker, включающую настройку веб-сервера nginx, создание собственного мини-сервера на C с использованием FastCGI, написание собственного Docker-образа, проверку его безопасности с помощью Dockle и настройку развертывания через Docker Compose.


🚀 Описание проекта

Проект разделен на 6 частей, каждая из которых решает конкретные задачи:

Готовый докер – Работа с официальным образом nginx (запуск, проверка, настройка портов).

Операции с контейнером – Настройка конфигурации nginx для отображения страницы статуса.

Мини веб-сервер – Создание простого сервера на C с использованием FastCGI.

Свой докер – Написание и сборка собственного Docker-образа для мини-сервера и nginx.

Dockle – Проверка и исправление Docker-образа на безопасность.

Docker Compose – Развертывание проекта с использованием нескольких контейнеров.


📌 Требования

✅ Установленный Docker и Docker Compose.
✅ Утилита Dockle для проверки безопасности (опционально, для части 5).
✅ Компилятор gcc и библиотека libfcgi-dev для сборки мини-сервера.
✅ Локальная машина с доступом к портам 80, 81, 443, 8080.

🛠 Установка и запуск

🔹 Часть 1: Готовый докер

Выкачать образ nginx:

docker pull nginx

Проверить наличие образа:

docker images

Запустить контейнер:

docker run -d nginx

📌 Подробные шаги и скриншоты команд находятся в отчете.

🔹 Часть 2: Операции с контейнером

Скопировать и настроить nginx.conf для отображения страницы /status.

Перезапустить nginx внутри контейнера:

docker exec [container_id] nginx -s reload

Проверить страницу http://localhost:80/status.

🔹 Часть 3: Мини веб-сервер

Скомпилировать сервер:

gcc src/server.c -o server -lfcgi

Запустить через spawn-fcgi:

spawn-fcgi -p 8080 ./server

Настроить проксирование в nginx/nginx.conf с порта 81 на 8080.

🔹 Часть 4: Свой докер

Собрать образ:

docker build -t my-nginx-server:1.0 .

Запустить с маппингом портов и папки:

docker run -d -p 80:81 -v $(pwd)/nginx:/etc/nginx/conf.d my-nginx-server:1.0

Проверить http://localhost:80 и http://localhost:80/status.

🔹 Часть 5: Dockle

Проверить образ:

dockle my-nginx-server:1.0

Исправить ошибки (например, добавить .dockerignore, убрать лишние зависимости).

🔹 Часть 6: Docker Compose

Запустить проект:

docker-compose up --build

📜 Лицензия

Этот проект распространяется под лицензией MIT. Свободно используйте, изменяйте и распространяйте!


