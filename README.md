# D1.6
Учебное задание курсов Fullstack Python от Skillfactory

В течение данного модуля надо выполнить следующие задания:

* Создать проект Django.
* Добавить в него 3 статические странички.
* На одной из страниц контент повторяется 2 раза без изменения content (два раза прописано {{ flatpage.content }}).
* Одна из страниц на сайте доступна только админу (только вошедшему пользователю).
* На одной из страниц изменены шрифты и размеры текста.
* Сайт представляет собой оформленный Bootstrap-шаблон со встроенными пользовательскими данными.
Статические файлы Bootstrap загружаются через теги {% load static %} и {% static %}.

============================================


* Создаём проект виртуальным окружением, расшариваем его в github
* Открываем терминал, и
> pip install django

* После установки Django создаём пустой проект:
> django-admin startproject D1_6

* Для настройки базы данных существуют специальные файлы, называемые миграциями, для их применения:
> python manage.py migrate

* После того как наша база данных заработала (и появилось место для хранения информации), создаём первого администратора командой:
> python manage.py createsuperuser

* Для запуска сервера используем команду в консоли:
> python manage.py runserver

* Сайт смотрим на странице http://localhost:8000/
* Настраиваем flatpages:
  * Редактирование settyngs.py:
    * В INSTALLED_APPS подключаем ещё приложения:
    ['django.contrib.sites', # Приложение, позволяющее разделять страницы между несколькими сайтами
    'django.contrib.flatpages',]
    * И добавляем переменную для указания сайта для приложения sites:
      `SITE_ID = 1`
	  * Добавляем в переменную MIDDLEWARE<br>
      `['django.contrib.flatpages.middleware.FlatpageFallbackMiddleware' ]`
	  * Чтобы шаблоны нормально искались, в переменной TEMPLATES 
		  * Добавляем импорт <br>
        `import os.path`
		  * в качестве путей добавляем os.path.join(BASE_DIR, 'templates'):<br>
	      `'DIRS': [os.path.join(BASE_DIR, 'templates')],`
  * Редактирование urls.py:
    * Чтобы использовать include для страниц, автоматически создаваемых с помощью flatpages, меняем импорт 
      `from django.urls import include, path`
	  * И добавляем в переменную urlpatterns
      `[path('pages/', include('django.contrib.flatpages.urls')),]`
* У нас появились новые миграции, их вносят в базу данных:
> python manage.py migrate

* Чтобы использовать flatpages, в директории с файлом manage.py нужно создать файл по следующему пути:
> mkdir -p templates/flatpages/
> > templates/flatpages/default.html

* Скачиваем шаблон bootstrap в архиве startbootstrap-bare-gh-pages.zip, и распаковываем этот архив в папку ./static, которую мы создадим в той же самой папке, где файл manage.py

* Добавляем в файл settyngs.py:
`STATICFILES_DIRS = [
    BASE_DIR / "static"
]`
* Переносим содержимое файла index.html в default.html, и редактируем получившийся файл:
  * В заголовке `<head>...</head>`:
	  * Удаляем иконку
	  * Делаем подгрузку стилей с помощью `{% static 'css/styles.css' %}` -
		  * включаем команду {% staic '....'%}, вставив в заголовок шаблона {% load static %}
		  * меняем `<link href="css/styles.css" rel="stylesheet" />`  на `<link href="{% static 'css/styles.css' %}" rel="stylesheet" />`
  * В теле html удаляем ссылки на javascript
* Создаём в админке странички с адресами "/doublecontent/", "/registeredonly/" и "/", на "/" будет изменённый шрифт
