# Kittygram - социальная сеть для котиков

**Это социальная сеть для обмена фотографиями любимых питомцев - это полная рабочая платформа, разработанная с использованием Django в качестве бэкенд-фреймворка и React в качестве фронтенд-библиотеки.**


**Бэкенд-приложение на Django предоставляет RESTful API для взаимодействия с базой данных, обработки запросов от клиентов и выполнения бизнес-логики. Здесь реализованы модели данных для пользователей, питомцев, фотографий и других связанных сущностей. Также бэкенд предоставляет эндпоинты для регистрации и аутентификации пользователей, добавления новых питомцев и загрузки фотографий.**


**Фронтенд-приложение на React взаимодействует с бэкендом через API, получая данные и отправляя запросы на создание, обновление и удаление объектов. На фронтенде реализованы страницы для просмотра, добавления и редактирования фотографий и профилей питомцев.**


**Вся система обеспечивает безопасность данных пользователей, используя механизм аутентификации и авторизации Django. Она также поддерживает масштабирование благодаря использованию Django ORM для работы с базой данных и React-компонентной архитектуры для удобного добавления новых функций и страниц.** 


**Этот проект может быть запущен на сервере или на локальной машине после установки всех зависимостей. В дополнение к тому, это открытый исходный код, поэтому вы можете адаптировать его под свои потребности или вносить свои изменения.**

# Технологии 
### 1. Frontend:
- Node.js
- React

\* Полный список библиотек в файле packeje.json

### 2. Backend:
- Django
- DRF
- Gunicorn
- Pillow

\* Полный список библиотек в файле requirements.txt

### 3. Сервер:
- nginx

# Развертывание 

1. Откройте терминал и перейдите в скрытую директорию .ssh — из любой директории выполните команду:

```
cd ~/.ssh
```

2. В директории .ssh создайте новую директорию для SSH-ключей. Выполните команду:
```
mkdir название-директории
```

3. Перенесите файл SSH-ключa в только что созданную директорию
4. Установите права доступа к файлам SSH-ключей с помощью команд:
```
chmod 600 название_файла_закрытого_SSH-ключа
```
5. Введите команду:
```
ssh -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом login@ip
```
6. Обновлиту список доступных пакетов:
```
sudo apt update
```
7. проверьте установлен ли Git:
```
git --version
```

если нет то установите: 
```
sudo apt install git
```
8. Сгенерируйте SSH ключ и добавьте на GitHub:
```
ssh-keygen
```
9. Сделайте Fork и клонируйте репозиторий: 
```
git clone git@github.com:Ваш_аккаунт/infra_sprint1.git 
```
10. Далее установите на сервер пакетный менеджер и утилиту для создания виртуального окружения. Сделать это можно одной командой, перечислив все необходимые к установке пакеты:
```
sudo apt install python3-pip python3-venv -y
```
11. Далее начинайте работать с зависимостями. Перейдите в директорию с бэкенд-приложением проекта, создайте и активируйте виртуальное окружение, установите пакеты из файла requirements.txt:
```
# Переходим в директорию backend-приложения проекта.
cd infra_sprint1/backend/
# Создаём виртуальное окружение.
python3 -m venv venv
# Активируем виртуальное окружение.
source venv/bin/activate
# Возвращаемся назад 
cd ..
# Устанавливаем зависимости.
pip install -r requirements.txt
```
12. И заключительный шаг перед запуском бэкенд-приложения — выполните миграции и создайте суперюзера, чтобы протестировать работу панели администратора. Для этого из директории, в которой находится файл manage.py, выполните команды:
```
# Применяем миграции.
python manage.py migrate
# Создаём суперпользователя.
python manage.py createsuperuser
```
13. На удалённом сервере при активированном виртуальном окружении проекта  установите пакет gunicorn:
```
pip install gunicorn==20.1.0
```
14. В директории /etc/systemd/system/ создайте файл gunicorn.service и откройте его в Nano
```
sudo nano /etc/systemd/system/gunicorn.service 
```
15. Подставьте в код из листинга свои данные, добавьте этот код без комментариев в файл gunicorn.service и сохраните изменения.
```
[Unit]
# Это текстовое описание юнита, пояснение для разработчика.
Description=gunicorn daemon 

# Условие: при старте операционной системы запускать процесс только после того, 
# как операционная система загрузится и настроит подключение к сети.
# Ссылка на документацию с возможными вариантами значений 
# https://systemd.io/NETWORK_ONLINE/
After=network.target 

[Service]
# От чьего имени будет происходить запуск:
# укажите имя, под которым вы подключались к серверу.
User=<имя_пользователя> 

# Путь к директории проекта:
# /home/<имя-пользователя-в-системе>/
# <директория-с-проектом>/<директория-с-файлом-manage.py>/.
# Например:
WorkingDirectory=/home/<имя_пользователя>/infra_sprint1/backend/

# Команду, которую вы запускали руками, теперь будет запускать systemd:
# /home/<имя-пользователя-в-системе>/
# <директория-с-проектом>/<путь-до-gunicorn-в-виртуальном-окружении> --bind 0.0.0.0:8000 kittygram_backend.wsgi
ExecStart=/home/<имя_пользователя>/infra_sprint1/backend/venv/bin/gunicorn --bind 0.0.0.0:8000 kittygram_backend.wsgi

[Install]
# В этом параметре указывается вариант запуска процесса.
# Значение <multi-user.target> указывают, чтобы systemd запустил процесс,
# доступный всем пользователям и без графического интерфейса.
WantedBy=multi-user.target
```
16. Запустите процесс gunicorn.service:
```
sudo systemctl start gunicorn
```
17. Дополнительной командой добавьте процесс Gunicorn в список автозапуска операционной системы на удалённом сервере:
```
sudo systemctl enable gunicorn
```
18. Установка Nginx:
```
sudo systemctl start nginx
```
19. Укажите файрволу, какие порты должны остаться открытыми. Для этого выполните на сервере две команды по очереди:
```
sudo ufw allow 'Nginx Full'
sudo ufw allow OpenSSH
```
20. Теперь включите файрвол:
```
sudo ufw enable
```
21. Перейдите в директорию infra_sprint1/frontend и выполните команду:
```
npm run build
```
22. Чтобы Nginx раздавал статику, он должен знать, где она лежит. У веб-сервера есть системная директория, которую он использует по умолчанию для доступа к статическим файлам, — /var/www/. Скопируйте в эту директорию содержимое папки .../frontend/build/:
```
# Команда cp — копировать, ключ -r — рекурсивно, включая вложенные папки и файлы.
sudo cp -r /home/yc-user/taski/frontend/build/. /var/www/kittygram/ 
# Точка после build важна — будет скопировано содержимое директории.
```
23. Через редактор Nano откройте файл конфигурации веб-сервера:
```
sudo nano /etc/nginx/sites-enabled/default
```
24. Удалите все настройки из файла, запишите и сохраните новые:
```
server {

    listen 80;
    server_name публичный_ip_вашего_удалённого_сервера;

    # Новый блок.
    location /api/ {
        # Эта команда определяет, куда нужно перенаправить запрос.
        proxy_pass http://127.0.0.1:8000;
    }

    location / {
        root   /var/www/infra_sprint1;
        index  index.html index.htm;
        try_files $uri /index.html;
    }

} 
```
25. Сохраните изменения, проверьте и перезагрузите конфигурацию веб-сервера:
```
sudo nginx -t
sudo systemctl reload nginx
```
26. Чтобы подготовить бэкенд-приложение для сбора статики, в файле settings.py укажите директорию, куда эту статику нужно сложить. 
Через редактор Nano откройте файл settings.py, укажите новое значение для константы STATIC_URL и создайте константу STATIC_ROOT:
```
# Замените стандартное значение 'static' на 'static_backend',
# чтобы не было конфликта запросов к статике фронтенда и бэкенда.
STATIC_URL = 'static_backend'
# Укажите директорию, куда бэкенд-приложение должно сложить статику.
STATIC_ROOT = BASE_DIR / 'static_backend'
# Укажите по какому адресу будет делаться запрос
MEDIA_URL = '/media/'
# Укажите директорию, куда бэкенд-приложение должно сложить картинки.
MEDIA_ROOT = '/var/www/kittygram/media' 
```
27. Теперь можно собрать статику бэкенд-приложения:
сохраните изменения и закройте файл,
на удалённом сервере при активированном виртуальном окружении перейдите в директорию с файлом manage.py и выполните команду:
```
python manage.py collectstatic
```
28. Перейдите в корень проекта Kittygram и скопируйте директорию static_backend/ в директорию /var/www/infra_sprint1/. Для этого выполните команду:
```
sudo cp -r /home/yc-user/infra_sprint1/backend/static_backend/ /var/www/kittygram/
```
29. Чтобы изменения в файле settings.py вступили в силу, перезапустите Gunicorn:
```
sudo systemctl restart gunicorn
```
30. Находясь на сервере, перейдите в директорию /taski/backend/backend, с помощью Nano откройте файл settings.py и добавьте в список ALLOWED_HOSTS полученное доменное имя:
```
ALLOWED_HOSTS = ['xxx.xxx.xxx.xxx', '127.0.0.1', 'localhost', 'ваш-домен'] 
# Вместо xxx.xxx.xxx.xxx — IP вашего сервера.
# Домен вводится в формате 'project.hopto.org'.
```
31. Сохраните изменения и перезапустите Gunicorn командой:
```
sudo systemctl restart gunicorn
```
32. Измените конфигурационный файл Nginx:
```
sudo nano /etc/nginx/sites-enabled/default
```
33. Перезагрузите nginx:
```
sudo systemctl reload nginx
```
34. Устновите ssl-сертификаты: 
[Ссылка на certbot](https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal) 

