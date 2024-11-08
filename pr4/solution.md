### Задача 1
На сайте https://onlywei.github.io/explain-git-with-d3 или http://git-school.github.io/visualizing-git/ (цвета могут отличаться, есть команды undo/redo) с помощью команд эмулятора git получить следующее состояние проекта (сливаем master с first, перебазируем second на master): см. картинку ниже. Прислать свою картинку.

![Screenshot_35](https://github.com/user-attachments/assets/8eb40b37-ecb5-435e-9371-e6c1a72baf4b)

```bash
git commit
git tag in
git branch first
git branch second
git commit
git commit
git checkout first
git commit
git commit
git checkout master
git merge first
git checkout second
git commit
git commit
git rebase master
git checkout master
git merge second
git checkout in
```


### Задача 2
```bash
# Инициализация локального репозитория
git init my_project
cd my_project

# Установка имени и почты для первого пользователя (coder1)
git config user.name "Coder 1"
git config user.email "coder1@gmail.com"

# Создание файла prog.py с какими-то данными
nano prog.py
print('HI')

# Добавление файла в индекс
git add prog.py

# Создание коммита
git commit -m "new: added prog.py"
```

![photo_2024-11-08_13-43-34](https://github.com/user-attachments/assets/d2e23b51-ec21-4c6d-9c2e-2587e33ae45d)



### Задача 3
```bash
# Инициализация первого репозитория и настройка
git init
git config user.name "coder1"
git config user.email "coder1@example.com"
echo 'print("HI")' > prog.py
git add prog.py
git commit -m "first commit"

# Создание bare-репозитория
mkdir -p repository
cd repository
git init --bare server

# Возвращение в основной репозиторий, подключение к серверу и пуш
cd ..
git remote add server repository/server
git remote -v
git push server master

# Клонирование серверного репозитория в клиентский
git clone repository/server repository/client
cd repository/client
git config user.name "coder2"
git config user.email "coder2@gmail.com"

# Добавление нового файла и коммит
echo "Author Information:" > readme.md
git add readme.md
git commit -m "docs"

# Переименование удаленного репозитория и пуш
git remote rename origin server
git push server master

# Возвращение в основной репозиторий, чтобы сделать pull
cd ..
git pull server master --no-rebase  # Используем merge вместо rebase

# Внесение изменений от coder1 и пуш
echo "Author: coder1" >> readme.md
git add readme.md
git commit -m "coder1 info"
git push server master

# Переход в клиентский репозиторий и внесение изменений от coder2
cd client
echo "Author: coder2" >> readme.md
git add readme.md
git commit -m "coder2 info"

# Перед `push` выполняем `pull` с merge, чтобы избежать линейной истории
git pull server master --no-rebase
git push server master

# Получение последних изменений с сервера
git pull server master --no-rebase

# Последний коммит и пуш исправлений в readme
git add readme.md
git commit -m "readme fix"
git push server master

# Переход к bare-репозиторию и просмотр истории
cd ..
cd server
git log -n 5 --graph --decorate --all
```

![photo_2024-11-08_14-02-56](https://github.com/user-attachments/assets/6a909d5f-dd75-4393-906a-a5b69502cdbb)



