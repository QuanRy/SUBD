|   Лабораторная работа | ФИО     | *Samorokov* |
| :-------------: |:--------------|  -----:     |
|        №3       | Группа        | *IVT-363*   |
|       СУБД      | Преподаватель |             |
| `Магазин одежды`| Дата сдачи    |             |
<div id="header" align="center">
  <H1>Ход работы:</H1>
</div>

## Схема, полученная с помощью кода:
<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_2/schema_relative_lab_2.PNG" width="500"/>
  <p> Рисунок 1 – реляционная схема БД </p> 
</div>

##### Код приложения:
----------------------------------- Библиотеки -----------------------------------
##### import sqlite3
##### import time

----------------------------------- Cоединение с БД -----------------------------------
##### conn = sqlite3.connect('2_lab_users.db')
##### cursor = conn.cursor()

----------------------------------- Приветствие -----------------------------------
##### flag_cycle = 0
##### print ("")
##### for i in range (11):
    print('|||||||||', end='')
    time.sleep(0.1)
##### print("\n\n Подключение к БД установлено.\n")

------------------------- Работа с запросами к БД по выбору пользователя -------------------------

##### while flag_cycle != 1:
    print("   Выберите запрос, который хотите выполнить с БД для ПОЛЬЗОВАТЕЛЕЙ:\n")
    print(" 1. Показать споры с информацией о бренде товара, ФИО заказчика и причины спора.")
    print(" 2. Показать сообщения на форуме, написанных не позднее 30-ти дней назад.")
    print(" 3. Показать отзывы, авторы которых достигли совершеннолетия,не более 15-ти.")
    print(" 4. Добавить нового пользователя.")
    print(" 5. Обновить ФИО пользователя.")
    print(" 6. Удалить пользователя.")
    print(" 7. Выйти.")

    choice_querry = input("\n Ваш выбор: ")
    print()

    index = 1
    try:
        if choice_querry == "1":
            cursor.execute("SELECT disputes.date_time, disputes.message, goods.brand, users.FIO, cause_disputes.name \
                            FROM disputes JOIN goods on goods.id_good = disputes.id_good JOIN users on users.id_user = disputes.id_user \
                            JOIN cause_disputes on cause_disputes.id_cause_dispute = disputes.id_cause_dispute;")

            result1 = cursor.fetchall()

            print("Количество споров, выводимых на экран':  ", len(result1))
            for row in result1:
                print(str(index) + ".", end=' ')
                print(" дата: ", row[0], end=' ||| ')
                print(" Сообщение: ", row[1], end=' ||| ')
                print(" Бренд: ", row[2], end=' ||| ')
                print(" ФИО автора: ", row[3], end=' ||| ')
                print(" Причина открытия спора: ", row[4], end="\n")
                index += 1

        elif choice_querry == "2":
            cursor.execute("SELECT forum_message.date_time, forum_message.message, users.FIO, users.phone, users.role FROM forum_message \
                            JOIN users on users.id_user= forum_message.id_user WHERE date('now', '-30 days') <= date(date_time);")
            result2 = cursor.fetchall()

            print("Количество сообщений на форуме, выводимых на экран':  ", len(result2))
            for row in result2:
                print(str(index) + ".", end=' ')
                print("дата: ", row[0], end=' ||| ')
                print("Сообщение: ", row[1], end=' ||| ')
                print("ФИО автора: ", row[2], end=' ||| ')
                print("Телефон: ", row[3], end=' ||| ')
                print("Роль автора: ", row[4], end="\n")
                index += 1

        elif choice_querry == "3":
            cursor.execute("SELECT reviews.date_time, reviews.comment, users.FIO, users.phone, users.gender, users.date_birth \
                            FROM reviews JOIN users on users.id_user = reviews.id_user \
                            WHERE date('now') - date(users.date_birth) >= 18 ORDER BY reviews.date_time desc limit 15;")
            result3 = cursor.fetchall()

            print("Количество отзывов, выводимых на экран':  ", len(result3))

            for row in result3:
                print(str(index) + ".", end=' ')
                print("дата: ", row[0], end=' ||| ')
                print("Содержание отзыва: ", row[1], end=' ||| ')
                print("ФИО автора: ", row[2], end=' ||| ')
                print("Телефон: ", row[3], end=' ||| ')
                print("Пол автора: ", row[4], end=' ||| ')
                print("Дата рождения автора: ", row[5], end="\n")
                index+=1

        elif choice_querry == "4":
            phone = input(" Введите номер телефона: ")
            gender = input(" Введите пол (ж / м): ")
            FIO = input(" Введите ФИО подопечного: ")
            date_birth = input(" Введите дату рождения (в формате гггг-мм-дд): ")
            password = input(" Придумайте пароль: ")

            cursor.execute("INSERT INTO users (phone, gender, FIO, date_birth, password, role) VALUES(?, ?, ?, ?, ?, ?);", (phone, gender, FIO, date_birth, password, 0))
            result4 = cursor.fetchall()
            conn.commit()
            cursor.execute("SELECT id_user, phone, gender, FIO, date_birth, password, role FROM users;")
            result41 = cursor.fetchall()

            print("Все пользователи: \n")
            for row in result41:
                # print(str(index) + ".", end=' ')      # нумерация по порядку
                print("ID: ", row[0], end=' ||| ')
                print("Телефон: ", row[1], end=' ||| ')
                print("Пол: ", row[2], end=' ||| ')
                print("ФИО: ", row[3], end=' ||| ')
                print("Дата рождения: ", row[4], end=' ||| ')
                print("Роль: ", row[5], end="\n")
                index+=1

        elif choice_querry == "5":
            cursor.execute("SELECT * FROM users")
            result51 = cursor.fetchall()

            print("Все пользователи: \n")
            for row in result51:
                # print(str(index) + ".", end=' ')      # нумерация по порядку
                print("ID: ", row[0], end=' ||| ')
                print("Телефон: ", row[1], end=' ||| ')
                print("Пол: ", row[2], end=' ||| ')
                print("ФИО: ", row[3], end=' ||| ')
                print("Дата рождения: ", row[4], end=' ||| ')
                print("Роль: ", row[5], end="\n")
                index+=1

            old_FIO = input("\n Введите ФИО, которое хотите изменить: ")
            new_FIO = input(" Введите ФИО, НА которое хотите заменить: ")

            cursor.execute("UPDATE users SET FIO = ? WHERE FIO = ?;", (new_FIO, old_FIO))
            result5 = cursor.fetchall()
            conn.commit()
            cursor.execute("SELECT id_user, phone, gender, FIO, date_birth, password, role FROM users;")
            result51 = cursor.fetchall()

            print("Все пользователи: \n")
            for row in result51:
                # print(str(index) + ".", end=' ')      # нумерация по порядку
                print("ID: ", row[0], end=' ||| ')
                print("Телефон: ", row[1], end=' ||| ')
                print("Пол: ", row[2], end=' ||| ')
                print("ФИО: ", row[3], end=' ||| ')
                print("Дата рождения: ", row[4], end=' ||| ')
                print("Роль: ", row[5], end="\n")
                index+=1

        elif choice_querry == "6":
            cursor.execute("SELECT * FROM users")
            result61 = cursor.fetchall()

            print("Все пользователи: \n")
            for row in result61:
                # print(str(index) + ".", end=' ')      # нумерация по порядку
                print("ID: ", row[0], end=' ||| ')
                print("Телефон: ", row[1], end=' ||| ')
                print("Пол: ", row[2], end=' ||| ')
                print("ФИО: ", row[3], end=' ||| ')
                print("Дата рождения: ", row[4], end=' ||| ')
                print("Роль: ", row[5], end="\n")
                index+=1

            id_user = int(input("\n Введите ID пользователя, которого необходимо удалить: "))

            cursor.execute("DELETE FROM users WHERE users.id_user = '%s';" %id_user)
            result6 = cursor.fetchall()
            conn.commit()
            cursor.execute("SELECT id_user, phone, gender, FIO, date_birth, password, role FROM users;")
            result61 = cursor.fetchall()

            print("Все пользователи: \n")
            for row in result61:
                # print(str(index) + ".", end=' ')      # нумерация по порядку
                print("ID: ", row[0], end=' ||| ')
                print("Телефон: ", row[1], end=' ||| ')
                print("Пол: ", row[2], end=' ||| ')
                print("ФИО: ", row[3], end=' ||| ')
                print("Дата рождения: ", row[4], end=' ||| ')
                print("Роль: ", row[5], end="\n")
                index+=1

        elif choice_querry == "7":   # выход из приложения
            break

        else:
            print("Вы ввели что-то другое...\n")

    except sqlite3.Error as error:
        print("Ошибка при работе с SQLite", error)   # на случай ошибок

    answer = input("\n\n Хотите продолжить работу в приложении? \n (1 - да / другой символ - нет)? \n Ваш ответ: ")
    if answer == "1":
        flag_cycle = 0

    elif answer != "1":
        print(" Программа успешно завершила свою работу...\n")
        flag_cycle = 1

------------------------------ Закрытие соединения с БД ------------------------------
##### conn.close()

## Пример работы приложения:
<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_3/1_welcome_window.JPG" width="500"/>
  <p> Рисунок 2 – Начальное окно приложения </p> 
</div>

<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_3/2_1_request.JPG" width="500"/>
  <p> Рисунок 3 – Результат 1-го запроса («Показать споры с информацией о бренде товара, ФИО заказчика, причины спора») </p> 
</div>

<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_3/3_2_request.JPG" width="500"/>
  <p> Рисунок 4 – Результат 2-го запроса («Показать сообщения на форуме, написанных не более 30-ти дней назад») </p> 
</div>

<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_3/3_3_request.JPG" width="500"/>
  <p> Рисунок 5 – Результат 3-го запроса («Показать не более 15-ти отзывов, авторы которых достигли совершеннолетия») </p> 
</div>

## Пример работы приложения:

<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_3/3_4_2_request.JPG" width="500"/>
  <p> Рисунок 6 – Результат 4-го запроса («Добавить нового пользователя») </p> 
</div>

<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_3/3_5_1_request.JPG" width="500"/>
  <p> Рисунок 7 – Начало 5-го запроса («Обновить ФИО пользователя») </p> 
</div>

<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_3/3_5_2_request.JPG" width="500"/>
  <p> Рисунок 8 – Результат 5-го запроса («Обновить ФИО пользователя») </p> 
</div>

<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_3/3_6_1_request.JPG" width="500"/>
  <p> Рисунок 9 – Начало 6-го запроса («Удалить пользователя») </p> 
</div>

<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Samorokov-(QuanRy)/images/lab_3/3_6_1_request.JPG" width="500"/>
  <p> Рисунок 10 – Результат 6-го запроса («Удалить пользователя») </p> 
</div>
