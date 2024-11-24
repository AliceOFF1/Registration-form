# Введение

В рамках моей тестовой работы для демонстрации своих навыков в области ручного тестирования, я решил создать простую форму регистрации и разместить её на платформе Glitch. Эта платформа позволила мне быстро настроить как клиентскую, так и серверную части, что дало возможность протестировать не только форму регистрации с точки зрения функциональности и интерфейса, но и проверить работу серверной части через API

В ходе выполнения задания я составил детальный тест-план, чек-листы и тест-кейсы, охватывающие как позитивные, так и негативные сценарии. Также были проведены кросс-браузерные тестирования и тесты на безопасность (например, защита от XSS-атак).

## Проект включает:

- Веб-форму регистрации с полями для имени, email, пароля и подтверждения пароля.
- Валидацию данных на клиентской и серверной сторонах.
- Реализацию серверных API для обработки данных.
- API тестирование с помощью Postman для проверки корректности работы серверных запросов и ответов.

# Форма регистрации
Этот сайт представляет собой простую веб-форму регистрации пользователей, с использованием HTML, CSS и JavaScript. Он взаимодействует с серверной частью для обработки регистрации, отправляя данные на сервер через API

![mind](folder_image/registrForm.png)
- URL проекта: https://dot-tropical-ocicat.glitch.me

## HTML-структура:
![mind](folder_image/1.png)
- Форма регистрации состоит из четырёх полей: имя, email, пароль и подтверждение пароля.
- Каждое поле сопровождается элементом <span> для отображения ошибок валидации, если пользователь введет неверные данные.
- Кнопка отправки формы вызывает функцию validateForm(), которая проверяет корректность введённых данных.

## CSS-стили:
![mind](folder_image/css.png)

Стили для формы регистрации задают простое и удобное оформление:
- Все элементы формы, включая поля ввода и кнопки, занимают всю ширину контейнера.
- Ошибки валидации выделяются красным цветом.
- Кнопка оформления заявки имеет фон синего цвета, который меняется при наведении.

## JavaScript-валидация и отправка данных на сервер:
![mind](folder_image/java_1.png)
![mind](folder_image/java_2.png)

validateForm():
1. Очистка предыдущих ошибок: перед проверкой данных удаляются все предыдущие сообщения об ошибках.
2. Валидация:
- Проверяется, что имя не пустое.
- Для email проверяется, чтобы он был корректно отформатирован.
- Пароль проверяется на минимальную длину (8 символов).
- Пароли должны совпадать.
3. Если форма прошла валидацию, данные отправляются на сервер с помощью fetch(). Сервер принимает данные в формате JSON и, в зависимости от результата, показывает сообщение об успехе или ошибке.

## API-соединение:
![mind](folder_image/api.png)

POST-запрос отправляется на сервер, который обработает данные регистрации. Адрес сервера указывается как https://mirror-spiced-brian.glitch.me/. Данные отправляются в формате JSON.

# Серверная часть
## Импорт зависимостей:
![mind](folder_image/Serv_1.png)

Здесь импортируются необходимые зависимости:

- express: используется для создания серверных маршрутов и обработки HTTP-запросов.
- body-parser: модуль для парсинга тела запроса, в частности JSON, что необходимо для работы с данными, отправляемыми в POST-запросах.
- sqlite3: модуль для работы с базой данных SQLite, который позволяет использовать SQL-запросы для сохранения и извлечения данных.
- cors: модуль, который разрешает кросс-доменные запросы, что важно, если клиентская часть и сервер находятся на разных доменах.

## Создание экземпляра Express и настройки:
![mind](folder_image/Serv_2.png)

- app — это экземпляр Express, который будет использоваться для создания серверных маршрутов.
- cors({ origin: "*" }): разрешает доступ к серверу с любых доменов. Это полезно для разработки, но в реальных условиях нужно указать конкретные домены.
- bodyParser.json(): парсит входящие JSON-запросы, позволяя извлекать данные из тела запроса.

## Создание базы данных и таблицы:
![mind](folder_image/Serv_3.png)

- Создаётся база данных SQLite с именем users.db. Если база уже существует, устанавливается соединение с ней.
- db.run(...): создаёт таблицу users, если она ещё не существует. В таблице хранятся данные пользователей: id (уникальный идентификатор), name (имя пользователя), email (электронная почта) и password (пароль).

## Маршрут для регистрации:
![mind](folder_image/Serv_4.png)

1. Этот маршрут обрабатывает POST-запросы по адресу /register. Он принимает данные от клиента: name, email, password и confirmPassword.
2. Применяется валидация:
- Проверяется наличие имени.
- Проверяется формат email с помощью регулярного выражения.
- Проверяется длина пароля (не менее 8 символов).
- Проверяется совпадение паролей.
3. Если все проверки проходят успешно, данные пользователя сохраняются в базе данных SQLite. В случае успеха сервер возвращает сообщение о том, что регистрация прошла успешно. Если возникла ошибка при добавлении данных в базу, отправляется ошибка.

## Маршрут для получения списка пользователей:
![mind](folder_image/Serv_5.png)

- Этот маршрут обрабатывает GET-запросы по адресу /users. Он извлекает все записи из таблицы users и отправляет их в ответе.
- В случае ошибки извлечения данных из базы данных, возвращается сообщение об ошибке.

## Обработчик для корневого маршрута:
![mind](folder_image/Serv_6.png)

- Этот маршрут используется для проверки работы сервера. Если обратиться по корневому маршруту, сервер вернёт сообщение "Сервер работает!".

## Запуск сервера:
![mind](folder_image/Serv_7.png)

- Сервер запускается на порту 3000 (или на порту, указанном в переменной окружения PORT).
- При успешном запуске выводится сообщение в консоль, что сервер работает.

## Файл package.json:
![mind](folder_image/Serv_8.png)

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
- Убедиться в отсутствии уязвимостей XSS.


### 3. Объекты тестирования
1. Клиентская часть:
- Поля: "Имя", "Email", "Пароль", "Подтверждение пароля".
- Кнопка: "Зарегистрироваться".
- Сообщения об ошибках.
2. Серверная часть:
- API для регистрации пользователя.
- Запросы: POST, GET.

### 4. Ограничения
- Мобильная версия сайта не тестируется.
- Тестирование производительности и нагрузочного тестирования сервера не проводится.
- Проверка только на тестовом сервере, без взаимодействия с реальной базой данных.


### 5. Области тестирования
1. Функциональное тестирование:
- Валидация полей на клиентской и серверной стороне.
- Корректность сообщений об ошибках.
- Проверка отправки данных на сервер.
2. UX/UI тестирование:
- Удобство и доступность элементов формы.
- Визуальная корректность интерфейса.
3. Кроссбраузерное тестирование:
- Работа формы в Google Chrome, Mozilla Firefox, Microsoft Edge и Safari.
4. Серверное тестирование (API):
- Проверка корректности обработки запросов POST /register.
-  Валидация данных на стороне сервера.
5. Безопасность:
- Защита от XSS-атак.


### 6. Типы тестирования
1. Позитивное тестирование: Проверка формы с корректными данными.
2. Негативное тестирование: Проверка с некорректными данными (например, пустые поля, неверный формат email).
3. Тестирование безопасности: Ввод вредоносных данных в поля формы.
4. API-тестирование: Проверка работы с серверными запросами.

### 7. Тестовое окружение
1. Операционная система: Windows 10
2. Браузеры:
- Google Chrome (последняя версия)
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
- Провести тестирование мобильной версии сайта.
- Расширить тестирование API для оценки производительности.


- Ссылка на тест-план: https://docs.google.com/document/d/1rZaS7SEOOVc_AMaZOvB1QA2uKWZ5PUR-owhR1kbRH8c/edit?tab=t.0

## Чек-листы:
### Чек-лист для функционального тестирования (клиентская часть)
![mind](folder_image/flcChek.png)

### Чек-лист для UX/UI тестирования
![mind](folder_image/chek_2.png)

### Чек-лист для кроссбраузерного тестирования
![mind](folder_image/chek_3.png)

### Чек-лист для серверного тестирования (API)
![mind](folder_image/apiChek.png)


- Ссылка на чек-листы: https://docs.google.com/document/d/1BaBD3Bmn_ZwCvDnhdXQq0nnMkMswQHk3sVdj6233ev8/edit?tab=t.0

## Тест-кейсы:
- Функциональное тестирования

![mind](folder_image/flcTest.png)

- UX/UI тестирование

![mind](folder_image/uclTest.png)

- Кроссбраузерное тестирование

![mind](folder_image/cclTest.png)

- Серверное тестирование (API)

![mind](folder_image/apiTest.png)

- Ссылка на тест-кейсы: https://docs.google.com/spreadsheets/d/10KoaP0LkXU2yAiv7zhEwtv7Yo1l2oUoDbVeeov7NLdg/edit?gid=0#gid=0

# Проведение тестов

## Клиентская часть:

### **FCL_01 (Отправка формы с пустыми полями)**
- **Результат**: Форма работает корректно. Уведомления об ошибках отображены во всех полях.

![mind](folder_image/fcl_01.png)

### **FCL_02 (Отправка формы с корректными данными)**
- **Результат**: Форма работает корректно. Регистрация пройдена.

![mind](folder_image/fcl_02.png)

### **FCL_03 (Ввод некорректного email)**
- **Результат**: Форма работает корректно. Отображается сообщение: "Введите корректный email"

![mind](folder_image/fcl_03.png)

### **FCL_04 (Пароль менее 8 символов)**
- **Результат**: Форма работает корректно. Отображается сообщение: "Пароль должен содержать не менее 8 символов"

![mind](folder_image/fcl_04.png)

### **FCL_05 (Несовпадение паролей)**
- **Результат**: Форма работает корректно. Отображается сообщение: "Пароли не совпадают"

![mind](folder_image/fcl_05.png)

### **FCL_06 (Проверка обязательности поля "Имя")**
- **Результат**: Форма работает корректно. При отсутствии значения в поле отображается сообщение об ошибке: "Имя обязательно для заполнения".
  
![mind](folder_image/fcl_06.png)

### **FCL_07 (Ввод пробелов в поле "Имя")**
- **Результат**: Результат выполнения тест-кейса отличается от ожидаемого. При вводе только пробелов в поле "Имя", регистрация недоступна, что соответствует ожидаемому поведению формы. Однако сообщение об ошибке идентично случаю из тест-кейса FCL_06: "Имя обязательно для заполнения".
  
- **Анализ**: Поле "Имя" не распознаёт пробелы как недопустимые символы, а скорее "игнорирует" их, считая значение пустым. Если валидационная логика формы должна строго запрещать ввод пробелов (в качестве единственного содержимого), то текущее поведение можно рассматривать как нарушение бизнес-логики и недостаточную проверку пользовательского ввода.
  
**Рекомендации:**
- Уточнить требования к полю "Имя".
- Если пробелы недопустимы, добавить соответствующую валидацию и выводить сообщение об ошибке, отличающееся от стандартного "Имя обязательно для заполнения".
- Ввести обработку случаев, когда поле содержит только пробелы, с явным указанием на некорректный формат данных.

![mind](folder_image/fcl_07.png)

### **FCL_08 (Ввод специальных символов в поле "Имя")**

- **Результат**: Регистрация успешно завершена, несмотря на наличие специальных символов в поле "Имя".

- **Анализ**: Валидация поля "Имя" не учитывает наличие специальных символов, что нарушает требования к корректности данных. Пользователь может ввести недопустимые символы, что приводит к некорректной регистрации.

**Рекомендации:**
- Реализовать проверку на недопустимые символы в поле.
- При вводе таких символов вывести сообщение об ошибке, информирующее пользователя о допустимом формате имени.

![mind](folder_image/fcl_08.png)

### **FCL_09 (Ввод больших значений в поле "Имя")**

- **Результат**: Регистрация проходит успешно даже при превышении максимальной длины (30 символов).

- **Анализ**: Поле "Имя" не ограничивает длину вводимого текста, что может привести к проблемам с обработкой данных в системе.

**Рекомендации:**

- Ограничить длину поля на уровне ввода.
- Реализовать серверную проверку на соответствие длины допустимым значениям.
- Выводить сообщение об ошибке, если длина превышает установленный лимит.

  ![mind](folder_image/fcl_09.png)

### **FCL_10 (Ввод цифр в поле "Имя")**

- **Результат**: Регистрация успешно завершена, несмотря на наличие цифр в поле "Имя".

- **Анализ**: Валидация поля не учитывает наличие цифр, что позволяет пользователю ввести некорректные данные.

**Рекомендации:**

- Добавить проверку на наличие цифр в поле "Имя".
- Выводить сообщение об ошибке при обнаружении цифр, информируя пользователя о допустимом формате имени.
- Убедиться, что валидация работает на клиентской и серверной стороне для обеспечения целостности данных.

  ![mind](folder_image/fcl_10.png)

## Серверная часть:

### **API_01 (Успешная регистрация)**
- **Результат**: Статус-код 200 OK. Тело ответа: {"message": "Регистрация успешна"}

 ![mind](folder_image/POST_API_01.png)
 
### **API_02 (Регистрация с пустым email)**
- **Результат**: Статус-код 400 Bad Request. Тело ответа: {"error": "Введите корректный email"}

 ![mind](folder_image/POST_API_02.png)
 
### **API_03 (Регистрация с некорректным email)**
- **Результат**: Статус-код 400 Bad Request. Тело ответа: {"error": "Введите корректный email"}

 ![mind](folder_image/POST_API_03.png)
 
### **API_04 (Несовпадение паролей)**
- **Результат**: Статус-код 400 Bad Request. Тело ответа: { "error": "Пароли не совпадают" }

 ![mind](folder_image/POST_API_04.png)
 
### **API_05 (Регистрация с уже существующим email)**

- **Результат**: Пользователь успешно регистрируется с одинаковыми данными, что нарушает логику системы.

- **Анализ**: Валидация email на уникальность не срабатывает должным образом, что может привести к дублированию пользователей и нарушению логики работы системы.

**Рекомендации:**

- Реализовать проверку на уникальность email в процессе регистрации.
- При попытке зарегистрировать email, который уже существует в базе данных, возвращать правильный статус-код и сообщение об ошибке.

 ![mind](folder_image/POST_API_05.png)
 

### **API_06 (Проверка XSS-атаки)**

- **Результат**: Регистрация проходит успешно, несмотря на попытку ввода потенциально опасного кода, что может привести к уязвимости в системе.

- **Анализ**: Валидация данных не блокирует ввод вредоносных скриптов, что может привести к рискам безопасности, таким как выполнение JavaScript-кода на стороне клиента.

**Рекомендации:**

- Добавить проверку на ввод небезопасных символов и XSS-атак.
- Сервер должен фильтровать или экранировать все пользовательские данные, чтобы предотвратить выполнение вредоносных скриптов.
- Вернуть соответствующий код ошибки и сообщение при попытке отправить такие данные.

 ![mind](folder_image/POST_API_06.png)
 
### **API_07 (Отправка некорректного JSON)**

- **Результат**: Статус-код 400 Bad Request. Тело ответа: { "error": "SyntaxError" }

 ![mind](folder_image/POST_API_07.png)
 
### **API_08 (Получение списка пользователей)**

- **Результат**: Статус-код 200 OK. Сервер возвращает корректный список пользователей в формате JSON. 

 ![mind](folder_image/GET_1.png)

 # Баг-репорт

По всем обнаруженным багам был составлен баг-репорт с помощью Jira. Каждый баг-репорт содержит в себе: 

- Шаги, по которым был обнаружен;
- Ожидаемый результат;
- Фактический результат;
- Анализ;
- Рекомендации по устранению;

![mind](folder_image/bug_fcl_07.png)

![mind](folder_image/bug_fcl_08.png)

![mind](folder_image/bug_fcl_09.png)

![mind](folder_image/bug_fcl_10.png)

![mind](folder_image/bug_api_05.png)

![mind](folder_image/bug_api_06.png)

 # Рекомендации по улучшению работы системы:
 
1. **Улучшение валидации на серверной стороне:**

- Добавить проверку на уникальность email при регистрации, чтобы предотвратить создание нескольких учетных записей с одинаковым email.
- Внедрить проверку на допустимость спецсимволов в поле "Имя" и других полях, чтобы избежать ввода некорректных данных и улучшить пользовательский опыт.
- Разработать более строгие проверки на корректность формата пароля (например, минимум одна заглавная буква, цифра и спецсимвол).
  
2. **Улучшение пользовательского интерфейса:**

- Разработать функцию подсказок, чтобы пользователи могли видеть, какие именно требования предъявляются к паролю и другим полям еще до начала ввода.
- Реализовать поддержку мультиязычности, чтобы интерфейс был доступен для пользователей, говорящих на разных языках.

3. **Повышение безопасности:**

- Добавить защиту от атак XSS (межсайтового скриптинга) путем фильтрации входящих данных и экранирования пользовательского ввода.
- Внедрить защиту от CSRF-атак (межсайтовых подделок запросов), добавив соответствующие токены проверки в запросы.

4. **Оптимизация производительности:**

- Провести тестирование производительности системы при одновременном выполнении множества регистраций, чтобы убедиться, что система может обрабатывать большое количество пользователей без сбоев.
- Реализовать кэширование данных на стороне клиента для ускорения загрузки страницы и уменьшения нагрузки на сервер.

5. **Покрытие тестами:**

- Дополнительно разработать автоматические тесты для проверки всех возможных вариантов ввода данных, включая тесты на корректность работы с большими объемами данных и на валидацию различных типов пользовательских данных.
- Провести дополнительные тесты на безопасность и устойчивость системы к различным типам атак, включая SQL-инъекции и другие распространенные уязвимости.

6. **Мобильная адаптация:**

- Провести тестирование формы на мобильных устройствах с разными разрешениями экрана для обеспечения корректного отображения и удобства использования.
- Оптимизировать элементы формы для мобильных устройств, чтобы пользовательский интерфейс был удобен и на маленьких экранах.
  
 # Заключение
- В рамках выполнения тестовой работы был проведен комплексный процесс тестирования веб-формы регистрации, включая как клиентскую, так и серверную части. Проект включал в себя разработку формы регистрации с полями для имени, email, пароля и подтверждения пароля, а также реализацию валидации данных на обеих сторонах. Тестирование охватило функциональные аспекты, проверку пользовательского интерфейса, кросс-браузерную совместимость, а также безопасность (например, защиту от XSS-атак).

- В ходе тестирования была составлена подробная документация, включая тест-план, чек-листы и тест-кейсы, что позволило охватить все ключевые аспекты работы формы и серверной части. Тесты подтвердили правильность работы основной функциональности, но также выявили несколько проблем в валидации данных, таких как отсутствие проверки на пробелы и спецсимволы в поле "Имя", а также возможность регистрации с уже существующим email, что требует доработки логики серверной части.

- Кроме того, были проведены тесты на совместимость с различными браузерами, что подтвердило корректную работу формы в популярных браузерах. Все найденные дефекты были описаны, и на основе результатов тестирования были выработаны рекомендации по улучшению работы системы.







