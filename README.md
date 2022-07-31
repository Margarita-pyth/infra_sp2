## Сервис YaMDb

Проект YaMDb собирает отзывы пользователей на произведения. 
Произведения делятся на категории: «Книги», «Фильмы», «Музыка». 
Список категорий может быть расширен администратором.

- ✨Magic ✨Python

## Технологии
- Django rest_framework
- Django rest_framework_simplejwt
- Django django_filters
- Git
- Docker
- NGINX
- GUNICORN
- POSTGRES

## Как запустить проект:

Клонировать репозиторий:
```sh
git clone git@github.com:Margarita-pyth/infra_sp2.git
```

### Перейдите в директорию infra_sp2\infra и создайте файл .env:
#### Шаблон наполнения файла:
```sh
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=username # логин для подключения к базе данных
POSTGRES_PASSWORD=password # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД 
```

## Для запуска приложения в контейнерах перейдите в директорию "infra_sp2\infra" и выполните команды:

```sh
docker-compose up -d --build 
```

Для создания суперюзера выполинте команду:
```sh
docker-compose exec web python manage.py createsuperuser
```

#### Теперь проект готов к работе и доступен по адресу http://localhost/api/v1.
#### И также доступен доступ к админке: http://localhost/admin/login/?next=/admin/.

Чтобы сделать резервную копию базы данных выполните команду из директории infra_sp2\infra:
```sh
docker-compose exec web python manage.py dumpdata > fixtures.json
```

Чтобы скопировать файл  базы данных в контейнер выполните команду из директории infra_sp2/infra:
```sh
docker cp fixtures.json <id>:app/
```

И подгрузите данные БД из директории infra\docker-compose.yaml:
```sh
docker-compose exec web python manage.py loaddata fixtures.json
```

#### Руководство к проекту:
##### Пользовательские роли
 - Аноним — может просматривать описания произведений, читать отзывы и комментарии.
 - Аутентифицированный пользователь (user) — может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому новому пользователю.
 - Модератор (moderator) — те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.
 - Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
 - Суперюзер Django должен всегда обладать правами администратора, пользователя с правами admin. Даже если изменить пользовательскую роль суперюзера — это не лишит его прав администратора. 

### Алгоритм регистрации пользователей
 - Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт /api/v1/auth/signup/.
 - Сервис YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на указанный адрес email.
 - Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).
 - В результате пользователь получает токен и может работать с API проекта, отправляя этот токен с каждым запросом.
 
### Создание пользователя администратором
 - Пользователя может создать администратор — через админ-зону сайта или через POST-запрос на специальный эндпоинт api/v1/users/ (описание полей запроса для этого случая — в документации). 
 - - В этот момент письмо с кодом подтверждения пользователю отправлять не нужно.
После этого пользователь должен самостоятельно отправить свой email и username на эндпоинт /api/v1/auth/signup/ , в ответ ему должно прийти письмо с кодом подтверждения.
 - Далее пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен), как и при самостоятельной регистрации.

 ## Ресурсы сервиса API YaMDb
- auth: аутентификация.
- users: пользователи.
- titles: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).
- categories: категории (типы) произведений («Фильмы», «Книги», «Музыка»).
- genres: жанры произведений. Одно произведение может быть привязано к нескольким жанрам.
- reviews: отзывы на произведения. Отзыв привязан к определённому произведению.
- comments: комментарии к отзывам. Комментарий привязан к определённому отзыву.

## License
**Подготовлено командой разработчиков: 

evgenlit,
Aleksandr-Fedotov,
Margarita-pyth
**
___________________________________________________________________________________________

## YaMDb Service

The YaMDb project collects user feedback on works.
The works are divided into categories: "Books", "Films", "Music".
The list of categories can be expanded by the administrator.

- ✨Magic ✨Python

## Technology
- Django rest_framework
- Django rest_framework_simplejwt
- Django django_filters
-Git
-Docker
- nginx
- GUNICORN
- POSTGRESS

## How to run the project:

Clone repository:
```sh
git clone git@github.com:margarita-pyth/infra_sp2.git
```
### Change to the infra_sp2\infra directory and create an .env file:
#### File filling pattern:
```sh
DB_ENGINE=django.db.backends.postgresql # indicate that we are working with postgresql
DB_NAME=postgres # database name
POSTGRES_USER=username # login to connect to the database
POSTGRES_PASSWORD=password # password to connect to the database (set your own)
DB_HOST=db # service (container) name
DB_PORT=5432 # port for connecting to the database
```

## To run the application in containers, go to the "infra_sp2\infra" directory and run the commands:

```sh
docker-compose up -d --build
```

To create a superuser, run the command:
```sh
docker-compose exec web python manage.py createsuperuser
```

#### The project is now ready to go and available at http://localhost/api/v1.
#### And access to the admin panel is also available: http://localhost/admin/login/?next=/admin/.

To backup the database, run the following command from the infra_sp2\infra directory:
```sh
docker-compose exec web python manage.py dumpdata > fixtures.json
```

To copy the database file to the container, run the command from the infra_sp2/infra directory:
```sh
docker cp fixtures.json <id>:app/
```

And load the database data from the infra\docker-compose.yaml directory:
```sh
docker-compose exec web python manage.py loaddata fixtures.json
```

#### Project Guide:
##### User roles
- Anonymous — can view descriptions of works, read reviews and comments.
 - Authenticated user (user) — can read everything, as well as Anonymous, can publish reviews and rate works (films / books / songs), can comment on reviews; can edit and delete their reviews and comments, edit their ratings of works. This role is assigned by default to each new user.
 - Moderator — the same rights as an Authenticated User, plus the right to delete and edit any reviews and comments.
 - Administrator (admin) — full rights to manage all the content of the project. Can create and delete works, categories and genres. Can assign roles to users.
 - The Django superuser must always have administrator rights, a user with admin rights. Even if you change the user role of the superuser, it will not deprive him of administrator rights. 

 ### User registration algorithm
 - The user sends a POST request with the email and username parameters to the endpoint /api/v1/auth/signup/.
 - The YaMDB service sends an email with a confirmation code (confirmation_code) to the specified email address.
 - The user sends a POST request with the username and confirmation_code parameters to the endpoint /api/v1/auth/token/, in response to the request he receives a token (JWT token).
 - As a result, the user receives a token and can work with the project API by sending this token with each request.

 ### Creating a user by an administrator
 - The user can be created by an administrator — through the site's admin zone or through a POST request to a special api endpoint/v1/users/ (the description of the request fields for this case is in the documentation). 
 - - At this moment, the user does not need to send an email with a confirmation code.
After that, the user must independently send his email and username to the endpoint /api/v1/auth/signup/, in response he should receive an email with a confirmation code.
 - Next, the user sends a POST request with the username and confirmation_code parameters to the endpoint /api/v1/auth/token/, in response to the request, he receives a token (JWT token), as with self-registration.

 ## YaMDb
- auth API service resources: authentication.
- users: users.
- titles: works to which reviews are written (a certain movie, book or song).
- categories: categories (types) of works ("Movies", "Books", "Music").
- genres: genres of works. One work can be linked to several genres.
- reviews: reviews of works. The review is tied to a specific work.
- comments: comments on reviews. The comment is linked to a specific review.

## License
**Prepared by the development team: 

evgenlit,
Aleksandr-Fedotov,
Margarita-pyth
**
