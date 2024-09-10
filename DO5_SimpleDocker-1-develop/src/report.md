## Part 1. Готовый докер
---
В качестве конечной цели своей небольшой практики вы сразу выбрали написание докер образа для собственного веб сервера, а потому в начале вам нужно разобраться с уже готовым докер образом для сервера.
Ваш выбор пал на довольно простой __nginx__.  

__= = Задание = =__  

#### Взять официальный докер образ с __nginx__ и выкачать его при помощи docker pull  

- Выполняем команду `docker pull nginx`  

- скрин с установкой образа:  
![docker](images/1_1.png)  

#### Проверить наличие докер образа через docker images  

- Выполняем команду `docker images`  

- скрин с выводом списка образов:  
![docker](images/1_2.png)  

#### Запустить докер образ через docker run -d [image_id|repository]  

- Выполняем команду `docker run -d nginx`  

- скрин подверждающий запуск контейнера:  
![docker](images/1_3.png)  

#### Проверить, что образ запустился через docker ps  

- Выполняем команду `docker ps`  

- скрин с выводом всех запущенных контейнеров:  
![docker](images/1_4.png)  

#### Посмотреть информацию о контейнере через docker inspect [container_id|container_name]  

- Выполняем команду `docker inspect b3664f0109d8`  

- скрин с информацией о контейнере:  
![docker](images/1_5.png)  

#### По выводу команды определить и поместить в отчёт размер контейнера, список замапленных портов и список замапленных портов  

- на скринах порт и ip адресс контейнера:  

![docker](images/1_6.png) 
![docker](images/1_6,5.png) 

- на скрине виден размер:  

![docker](images/1_2.png)  

- размер контейнера         - 187MB 
- список замапленных портов - "80/tcp": null
- список замапленных портов - 172.17.0.3  

#### Остановить докер образ через docker stop [container_id|container_name]  

- Выполняем команду `docker stop b3664f0109d8`  
![docker](images/1_7.png) 

#### Проверить, что образ остановился через docker ps  

- Выполняем команду `docker ps`
- ![docker](images/1_8.png)    

#### Запустить докер с замапленными портами 80 и 443 на локальную машину через команду run  

- Выполняем команду `docker run -d -p 80:80 -p 443:443 nginx` 

- скрин подверждающий запуск контейнера с замаплненными портами:  
![docker](images/1_9.png)  

#### Проверить, что в браузере по адресу localhost:80 доступна стартовая страница nginx 

- скрин с проверкой доступности странички по адресу localhost:80 в ОС:  
![docker](images/1_12.png)  

#### Перезапустить докер контейнер через docker restart [container_id|container_name]  

- Выполняем команду `docker restart`  

- скрин подверждающий выполнение перезапуска контейнера:  
![docker](images/1_11.png)  

#### Проверить любым способом, что контейнер запустился  

- Выполняем команду `curl localhost:80` 

- скрин с успешно отданным ответом:  
![docker](images/1_13.png)  


## Part 2. Операции с контейнером
---
Докер образ и контейнер готовы. Теперь можно покопаться в конфигурации __nginx__ и отобразить статус страницы.

__== Задание = =__

#### Прочитать конфигурационный файл nginx.conf внутри докер образа через команду exec  

- Выполняем команду:  
- `sudo docker exec 9bb7b5d44cle cat etc/nginx/nginx.conf`  

![docker](images/2_1.png)  

#### Создать на локальной машине файл nginx.conf  

- выполняем команду копирования файла nginx.conf из контейнера в хост:  
`sudo docker exec 9bb7b5d44cle cat etc/nginx/nginx.conf > src/data/nginx.conf`

#### Настроить в нем по пути */status* отдачу страницы статуса сервера __nginx__   

#### Скопировать созданный файл *nginx.conf* внутрь докер образа через команду docker cp  

- Выполняем команду `sudo docker cp data/nginx.conf 9bb7b5d44cle:etc/nginx/nginx.conf` 

#### Перезапустить nginx внутри докер образа через команду exec  

- Выполняем команду `sudo docker exec 9bb7b5d44cle nginx -s reload`

#### Проверить, что по адресу localhost:80/status отдается страничка со статусом сервера nginx  

- Выполняем команду `curl localhost:80/status`  

- скрин с копированием файла nginx.conf в контейнер, перезагрузкой контейнера и проверкой работоспособности странички /status:  
![docker](images/2_4.png)   

#### Экспортировать контейнер в файл *container.tar* через команду *export*

- Выполняем команду `sudo docker container export 9bb7b5d44cle > data/container.tar`
![docker](images/2_9.png)  
#### Остановить контейнер  

- Выполняем команду `sudo docker stop 9bb7b5d44cle`
![docker](images/2_7.png)  
#### Удалить образ, не удаляя перед этим контейнеры  

- Выполняем команду `sudo docker rmi -f [image_id|repository]`   

![docker](images/2_8.png) 

#### Импортировать контейнер обратно через команду *import*  

- Выполняем команду  `docker import data/container.tar cmonte_img` 

##### Запустить импортированный контейнер  

- `sudo docker run -itd --name imported -p 81:81 -d nginx`
- ![docker](images/2_11.png) 
- `docker exec 317319a483f8 service nginx reload`
![docker](images/2_14.png)  

- `curl localhost:80/status`  
![docker](images/2_16.png)
- `curl localhost:80` 
- ![docker](images/2_15.png)
