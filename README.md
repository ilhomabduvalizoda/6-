Импорт библиотек: import tkinter as tk из tkinter import messagebox импортировать sqlite3

Здесь импортируются необходимые модули: tkinter для создания графического интерфейса, messagebox для отображения сообщений пользователю и sqlite3 для работы с базой данных.

Создание базы данных и таблицы пользователей: def create_db(): conn = sqlite3.connect('users.db') cursor = conn.cursor() cursor.execute(''' CREATE TABLE IF NOT EXISTS users ( id INTEGER PRIMARY KEY, username TEXT UNIQUE, password TEXT ) ''') conn.commit() conn.close()

Эта функция создает базу данных users.db и таблицу users, если они еще не существуют. В таблице есть три поля: id (первичный ключ), username (уникальное имя пользователя) и password (пароль).

Регистрация нового пользователя: def register_user(имя_пользователя, пароль): conn = sqlite3.connect('users.db') cursor = conn.cursor() try: cursor.execute('INSERT INTO users (имя_пользователя, пароль) VALUES (?, ?)', (имя_пользователя, пароль)) conn.commit() messagebox.showinfo("Успех", "Пользователь успешно зарегистрирован!") except sqlite3.Ошибка целостности: messagebox.showerror("Ошибка", "Пользователь с таким именем уже существует.") наконец: conn.close()

Функция принимает логин и пароль пользователя и пытается сохранить их в базе данных.

Если имя пользователя уже существует, выводится сообщение об ошибке.
В случае успешной регистрации выводится сообщение об успешном завершении.
Авторизация пользователя: def authorize_user(имя_пользователя, пароль): conn = sqlite3.connect('users.db') cursor = conn.cursor() cursor.execute('SELECT * FROM users WHERE username=? AND password=?', (имя_пользователя, пароль)) user = cursor.fetchone() conn.close()

if user:
    messagebox.showinfo("Успех", "Вы успешно авторизованы!")
else:
    messagebox.showerror("Ошибка", "Неверный логин или пароль.")
Эта функция проверяет, существует ли пользователь с указанными логином и паролем.

Если пользователь найден, появится сообщение об успешной авторизации.
В противном случае отобразится ошибка.
Окно регистрации: def open_registration_window(): reg_window = tk.Toplevel(root) reg_window.title("Регистрация")

tk.Label(reg_window, text="Логин").pack(pady=5)
username_entry = tk.Entry(reg_window)
username_entry.pack(pady=5)

tk.Label(reg_window, text="Пароль").pack(pady=5)
password_entry = tk.Entry(reg_window, show='*')
password_entry.pack(pady=5)

tk.Button(reg_window, text="Регистрация",
          command=lambda: register_user(username_entry.get(), password_entry.get())).pack(pady=10)
Функция создает новое окно для регистрации.

Пользователь может ввести логин и пароль, а затем нажать кнопку для регистрации, которая вызовет функцию register_user.
Основное окно: root = tk.Tk() root.title("Авторизация")

create_db()

tk.Label(корневой каталог, текст = «Логин»).pack(отступ = 5) entry_username = tk.Entry(корневой каталог) entry_username.pack(отступ = 5)

tk.Label(корневой каталог, текст = «Пароль»).pack(отступ = 5) entry_password = tk.Entry(корневой каталог, показать = «*») entry_password.pack(отступ = 5)

tk.Button(root, text="Авторизация", command=lambda: authorize_user(entry_username.get(), entry_password.get())).pack(pady=10) tk.Button(root, text="Регистрация", command=open_registration_window).pack(pady=5)

root.mainloop()

Здесь создается основное окно приложения.
