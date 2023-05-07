
|   Лабораторная работа | ФИО     | *Novruzov* |
| :-------------: |:--------------|  -----:     |
|        №4       | Группа        | *IVT-363*   |
|       СУБД      | Преподаватель |             |
| `Магазин одежды`| Дата сдачи    |             |
<div id="header" align="center">
  <H1>Ход работы:</H1>
</div>

import pymongo
from datetime import datetime



# Establish a connection to MongoDB database
myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["forum_shop"]
mycol = mydb["user"]
mycol2 = mydb["forum"]
mycol3 = mydb["review"]



# Create operation
def create_user():
    phone = input("Enter user phone: ")
    email = input("Enter user email: ")
    gender = input("Enter user gender (male/female): ")
    FIO = input("Enter user FIO: ")
    date_birth = input("Enter user date of birth (YYYY-MM-DD): ")
    password = input("Enter user password: ")
    role = input("Enter user role (user/moderator/admin): ")



    user = {
        "phone": phone,
        "email": email,
        "gender": gender,
        "FIO": FIO,
        "date_birth": date_birth,
        "password": password,
        "role": role
    }



    mycol.insert_one(user)
    print("User created successfully!")



# Read operation
def read_user():
    phone = input("Enter user phone to read: ")
    user = mycol.find_one({"phone": phone})
    
    if user:
        print(f"Phone: {user['phone']}")
        print(f"Email: {user['email']}")
        print(f"Gender: {user['gender']}")
        print(f"FIO: {user['FIO']}")
        print(f"Date of Birth: {user['date_birth']}")
        print(f"Role: {user['role']}")
    else:
        print("User not found!")



# Update operation
def update_user():
    phone = input("Enter user phone to update: ")
    user = mycol.find_one({"phone": phone})



    if user:
        print(f"Phone: {user['phone']}")
        print(f"Email: {user['email']}")
        print(f"Gender: {user['gender']}")
        print(f"FIO: {user['FIO']}")
        print(f"Date of Birth: {user['date_birth']}")
        print(f"Role: {user['role']}")



        print("Enter new values (leave blank to keep current value):")
        email = input(f"Email ({user['email']}): ") or user['email']
        gender = input(f"Gender ({user['gender']}): ") or user['gender']
        FIO = input(f"FIO ({user['FIO']}): ") or user['FIO']
        date_birth = input(f"Date of Birth ({user['date_birth']}): ") or user['date_birth']
        password = input(f"Password: ") or user['password']
        role = input(f"Role ({user['role']}): ") or user['role']



        myquery = { "phone": phone }
        newvalues = { "$set": {
            "email": email,
            "gender": gender,
            "FIO": FIO,
            "date_birth": date_birth,
            "password": password
        }}
        mycol.update_one(myquery, newvalues)



        print("User updated successfully!")
    else:
        print("User not found!")



# Delete operation
def delete_user():
    phone = input("Enter user phone to delete: ")
    user = mycol.find_one({"phone": phone})



    if user:
        mycol.delete_one({"phone": phone})
        print("User deleted successfully!")
    else:
        print("User not found!")



# Создание форума
def create_forum():
    name = input("Enter forum name: ")
    description = input("Enter forum description: ")



    forum = {
        "name": name,
        "description": description,
    }



    mycol2.insert_one(forum)
    print("Forum created successfully!")



# Получение списка всех форумов
def read_forum():
    name = input("Enter forum name to read: ")
    forum = mycol2.find_one({"name": name})



    if forum:
        print(f"Name: {forum['name']}")
        print(f"Description: {forum['description']}")
    else:
        print("Forum not found!")



# Обновление данных форума по имени
def update_forum():
    name = input("Enter forum name to update: ")
    forum = mycol2.find_one({"name": name})



    if forum:
        print(f"Name: {forum['name']}")
        print(f"Description: {forum['description']}")



        print("Enter new values (leave blank to keep current value):")
        description = input(f"Description ({forum['description']}): ") or forum['description']



        myquery = { "name": name }
        newvalues = { "$set": {
            "description": description
        }}
        mycol2.update_one(myquery, newvalues)



        print("Forum updated successfully!")
    else:
        print("Forum not found!")



# Удаление форума по имени
def delete_forum():
    name = input("Enter forum name to delete: ")
    forum = mycol2.find_one({"name": name})
    if forum:
        mycol2.delete_one({"name": name})
        print("Forum deleted successfully!")
    else:
        print("Forum not found!")




def get_average_rating_each_products():
    pipeline = [
        {
            "$group": {
                "_id": "$product",
                "average_rating": {"$avg": "$rating"}
            }
        }
    ]



    # Выполняем запрос
    results = mycol3.aggregate(pipeline)



    # Печатаем результаты запроса
    for result in results:
        print(result)




def get_users_between_dates():
    start_date = input("Enter start date (YYYY-MM-DD): ")
    end_date = input("Enter end date (YYYY-MM-DD): ")



    start_date = datetime.strptime(start_date, '%Y-%m-%d')
    end_date = datetime.strptime(end_date, '%Y-%m-%d')
    
    pipeline = [
        {
            "$match": {
                "date_birth": {
                    "$gte": start_date,
                    "$lt": end_date
                }
            }
        },
        {
            "$count": "total_users"
        }
    ]



    # Выполняем запрос
    results = mycol.aggregate(pipeline)



    # Печатаем результаты запроса
    for result in results:
        print(result)




# Menu interface
while True:
    print("\nMenu:")
    print("1 - Create user")
    print("2 - Read user")
    print("3 - Update user")
    print("4 - Delete user")
    print("5 - Create forum")
    print("6 - Read forum")
    print("7 - Update forum")
    print("8 - Delete forum")
    print("9 - Avarage ratings products")
    print("10 - Get users form interval")
    print("0 - Exit")



    choice = input("\nEnter your choice: ")



    if choice == "1":
        create_user()
    elif choice == "2":
        read_user()
    elif choice == "3":
        update_user()
    elif choice == "4":
        delete_user()
    elif choice == "5":
        create_forum()
    elif choice == "6":
        read_forum()
    elif choice == "7":
        update_forum()
    elif choice == "8":
        delete_forum()
    elif choice == "9":
        get_average_rating_each_products()
    elif choice == "10":
        get_users_between_dates()
    elif choice == "0":
        break
    else:
        print("Invalid choice!")
