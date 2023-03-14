|   Лабораторная работа | ФИО     | *Samorokov* |
| :-------------: |:--------------|  -----:     |
|        №2       | Группа        | *IVT-363*   |
|       СУБД      | Преподаватель |             |
| `Магазин одежды`| Дата сдачи    |             |
<div id="header" align="center">
  <H1>Ход работы:</H1>
</div>

## Схема, полученная с помощью кода:
<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_2/schema_relative_lab_2.PNG" width="500"/>
  <p> Рисунок 1 – реляционная схема БД, полученная при создании БД в SQLite </p> 
</div>

## Схема, полученная на этапе разработки:
<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_2/schema_relative_lab_1.PNG" width="500"/>
  <p> Рисунок 2 – реляционная схема БД, разработанная в ходе 1-ой л.р. </p> 
</div>

<div id="header" align="center">
  <H1>Код создания БД:</H1>
</div>

## Основные таблицы:
##### CREATE TABLE "users" (
##### 	"id_user"	INTEGER NOT NULL,
##### 	"phone"	NUMERIC NOT NULL,
##### 	"gender"	INTEGER NOT NULL,
##### 	"FIO"	TEXT NOT NULL,
##### 	"date_birth"	TEXT NOT NULL,
##### 	"password"	TEXT NOT NULL,
##### 	"role"	INTEGER NOT NULL,
##### 	PRIMARY KEY("id_user" AUTOINCREMENT)
##### );
##### CREATE TABLE "goods" (
##### 	"id_good"	INTEGER NOT NULL,
##### 	"brand"	TEXT NOT NULL,
##### 	"color"	TEXT NOT NULL,
##### 	"material"	TEXT NOT NULL,
##### 	"type_clothes"	TEXT NOT NULL,
##### 	"guarantee"	TEXT NOT NULL,
##### 	PRIMARY KEY("id_good" AUTOINCREMENT)
##### );
##### CREATE TABLE "reviews" (
##### 	"id_review"	INTEGER NOT NULL,
##### 	"date_time"	TEXT NOT NULL,
##### 	"comment"	TEXT NOT NULL,
##### 	"id_user"	INTEGER NOT NULL,
##### 	FOREIGN KEY("id_user") REFERENCES "users"("id_user") ON DELETE CASCADE,
##### 	PRIMARY KEY("id_review" AUTOINCREMENT)
##### );
##### CREATE TABLE "cause_disputes" (
##### 	"id_cause_dispute"	INTEGER NOT NULL,
##### 	"name"	TEXT NOT NULL,
##### 	PRIMARY KEY("id_cause_dispute" AUTOINCREMENT)
##### );
##### CREATE TABLE "disputes" (
##### 	"id_dispute"	INTEGER NOT NULL,
##### 	"date_time"	TEXT NOT NULL,
##### 	"message"	TEXT NOT NULL,
##### 	"id_good"	INTEGER NOT NULL,
##### 	"id_user"	INTEGER NOT NULL,
##### 	"id_cause_dispute"	INTEGER NOT NULL,
##### 	PRIMARY KEY("id_dispute" AUTOINCREMENT),
##### 	FOREIGN KEY("id_user") REFERENCES "users"("id_user") ON DELETE CASCADE,
##### 	FOREIGN KEY("id_good") REFERENCES "goods"("id_good") ON DELETE CASCADE,
##### 	FOREIGN KEY("id_cause_dispute") REFERENCES "cause_disputes"("id_cause_dispute") ON DELETE CASCADE
##### );
##### CREATE TABLE "warehouses" (
##### 	"id-warehouse"	INTEGER NOT NULL,
##### 	"name"	TEXT NOT NULL,
##### 	"address"	TEXT NOT NULL,
##### 	"quantity"	TEXT NOT NULL,
##### 	PRIMARY KEY("id-warehouse" AUTOINCREMENT)
##### );
##### CREATE TABLE "sales" (
##### 	"id_sale"	INTEGER NOT NULL,
##### 	"name"	TEXT NOT NULL,
##### 	"date_time_start"	TEXT NOT NULL,
##### 	"date_time_end"	TEXT NOT NULL,
##### 	"conditions"	TEXT NOT NULL,
##### 	PRIMARY KEY("id_sale" AUTOINCREMENT)
##### );
##### CREATE TABLE "forum_message" (
##### 	"id_forum_message"	INTEGER NOT NULL,
##### 	"date_time"	TEXT NOT NULL,
##### 	"message"	TEXT NOT NULL,
##### 	"id_user"	INTEGER NOT NULL,
##### 	PRIMARY KEY("id_forum_message" AUTOINCREMENT),
##### 	FOREIGN KEY("id_user") REFERENCES "users"("id_user") ON DELETE CASCADE
##### );
## Таблицы связи (многие-ко-многим):
##### CREATE TABLE "sales_to_goods" (
##### 	"id_sale"	INTEGER NOT NULL,
##### 	"id_good"	INTEGER NOT NULL,
##### 	FOREIGN KEY("id_good") REFERENCES "goods"("id_good") ON DELETE CASCADE,
##### 	FOREIGN KEY("id_sale") REFERENCES "sales"("id_sale") ON DELETE CASCADE
##### );
##### CREATE TABLE "goods_to_warehouses" (
##### 	"id_good"	INTEGER NOT NULL,
##### 	"id_warehouse"	INTEGER NOT NULL,
##### 	FOREIGN KEY("id_goos") REFERENCES "goods"("id_good") ON DELETE CASCADE,
##### 	FOREIGN KEY("id_warehouse") REFERENCES "warehouses"("id-warehouse") ON ##### DELETE CASCADE
##### );
##### CREATE TABLE "goods_to_user" (
##### 	"id_good"	INTEGER NOT NULL,
##### 	"id_user"	INTEGER NOT NULL,
##### 	FOREIGN KEY("id_good") REFERENCES "goods"("id_good") ON DELETE CASCADE,
##### 	FOREIGN KEY("id_user") REFERENCES "users"("id_user") ON DELETE CASCADE
##### );
## 
## Пример выполнения запросов:
### Показать споры с информацией о бренде товара, ФИО заказчика и причины спора
##### SELECT disputes.date_time, disputes.message, goods.brand, users.FIO, cause_disputes.name FROM disputes
#####  JOIN goods on goods.id_good = disputes.id_good
#####  JOIN users on users.id_user = disputes.id_user
#####  	JOIN cause_disputes on cause_disputes.id_cause_dispute = disputes.id_cause_dispute;


<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_2/1_join_request.PNG" width="500"/>
</div>

### Показать сообщения на форуме с указанием роли пользователя (является ли он обычным пользователем или администратором), его ФИО и номера телефона, дата написания которых не позднее 30-ти дней (т.е. не прошло месяца с написания сообщения на форуме)
##### SELECT forum_message.date_time, forum_message.message, users.FIO, users.phone, users.role FROM forum_message
#####  JOIN users on users.id_user= forum_message.id_user
#####  WHERE date('now', '-30 days') <= date(date_time);
<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_2/2_join_request.PNG" width="500"/>
</div>

### Показать отзывы с полной информации об авторе, авторов, которые уже достигли совершеннолетия, отсортировать по убыванию написания отзыва (чем дальше, тем старше отзывы), вывести максимум 15 штук
##### SELECT reviews.date_time, reviews.comment, users.FIO, users.phone, users.gender, users.date_birth FROM reviews
##### 	JOIN users on users.id_user = reviews.id_user
##### 	WHERE date('now') - date(users.date_birth) >= 18
##### 	ORDER BY reviews.date_time desc limit 15;
<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_2/3_join_request.PNG" width="500"/>
</div>

## CRUD
<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_2/4_start_users_table.PNG" width="500"/>
  <p> Рисунок 3 – Начальная таблица "users" до внесения изменения с помощью CRUD </p> 
</div>

### INSERT
##### INSERT INTO users (phone, gender, FIO, date_birth, password, role) VALUES('79270381220', 'ж', 'Абрамова Ксения Андреевна', '1989-08-26', 'f1434;fgfa', '0'); 
<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_2/5_result_insert.PNG" width="500"/>
</div>

### UPDATE
##### UPDATE users SET FIO = 'Скалова Ксения Андреевна' WHERE FIO = 'Абрамова Ксения Андреевна';
<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_2/6_result_update.PNG" width="500"/>
</div>

### DELETE
##### DELETE from users where users.id_user = 11 and users.FIO = 'Иванов Иван Иванов';
<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_2/7_result_delete.PNG" width="500"/>
    <p> Рисунок 4 – Конечная таблица "users" после внесения правок с помощью CRUD </p> 
</div>
