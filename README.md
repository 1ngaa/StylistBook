# StylistBook
## Описание.
Веб-приложение для записи к стилисту. Приложение должно предоставлять удобный интерфейс для пользователей, позволяя им просматривать услуги, выбирать дату и осуществлять запись к стилисту.

## Пользовательские сценарии.
1. Неавторизованный пользователь переходит на сайт, отображается главная страница со ссылкой на авторизацию через Telegram.
2. Авторизованный пользователь переходит на свой профиль, где отображается вся информация о предыдущих записях, включая дату, время, статус, а также кнопка, которая направляет на страницу с названием услуги, её описанием и возможностью записи. При нажатии на кнопку "записаться" происходит переход на страницу для выбора времени и доступной даты.
3. Администратор может войти в панель администратора, где доступны три раздела: "Пользователи", "Услуги" и "Записи". При выборе раздела "Пользователи" администратор может назначать администраторов и удалять пользователей. Переход в раздел "Услуги" позволяет администратору добавлять, редактировать и удалять услуги с их описанием. В разделе "Записи" администратор видит все записи и имеет возможность установить статус: "Отказано", "Подтверждено" или "В обработке".

## Технологический стек
#### Серверная часть:
* Языки программирования: PHP
* Система управления базами данных: MySQL.

#### Фронтенд:
* Языки программирования: JavaScript, HTML, CSS.
* Фреймворк: Bootstrap для улучшения дизайна и адаптивности.

#### Интеграция с мессенджером Telegram:
* Использование Telegram API для реализации системы авторизации.

## Интерфейс
![text](https://i.imgur.com/taROkUS.png)
## Диаграммы
![text](https://i.imgur.com/qyytp7o.jpg)
### Описание таблиц базы данных:
 База данных содержит три таблицы: appointments, users и uslugi.
##### Таблица appointments:
id (INT, AUTO_INCREMENT): уникальный идентификатор записи о встрече.
user_id (INT): идентификатор пользователя, который создал запись о встрече.
usluga_id (INT): идентификатор услуги, на которую записан пользователь.
usluga_name (VARCHAR(255)): название услуги.
date (DATE): дата встречи.
time (TIME): время встречи.
status (VARCHAR(32)): статус встречи (Confirmed, Processing, Denied).

##### Таблица users:
id (INT, AUTO_INCREMENT ): уникальный идентификатор пользователя.
first_name (VARCHAR(32)): имя пользователя.
last_name (VARCHAR(32)): фамилия пользователя.
telegram_id (VARCHAR(64)): идентификатор пользователя в Telegram.
telegram_username (VARCHAR(64)): юзернейм пользователя в Telegram.
profile_picture (VARCHAR(64)): ссылка на фото профиля пользователя.
auth_date (INT): дата авторизации пользователя.
is_admin (TINYINT): признак администратора (0 - не админ, 1 - админ).

##### Таблица uslugi:
id (INT, AUTO_INCREMENT): уникальный идентификатор услуги.
name (VARCHAR(64)): название услуги.
description (VARCHAR(255)): описание услуги.

## Документация
Фрагмент кода:
```
openapi: 3.0.0
info:
  title: StylistBook API
  description: API для взаимодействия с StylistBook
  license:
    name: Apache 2.0
    url: http://www.apache.org/licenses/LICENSE-2.0.html
  version: 0.0.0
servers:
  - url: http://127.0.0.1/
tags:
 - name: uslugi
    description: Услуги
  - name: admin
    description: админ-панель
 paths:
   /appointment/appointment/:
     get:
      tags:
        - uslugi
      summary: Получение информации из базы данных о конкретной услуги
      operationId: getUslugaInfo
      responses:
        '200':
          description: successful operation
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/usluga'
            application/xml:
              schema:
                $ref: '#/components/schemas/usluga'
     post:
      tags:
        - uslugi
      summary: Запись к стилисту
      operationId: appointmentUsluga
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/addAppointment'
          application/xml:
            schema:
              $ref: '#/components/schemas/addAppointment'
      responses:
        '200':
          description: successful operation
        '400':
          description: access denied
    /admin/users/{userID}:
     delete:
       tags:
        - admin
      summary: Удаление пользователя по его ID
      operationId: deleteUser
      parameters:
        - name: userID
          in: path
          description: ID пользователя
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: successful operation
        '400':
          description: access denied
components:
  schema:
     usluga:
      type: object
      properties:
        id:
          type: integer
          format: int64
          example: 1
        name:
          type: string
          example: Карта стиля
    addAppointment:
      type: object
      properties:
        user_id:
          type: integer
          format: int64
          example: 1234567
        usluga_id:
          type: integer
          format: int64
          example: 1
        date:
          type: string
          example: 2023-01-01
        time:
          type: string
          example: 12:40
```
Все методы:
![text](https://i.imgur.com/5hW3HdS.png)
## Развёртывание
1. Клонируйте репозиторий: 
git clone https://github.com/1ngaa/StylistBook.git
2. Импортируйте дамп базы данных
3. Настройте подключение к базе данных: 
Отредактируйте файл database/config.php и укажите параметры подключения к вашей базе данных.
4. Настройка авторизации через Telegram:
* Создайте нового бота у [@BotFather] в Telegram.
* Запомните токен, который вы получили от @BotFather.

## Скрины проекта
![text](https://i.imgur.com/w8mVZ1q.png)
![text](https://i.imgur.com/gG7jhIK.png)

## Вывод
В ходе выполнения курсовой работы успешно создано веб-приложение "StylistBook". Это приложение предназначено для упрощения процесса записи клиентов к стилистам, обеспечивая надежное взаимодействие между ними. Разработанная документация API и инструкции по развертыванию играют ключевую роль в поддержке бэкенда и фронтенда приложения. Проект фокусируется на создании эффективной, надежной и удобной системы, способствуя успешной работе стилистов и удовлетворению потребностей клиентов.  
