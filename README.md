# Разворачивание решения

### **Шаг 1: Клонирование репозитория или подготовка проекта**

Клонируйте репозитории Git:

```commandline
git clone https://github.com/Dimacing/MuseumReviews.git
```

Перейдите в директорию с проектом:

```commandline
cd MuseumReviews
```

### **Шаг 2: Создание виртуального окружения**

Необходимо создать виртуальное окружение для изоляции зависимостей проекта.

```commandline
python3 -m venv venv
```
если не создается на Windows, то попробовать

```commandline
py -m venv venv
```

Активация виртуального окружения:

- На Windows:
    ```commandline
    venv\Scripts\activate
    ```

- На macOS и Linux:
    ```commandline
    source venv/bin/activate
    ```

### **Шаг 3: Установка зависимостей из requirements.txt**

Для установки зависимостей необходимо выполнить следующую команду (загрузка зависимостей займет время):

```commandline
pip install --upgrade pip
pip install -r requirements.txt
```

Эта процедура установит все необходимые библиотеки.



