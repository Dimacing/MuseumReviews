# Оценка тональности отзывов о музее (Умный городской гид)

Описание файлов:
- data_analysis.ipynb - анализ собранных отзывов и предобработка данных
- model_training.ipynb - дообучение модели и оценка результатов
- telegram_bot.py - реализация телеграм бота (парсинг сайта, анализ данных, поведение бота, api запросы к GigaChat)

Введение: 
Современные музеи активно работают над улучшением качества обслуживания, стремясь предоставлять посетителям комфортные условия для знакомства с культурными ценностями. Однако однако одной из ключевых проблем, с которым сталкиваются такие учреждения, — это анализ отзывов посетителей. Отзывы часто содержат важные замечания, но их анализ вручную затруднен из-за большого объема текстов и субъективности восприятия. В настоящее время использование машинного обучения, связанных с обработкой  текста является важным инструментом для решения подобных задач. 


Актуальность:
1. Улучшение пользовательского опыта в музеях.  
   Современные музеи стремятся не только сохранять культурное наследие, но и предоставлять своим посетителям комфортные и приятные условия. Анализ отзывов позволяет выявить ключевые проблемы, такие как неудобства в навигации, состояние экспонатов или высокую стоимость билетов, и своевременно реагировать на них.  
2. Большие объемы данных.  
   В условиях цифровизации посетители музеев активно оставляют отзывы на платформах, таких как TripAdvisor, Яндекс.Карты и Google Reviews. Однако ручная обработка таких данных становится практически невозможной из-за их огромного объема и разнородности.  
3. Автоматизация рутинных процессов.  
   Автоматический анализ отзывов помогает музеям экономить ресурсы, минимизируя затраты на обработку данных вручную. Это позволяет учреждениям фокусироваться на реализации выявленных рекомендаций.  
4. Выявление ключевых тенденций и проблем.  
   Использование современных технологий NLP, таких как дообученные языковые модели, дает возможность не только классифицировать отзывы по тональности, но и выделять ключевые темы, что помогает музеям сосредотачиваться на наиболее актуальных для посетителей аспектах.  
5. Применимость к другим культурным учреждениям.  
   Разработанные инструменты могут быть масштабированы и использованы для анализа отзывов других культурных объектов, таких как театры, выставочные залы и галереи, что делает их универсальными и полезными для всей отрасли.  

Цель проекта:  
Получить модель, которая будет определять положительный или отрицательный был оставлен отзыв о музее.

Постановка задач: 
1. Выгрузка данных с помощью парсинга сайта.
2. Проивзести разметку отзывов на 0(отрицательный) и 1(положительный).
3. Проанализировать полученный датасет с размеченными данными.
4. Дообучить датасет с использованием готовой модели.

# Данные  
Источником данных являлся сайт, с которого были взяты отзывы. Для этого был написан скрипт для выгрузки отзывов: 1.py.  
Была произведена разметка тональности: 0 - негативный, 1 - позитивный.  

В итоге структура данных:  
- Индекс (столбец Index).
- Текст отзыва (столбец Review).  
- Метка тональности (столбец label): 0 - негативный, 1 - позитивный.  

Объем данных:  
- 1470 записей для обучения и тестирования.

# Выбор модели  
cointegrated/rubert-tiny-sentiment-balanced была выбрана по нескольким основным причинам:   
- Адаптация к русскоязычным текстам.  
Модель RuBERT-tiny-sentiment-balanced была специально разработана для анализа тональности текстов на русском языке.      
- Готовность к дообучению.  
rubert-tiny-sentiment-balanced позволяет легко адаптироваться под конкретные задачи.  

# Подход к реализации  

После этого было произведенно разделение данных: обучающая выборка (80%) и тестовая выборка (20%) для оценки качества модели.  

Для начала была произведена балансировка классов с использованием техники undersampling для предотвращения доминирования одного класса. (resample(positive, replace=False, n_samples=len(negative), random_state=42)).

Далее был использован встроенный метод токенизации AutoTokenizer, обеспечивающий: Padding — выравнивание длины текстов, Truncation — усечение текстов, превышающих максимальную длину.  

После было произведенно дообучение модели с выбранными оптимальными гиперпараметрами: скорость обучения (5e-5), размер батча (16), число эпох (3), использование регуляризации (weight decay = 0.01).  

Также были использованы инструменты: библиотека Hugging Face Transformers для тренировки модели, класс Trainer для упрощения настройки обучения и оценки.  

После этого сохраняется данная модель и далее используется.

  
# Результаты  
Модель 1 (cointegrated/rubert-tiny-sentiment-balanced)
Accuracy: 0.73 
F1-score: 0.79
 
Модель 2 (дообучение cointegrated/rubert-tiny-sentiment-balanced)
Accuracy: 0.91
F1-score: 0.91 
 
# Телеграм-бот
Для удобства использования был разработан бот, который автоматически определяет, является ли введенный отзыв положительным или отрицательным.

# Возможные области применения
Telegram-бот для определения тональности отзывов имеет широкий спектр применений, особенно в музейной сфере. Вот несколько возможных применений:
1. Мониторинг качества обслуживания
   - Анализировать отзывы, чтобы выявить основные недостатки (например, плохая организация выставки, очереди) или сильные стороны (интересные экспонаты, доброжелательный персонал).
2. Оценка новых инициатив
   - Использовать отзывы для оценки реакции на новые выставки, интерактивные зоны, экскурсии или цифровые сервисы.
3. Сегментация данных
   - Разделение отзывов на положительные и отрицательные для углубленного анализа, выделения часто упоминаемых слов, поиска трендов и выявления типичных тем.
 4. Поддержка оперативного реагирования
   - Настраивать уведомления о негативных отзывах для своевременного реагирования со стороны сотрудников музея.
5. Статистика и отчеты
   - Формировать сводные отчеты о динамике отзывов: рост или спад положительных оценок за период, что помогает следить за общей удовлетворенностью посетителей.
6. Обучение персонала
   - Использовать отзывы для определения областей, где сотрудникам нужно улучшить навыки, например, вежливость, точность консультаций и т. д.
7. Разработка новых стратегий
   - Обоснованное принятие решений на основе данных: стоит ли вводить скидки, улучшать навигацию, добавлять интерактивные элементы и т. д.
8. Сравнительный анализ
   - Сравнивать реакцию посетителей на разные музеи, выставки или сервисы, если бот используется в нескольких учреждениях.
9. Интеграция с CRM
   - Собирать данные из бота и связывать их с системой управления клиентами (CRM) для дальнейшей персонализации взаимодействия.
 10. Маркетинговые исследования
   - Выявление факторов, влияющих на положительные и отрицательные отзывы, для оптимизации рекламных кампаний.

# Бизнес-модель проекта
Целевая аудитория: музеи, галереи, культурные центры, разработчики маркетинговых решений.  

Платформа анализа данных
- Подписка для музеев и культурных учреждений, предоставляющая доступ к инструментам анализа отзывов, включая полярность, частотный анализ тем и динамику мнений.  
- Продажа API для интеграции в системы CRM, платформы управления отзывами и маркетинговые аналитические инструменты.  

Бот-помощник для музеев
Функционал:  
- Автоматическая классификация отзывов на положительные и отрицательные.  
- Выявление ключевых тем и тенденций в отзывах посетителей.  
- Уведомления о негативных отзывах с рекомендациями для реакции.  

Монетизация через:  
- Лицензирование бота для использования музеями и галереями.  
- Интеграция в туристические платформы для анализа отзывов о достопримечательностях.  
- Сотрудничество с компаниями, занимающимися улучшением клиентского опыта, для предоставления аналитических данных.  

Особенности
- Поддержка локализации и учета особенностей языка для разных стран.  
- Соблюдение законов о защите данных посетителей.  
- Возможность кастомизации под запросы конкретного учреждения.  


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



