### Docker

cmd> docker - общая команда просмотра функций докера
cmd> docker version - общая команда просмотра функций докера

Install docker extension in VScode
hub.docker.com - это склад с готовыми образами

# Containers & Images
Images - это образы (шаблоны), из них составляются контейнеры. Они только для чтения, нельзя изменить
Container - включает в себе набор образов, необходим для их чтения. Это место для запуска приложения.

# Images
cmd> docker pull node -установить образ (пакет) для работы с nodeJS
cmd> docker images -просмотреть установленные на ПК образы
cmd> docker image prune -удалить все неиспользуемые образы кроме базовых
cmd> docker rmi 78074855188 -удалить конкретный образ
cmd> docker tag imagename newimagename -копирование с переименованием

cmd> docker push grigorym/repositoryname -загрузка образа на docker hub
cmd> docker pull grigorym/repositoryname -скачивание образа с docker hub

cmd> docker image inspect -просмотр информации об образе



# Containers
cmd> docker ps --help -флаг help показывает описание команды ps
cmd> docker ps -a -посмотреть список всех контейнеров

cmd> docker run node -зарегистрировать контейнер на основании образа с node (можно прописать id)
cmd> docker run -it node -запустить контейнер на основании образа с node и работать внутри контейнера
cmd> docker start f78074855188 -запустить контейнер в фоновом режиме
cmd> docker stop f78074855188 -остановка контейнера
cmd> .exit -выход из контейнера

cmd> docker rm 8f8ec6254606 -удалить созданный контейнер
cmd> docker container prune -удалить все остановленные контейнеры




# App
В корневой директории приложения создать файл Dockerfile

Это файл с инструкциями, на основании которого будет создан контейнер
    FROM node -на основании базового образа

    WORKDIR /app -указывается рабочая директория

    COPY package.json /app -если будут изменения то перезапись, если нет то просто обновление кода

    RUN npm install -собирает образ

    COPY . . -копируется вся папка с проектом в корень образа

    ENV PORT 4200 -задается системная переменная порт

    EXPOSE $PORT -ссылка на порт, на котором будет запуск на образе

    VOLUME ["/app/data"] -указывается директива с временными файлами для сохранения в volumes

    CMD ["npm", "start"] -запускает образ

app> docker build . -запуск процесса создания образа приложения
app> docker build -t appname:2.0 . -создания образа приложения с именем и версией


# Main commands
app> docker run -p 3000:3000 1609600fc0c7 -запуск контейнера на основании образа(id) на порт 3000 локально и порт 3000 в образе приложения
app> docker run -d -p 3000:3000 1609600fc0c7 -аналогично но без погружения в консоль приложения в образе
    --name logsapp -можно задать свое имя для контейнера
    --rm -после остановки сразу удаляется из списка сгенерированных
    -e PORT=3000 -задаются значения переменных .env !или:
    -env-file ./config/.env
    -v name:/app/data -указание имени volume и ссылки на директиву в проекте

app> docker attach 1609600fc0c7 -перейти в консоль запущенного контейнера (не работает как надо)


# Volumes
app> docker run -d -p 3000:3000
    -v name:/app/data -указание имени volume и ссылки на директиву в проекте

app> docker volume rm volume-name -удалить конкретный volume
app> docker volume prune -удалить все volumes
app> docker volume create name -создать volume с именем name



пример:
docker run -d -p 80:4200 --rm --name logsapp -v logs:/app/data logsapp:volumes
docker stop logsapp
docker run -d -p 80:4200 --rm --name logsapp -v logs:/app/data logsapp:volumes
и в новом контейнере используются сохраненные данные из volume logs



пример режима разработки:
docker run -d -p 80:4200 --rm --name logsapp -v "D:\DEVELOPMENT\Docker\app:/app" -v /app/node_modules -v logs:/app/data logsapp:volumes


# Compose
docker-compose build -построить образ из образов в файле docker-compose.yml
docker-compose up -запустить