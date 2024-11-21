# Введение

В рамках моей тестовой работы для демонстрации своих навыков в области ручного тестирования, я решил создать простую форму регистрации и разместить её на платформе Glitch. Эта платформа позволила мне быстро настроить как клиентскую, так и серверную части, что дало возможность провести комплексные API тестирования.

Целью данного проекта было не только протестировать форму регистрации с точки зрения функциональности и интерфейса, но и проверить работу серверной части через API. Я создал сервер, который обрабатывает запросы, и реализовал все необходимые проверки, включая обработку ошибок, валидацию данных и взаимодействие с фронтендом.

В ходе выполнения задания я составил детальный тест-план, чек-листы и тест-кейсы, охватывающие как позитивные, так и негативные сценарии. Также были проведены кросс-браузерные тестирования и тесты на безопасность (например, защита от XSS-атак).

## Проект включает:

- Веб-форму регистрации с полями для имени, email, пароля и подтверждения пароля.
- Валидацию данных на клиентской и серверной сторонах.
- Реализацию серверных API для обработки данных.
- API тестирование с помощью Postman для проверки корректности работы серверных запросов и ответов.

Я подготовил все необходимые тесты для проверки как функциональности, так и безопасности формы регистрации. Этот проект стал отличной возможностью показать мой опыт в тестировании веб-приложений, включая работу с API, а также понимание принципов безопасности и качества пользовательского интерфейса.

# Форма регистрации
Этот сайт представляет собой простую веб-форму регистрации пользователей, с использованием HTML, CSS и JavaScript. Он взаимодействует с серверной частью для обработки регистрации, отправляя данные на сервер через API

![mind](registrForm.png)
- URL проекта: https://dot-tropical-ocicat.glitch.me

## HTML-структура:
![mind](1.png)
- Форма регистрации состоит из четырёх полей: имя, email, пароль и подтверждение пароля.
- Каждое поле сопровождается элементом <span> для отображения ошибок валидации, если пользователь введет неверные данные.
- Кнопка отправки формы вызывает функцию validateForm(), которая проверяет корректность введённых данных.

## CSS-стили:
![mind](css.png)

Стили для формы регистрации задают простое и удобное оформление:
- Все элементы формы, включая поля ввода и кнопки, занимают всю ширину контейнера.
- Ошибки валидации выделяются красным цветом.
- Кнопка оформления заявки имеет фон синего цвета, который меняется при наведении.

## JavaScript-валидация и отправка данных на сервер:
![mind](java_1.png)
![mind](java_2.png)

validateForm():
1. Очистка предыдущих ошибок: перед проверкой данных удаляются все предыдущие сообщения об ошибках.
2. Валидация:
- Проверяется, что имя не пустое.
- Для email проверяется, чтобы он был корректно отформатирован.
- Пароль проверяется на минимальную длину (8 символов).
- Пароли должны совпадать.
3. Если форма прошла валидацию, данные отправляются на сервер с помощью fetch(). Сервер принимает данные в формате JSON и, в зависимости от результата, показывает сообщение об успехе или ошибке.

## API-соединение:
![mind](api.png)

POST-запрос отправляется на сервер, который обработает данные регистрации. Адрес сервера указывается как https://mirror-spiced-brian.glitch.me/. Данные отправляются в формате JSON.

# Серверная часть
## Импорт зависимостей:
![mind](Serv_1.png)

Здесь импортируются необходимые зависимости:

- express: используется для создания серверных маршрутов и обработки HTTP-запросов.
- body-parser: модуль для парсинга тела запроса, в частности JSON, что необходимо для работы с данными, отправляемыми в POST-запросах.
- sqlite3: модуль для работы с базой данных SQLite, который позволяет использовать SQL-запросы для сохранения и извлечения данных.
- cors: модуль, который разрешает кросс-доменные запросы, что важно, если клиентская часть и сервер находятся на разных доменах.

## Создание экземпляра Express и настройки:
![mind](Serv_2.png)

- app — это экземпляр Express, который будет использоваться для создания серверных маршрутов.
- cors({ origin: "*" }): разрешает доступ к серверу с любых доменов. Это полезно для разработки, но в реальных условиях нужно указать конкретные домены.
- bodyParser.json(): парсит входящие JSON-запросы, позволяя извлекать данные из тела запроса.

## Создание базы данных и таблицы:
![mind](Serv_3.png)

- Создаётся база данных SQLite с именем users.db. Если база уже существует, устанавливается соединение с ней.
- db.run(...): создаёт таблицу users, если она ещё не существует. В таблице хранятся данные пользователей: id (уникальный идентификатор), name (имя пользователя), email (электронная почта) и password (пароль).

## Маршрут для регистрации:
![mind](Serv_4.png)

1. Этот маршрут обрабатывает POST-запросы по адресу /register. Он принимает данные от клиента: name, email, password и confirmPassword.
2. Применяется валидация:
- Проверяется наличие имени.
- Проверяется формат email с помощью регулярного выражения.
- Проверяется длина пароля (не менее 8 символов).
- Проверяется совпадение паролей.
3. Если все проверки проходят успешно, данные пользователя сохраняются в базе данных SQLite. В случае успеха сервер возвращает сообщение о том, что регистрация прошла успешно. Если возникла ошибка при добавлении данных в базу, отправляется ошибка.

## Маршрут для получения списка пользователей:
![mind](Serv_5.png)

- Этот маршрут обрабатывает GET-запросы по адресу /users. Он извлекает все записи из таблицы users и отправляет их в ответе.
- В случае ошибки извлечения данных из базы данных, возвращается сообщение об ошибке.

## Обработчик для корневого маршрута:
![mind](Serv_6.png)

- Этот маршрут используется для проверки работы сервера. Если обратиться по корневому маршруту, сервер вернёт сообщение "Сервер работает!".

## Запуск сервера:
![mind](Serv_7.png)

- Сервер запускается на порту 3000 (или на порту, указанном в переменной окружения PORT).
- При успешном запуске выводится сообщение в консоль, что сервер работает.

## Файл package.json:
![mind](Serv_8.png)

- Этот файл содержит информацию о проекте и его зависимостях. В секции scripts определено, что для запуска сервера нужно использовать команду node server.js.
- В dependencies перечислены используемые модули: express, body-parser, sqlite3 и cors.


# Тестовая документация

Таким образом, в ходе разработки проекта, для проверки функциональности и обеспечения качества работы формы и API я перешел к составлению тестовой документации, которая охватывает следующие аспекты:

- Тестирование формы регистрации: Включает проверки корректности ввода данных пользователем, таких как имя, email, пароль и подтверждение пароля. Также проводится тестирование на корректность отображения ошибок и обработки некорректных данных.

- Тестирование API: Проверка корректности работы серверной части, включая валидацию данных на сервере, обработку запросов, сохранение данных в базе данных и ответы на запросы. Включает как позитивные, так и негативные тесты, чтобы убедиться, что система правильно обрабатывает все типы данных и ошибок.

- На основе функциональности формы регистрации и серверной части я создал тестовые случаи и чек-листы, охватывающие основные сценарии взаимодействия с системой. Тестирование включает как проверку работы интерфейса, так и тестирование API.

## Тест-план:
### 1. Общие сведения
1. Название проекта: Тестирование формы регистрации
2. Описание: Тестирование функциональности, валидации данных, пользовательского интерфейса и серверного взаимодействия формы регистрации.
3. Версия документа: 1.0
4. Дата составления: 20.11.2024
5. Ответственный: Дыченко В.И.
6. Термины и определения:
- UX (User Experience): Опыт взаимодействия пользователя с системой.
- UI (User Interface): Пользовательский интерфейс.
- XSS: Межсайтовый скриптинг (атака, позволяющая выполнить вредоносный JavaScript-код).
- API: Интерфейс программирования приложений.

### 2. Цель тестирования
- Убедиться, что форма регистрации работает корректно, удовлетворяет требованиям и предоставляет пользователю удобный интерфейс.
- Проверить правильность обработки данных как на клиентской стороне (валидация) так и на серверной (API).
- Убедиться в отсутствии уязвимостей, таких как XSS и SQL-инъекции.

### 3. Объекты тестирования
1. Клиентская часть:
- Поля: "Имя", "Email", "Пароль", "Подтверждение пароля".
- Кнопка: "Зарегистрироваться".
- Сообщения об ошибках.
2. Серверная часть:
- API для регистрации пользователя.
- Запросы: POST /register.

### 4. Ограничения
- Мобильная версия сайта не тестируется.
- Тестирование производительности и нагрузочного тестирования сервера не проводится.
- Проверка только на тестовом сервере, без взаимодействия с реальной базой данных.

### 5. Области тестирования
1. Функциональное тестирование:
- Валидация полей на клиентской и серверной стороне.
- Корректность сообщений об ошибках.
- Проверка отправки данных на сервер.
- UX/UI тестирование:
- Удобство и доступность элементов формы.
- Визуальная корректность интерфейса.
2. Кроссбраузерное тестирование:
- Работа формы в Google Chrome, Mozilla Firefox, Microsoft Edge и Safari.
3. Серверное тестирование (API):
- Проверка корректности обработки запросов POST /register.
- Валидация данных на стороне сервера.
4. Безопасность:
- Защита от XSS-атак.
- Защита от SQL-инъекций.

### 6. Типы тестирования
- Позитивное тестирование: Проверка формы с корректными данными.
- Негативное тестирование: Проверка с некорректными данными (например, пустые поля, неверный формат email).
- Тестирование безопасности: Ввод вредоносных данных в поля формы.
- API-тестирование: Проверка работы с серверными запросами.

### 7. Тестовое окружение
1. Операционная система: Windows 10
2. Браузеры:
Google Chrome (последняя версия)
- Mozilla Firefox
- Microsoft Edge
- Safari
3. Инструменты:
- Bugzilla, JIRA (для учета багов)
- Postman (для API-тестирования)
- TestRail (для управления тест-кейсами)

### 8. Критерии начала и завершения тестирования
1. Критерии начала:
- Форма регистрации доступна для тестирования.
- Настроен тестовый сервер.
- Подготовлены тестовые данные и окружение.
2. Критерии завершения:
- Все тест-кейсы выполнены.
- Все баги высокого и критического приоритета устранены.
- Проведено повторное тестирование после исправлений.

### 9. Риски
- Задержки из-за критических дефектов, мешающих тестированию.
- Ограничения в тестовом окружении (устаревшие браузеры).
- Неполная документация по API.

### 10. Результаты
1. Ожидаемые результаты:
- Все баги устранены.
- Валидация данных работает корректно.
- API корректно обрабатывает запросы и возвращает правильные ответы.
- Пользовательский интерфейс удобен и соответствует требованиям.
2. Рекомендации:
Провести тестирование мобильной версии сайта.
Расширить тестирование API для оценки производительности.

- Ссылка на тест-план: https://docs.google.com/document/d/1rZaS7SEOOVc_AMaZOvB1QA2uKWZ5PUR-owhR1kbRH8c/edit?tab=t.0

## Чек-листы:
### Чек-лист для функционального тестирования (клиентская часть)
![mind](chek_1.png)

### Чек-лист для UX/UI тестирования
![mind](chek_2.png)

### Чек-лист для кроссбраузерного тестирования
![mind](chek_3.png)

### Чек-лист для серверного тестирования (API)
![mind](chek_4.png)


- Ссылка на чек-листы: https://docs.google.com/document/d/1BaBD3Bmn_ZwCvDnhdXQq0nnMkMswQHk3sVdj6233ev8/edit?tab=t.0

## Тест-кейсы:
![mind](cases_1_1.png)
- Ссылка на тест-кейсы: https://docs.google.com/spreadsheets/d/10KoaP0LkXU2yAiv7zhEwtv7Yo1l2oUoDbVeeov7NLdg/edit?gid=0#gid=0
