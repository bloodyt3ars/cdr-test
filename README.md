# cdr-test
## Оглавление
* ### [Описание проекта](#defenition)
* ### [Основные функции](#mainfunctions)
* ### [Структура проекта](#projectstructure)
* ### [Требования к системе](#requirements)
* ### [Запуск проекта](#start)
* ### [API](#api)


## Описание проекта <a name="defenition"></a>
Данный проект представляет собой систему управления телефонными звонками, которая позволяет авторизовывать пользователей, тарифицировать их звонки, изменять тарифы и балансы абонентов, а также просматривать детализацию звонков.


Микросервисы:
* Коммутатор, собирающий файл CDR (Call Data Record) с информацией о звонках.
* Сервер BRT (Billing Real Time), который авторизует абонентов и генерирует файл CDR+ с информацией о тарифе абонента.
* Сервер HRS (High performance rating server), который тарифицирует звонки и возвращает данные о списании средств.
* CRM система с двумя уровнями прав: менеджер и абонент.


Для тарификации абонента, необходимо отправить запрос в BRT с данными о номере телефона и балансе абонента. Для изменения тарифа и баланса абонента используется запрос от менеджера. Для просмотра детализации звонков абонента используется запрос от абонента с указанием его номера телефона.


Тарификация звонков происходит в зависимости от выбранного тарифа. Тарифы могут быть следующими:

* Безлимит 300: за тарифный период (например, месяц) стоит фиксированную сумму 100р. Каждая последующая минута стоит 1р.
* Поминутный: 1 минута разговора стоит 1.5 рубля.
* Обычный: входящие - бесплатно, исходящие - первые 100 минут по 0.5р/минута, после по тарифу "поминутный".


### Уточнения
* Входящие звонки всегда платные, если не указано обратное.
* Каждая дополнительная секунда разговора засчитывается как минута.
* При тарификации каждой CDR представляет из себя тарифный период.


## Основные функции <a name="mainfunctions"></a>
* Регистрация абонента с указанием логина, номера телефона, тарифа и начального баланса.
* Выбор и изменение тарифа абонента менеджером.
* Пополнение баланса абонента.
* Звонки между абонентами с учетом тарификации.
* Проведение тарификации менеджером.


## Структура проекта <a name="projectstructure"></a> 
* cdrtest - главная директория проекта.
* component - содержит компоненты, отвечающие за логику приложения, включая сервисы и классы, обрабатывающие данные.
* controller - содержит контроллеры, которые обрабатывают HTTP-запросы, и связываются с компонентами.
* converter - содержит классы для конвертации объектов из одного типа в другой.
* dto - содержит DTO (Data Transfer Object) - классы, используемые для передачи данных между слоями приложения.
* entity - содержит классы-сущности, отображающие таблицы базы данных.
* repository - содержит интерфейсы репозиториев, отвечающие за доступ к базе данных.
* security - содержит классы, отвечающие за безопасность приложения.
* swagger - содержит классы, отвечающие за генерацию документации API с помощью Swagger.


## Требования к системе <a name="requirements"></a>
Для работы системы необходимо наличие следующих компонентов:
* JDK 17
* PostgreSQL 15.2


## Запуск проекта <a name="start"></a>
Для установки и запуска приложения необходимо:
1. Склонировать репозиторий на свой локальный компьютер.
2. Установить необходимые зависимости, указанные в файле pom.xml.
3. Изменить настройки подключения к базе данных в файле application.properties.
4. Перед запуском приложения запустите файлик start.sql в папке ресурсов
5. Запустить приложение с помощью команды mvn spring-boot:run.


В базе уже заранее создано два пользователя.


Абонент:
* логин: **_user_**
* пароль: **_user_**


Менеджер:
* логин: **_admin_**
* пароль: **_admin_**


## API <a name="api"></a>
Для получения документации по API приложения необходимо запустить приложение и перейти по адресу http://localhost:8080/swagger-ui.html. 


Или в файле openapi.yaml и вставив все по ссылке: https://editor.swagger.io/


В Swagger документации описаны все доступные запросы и ответы на них.
