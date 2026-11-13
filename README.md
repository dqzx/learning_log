        🌟 Проект learning_log

Автономная учебная копия материалов по Python & Django.
Проект предназначен для локальной разработки, а также развёртывания на сервере Секунова (Sekunov).

        📁 Структура проекта

learning_log/
├── manage.py
├── Dockerfile
├── compose.yaml
├── .env.example  # Пример файла с переменными окружения
└── README.md
└── learning_log/   # Настройки проекта
│    ├── settings.py
│    └── ...
└── learning_logs/  # Приложение Learning Log
     ├── models.py
     ├── views.py
     └── ...
└── users/          # Приложение Users
     ├── forms.py
     ├── urls.py
     └── ...    
⚠️ Файл .env не должен попадать в репозиторий! Используйте файл .gitignore.


🛠 Установка и запуск: Локальная разработка

1. Создайте папку: (замените <фамилия> на вашу фамилию латиницей)
cd ~/Документы && mkdir -p <фамилия>/learning_log && cd $_ 

2. Создайте виртуальное окружение:
Linux/macOS:
python3.12 -m venv ll_env && source ll_env/bin/activate

Windows (PowerShell):
py -3.12 -m venv ll_env && .\ll_env\Scripts\Activate.ps1

3. Установите зависимости:
python -m pip install --upgrade pip
pip install -r requirements.txt

4. Запустите проект:
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
Откройте браузер по адресу http://localhost:8000 или http://127.0.0.1:8000.

🚢 Развёртывание на сервере Секунова
Подготовка файлов

Убедитесь, что у вас есть следующие файлы в корне проекта рядом с manage.py:

✅ Dockerfile
✅ compose.yaml
✅ .dockerignore
✅ .env.example (пример настроек — сам .env создавать не нужно!)

Файлы должны быть настроены следующим образом:
🗄️ Пример .env.example

(Этот файл нужен только как шаблон; реальный .env создаётся автоматически при деплое)
DJANGO_SECRET_KEY=replace-with-a-long-random-secret-key
DJANGO_DEBUG=False
DJANGO_ALLOWED_HOSTS=192.168.1.11,localhost,127.0.0.1
DJANGO_CSRF_TRUSTED_ORIGINS=http://192.168.1.11:${PORT1}
SQLITE_PATH=/app/data/db.sqlite3
APP_BIND_IP=0.0.0.0
APP_PORT=${PORT1} # Порт будет подставлен автоматически
Важно: Не указывайте номер порта руками (${PORT1}). Сервер выдаст вам диапазон портов, например от 20270 до 20279. Переменная $PORT1 должна соответствовать вашему выделенному порту.

🧩 docker-compose.yml
В секции ports: обязательно используйте переменную окружения:

ports:
  - "${APP_PORT}:8000"

  💡 Не пишите номер порта вручную. Это приведёт к ошибкам! Также не используйте параметр container_name. Имена контейнеров будут генерироваться автоматически с префиксом sekunov-learning-log-....

🏭 Инструкция по запуску на сервере

После получения доступа к серверу выполните команды:
cd /home/<student>/projects/sekunov-learning-log
docker compose down # Если ранее был запущен
docker compose up -d --build

    Ваше приложение станет доступно по ссылке вида: http://192.168.1.11:<ваш_порт> 
    Например: http://192.168.1.11:20270 
    Для проверки логов контейнера используйте:
    docker compose logs -f


🖋 Что должно быть в вашем репозитории?

Перед отправкой убедитесь, что в нём находятся:

Все исходные файлы приложения (learning_log, learning_logs, users)
Миграции базы данных
Dockerfile
compose.yaml
.dockerignore
.gitignore
README.md (этот файл)
requirements.txt (или аналог)
.env.example (шаблон без реальных значений)

❌ Запрещено коммитить: .env, базу данных (db.sqlite3), статические файлы (staticfiles/*) и любые секретные ключи.