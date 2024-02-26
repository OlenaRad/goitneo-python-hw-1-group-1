from collections import defaultdict
from datetime import datetime, timedelta

def get_birthdays_per_week(users):
    # Підготовка структури даних для зберігання імен користувачів по днях тижня
    birthdays_per_week = defaultdict(list)
    
    # Отримуємо поточну дату
    today = datetime.today().date()
    
    # Перебираємо користувачів
    for user in users:
        name = user["name"]
        birthday = user["birthday"].date()  # Конвертуємо дату народження до типу date
        
        # Визначаємо дату народження на поточний рік
        birthday_this_year = birthday.replace(year=today.year)
        
        # Якщо день народження вже пройшов цього року, переносимо його на наступний рік
        if birthday_this_year < today:
            birthday_this_year = birthday_this_year.replace(year=today.year + 1)
        
        # Визначаємо різницю між днем народження та поточною датою
        delta_days = (birthday_this_year - today).days
        
        # Визначаємо день тижня народження
        birthday_weekday = (today + timedelta(days=delta_days)).strftime("%A")
        
        # Якщо день народження вихідний, переносимо його на понеділок
        if delta_days >= 7:
            birthday_weekday = "Monday"
        
        # Зберігаємо ім'я користувача відповідно до дня тижня
        birthdays_per_week[birthday_weekday].append(name)
    
    # Виводимо результат
    for day, names in birthdays_per_week.items():
        print(f"{day}: {', '.join(names)}")

# Приклад використання:
users = [
    {"name": "Bill Gates", "birthday": datetime(1955, 10, 28)},
    {"name": "Jan Koum", "birthday": datetime(1976, 2, 24)},
    {"name": "Kim Kardashian", "birthday": datetime(1980, 10, 21)},
    {"name": "Jill Valentine", "birthday": datetime(1983, 5, 14)}
]

get_birthdays_per_week(users)
