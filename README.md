# Курсовой проект «Сетевой чат»

## Описание проекта

Вам нужно разработать два приложения для обмена текстовыми сообщениями по сети с помощью консоли (терминала) между двумя и более пользователями.

**Первое приложение — сервер чата**. Оно должно ожидать подключения пользователей.

**Второе приложение — клиент чата**. Подключается к серверу чата и осуществляет доставку и получение новых сообщений.

Все сообщения должны записываться в file.log как на сервере, так и на клиентах. File.log должен дополняться при каждом запуске, а также при отправленном или полученном сообщении. Выход из чата должен быть осуществлён по команде exit.

## Требования к серверу

1. Установка порта для подключения клиентов через файл настроек (например, settings.txt).
2. Возможность подключиться к серверу в любой момент и присоединиться к чату.
3. Отправка новых сообщений клиентам.
4. Запись всех отправленных через сервер сообщений с указанием имени пользователя и времени отправки.

## Требования к клиенту

1. Выбор имени для участия в чате.
2. Прочитать настройки приложения из файла настроек: например, номер порта сервера.
3. Подключение к указанному в настройках серверу.
4. Для выхода из чата нужно набрать команду выхода “/exit”.
5. Каждое сообщение участников должно записываться в текстовый файл — файл логирования. При каждом запуске приложения файл должен дополняться.

## Требования в реализации

1. Сервер должен уметь одновременно ожидать новых пользователей и обрабатывать поступающие сообщения от пользователей.
2. Использован сборщик пакетов gradle/maven. 3.
3. Код размещён на GitHub.
4. Код покрыт unit-тестами.

## Шаги реализации

1. Нарисовать схему приложений.
2. Описать архитектуру приложений — сколько потоков за что отвечают, придумать протокол обмена сообщениями между приложениями.
3. Создать репозиторий проекта на GitHub.
4. Написать сервер.
5. Провести интеграционный тест сервера, например, с помощью telnet.
6. Написать клиент.
7. Провести интеграционный тест сервера и клиента.
8. Протестировать сервер при подключении нескольких клиентов.
9. Написать README.md к проекту.
10. Отправить на проверку.

# Описание предоставленного решения к проекту
## [Сервер](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/Server.java)

### Сервер используется для принятия подключений пользователей и получения от них сообщений

- [ServLogger.java](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/log/ServLogger.java)
Производит запись событий сервера в отдельный лог файла;
- Запись в файл [setting](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/5bff8cd39e02f240c97360c8128aedfb8c4ca5b7/src/main/java/Server.java#L33-L39) настроек сервера, откуда пользователь считывает настройки и подключается;
- [MyLogger.java](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/log/MyLogger.java)
Служит для записи сообщений от пользователей;
- Устанавливаем поток для принятия новых пользователей в [MonoThreadClientHandler](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/thread/MonoThreadClientHandler.java), откуда происходит дальнейшее чтение сообщений каждого польователя отдельно;
- [Поток чтения команд сервера](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/Server.java#L32-L52).

##  MonoThreadClientHandler.java - поток для обработки [сообщений](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/thread/MonoThreadClientHandler.java)
- Получаем [имя пользователя](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/thread/MonoThreadClientHandler.java#L27-L29), чтобы знать, от кого приходят сообщения и под его именем записывать в файл сообщение;
- Получение, отправка и запись [сообщений](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/thread/MonoThreadClientHandler.java#L31-L48) от пользователя, пока не поступила команда "/end".

## [Client.java](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/Client.java)

- Считываем [settings](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/Client.java#L23-L38) настроек сервера для подключения;
- Производим авторизацию и передаем серверу [имя пользователя](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/Client.java#L40-L51);
- Создаем поток для постоянно считывания файла и проверка на новые [сообщения](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/Client.java#L54-L55) и сам [код потока](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/thread/ThreadReadMessage.java);
- Посылаем сообщения серверу для записи в [файл](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/Client.java#L57-L85) до момента ввода "/end", тогда происходит разрыв канала.

## [ClientTest.java](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/thread/ClientTest.java)

- Поток для тестирования подключения нескольких пользователей, которые запускаются через [Main.java](https://github.com/neo7976/Java-6-Homeworks-Multithreading-6-Course/blob/abb732216266eeb5221c565e48a74bc1e4baa2bc/src/main/java/Main.java)

## Пример записи в файл сообщений от тестированных нескольких пользоваталей
![](/src/main/resources/logUser.png)
## Пример записи в файл сервера от тестированных нескольких пользоваталей
![](/src/main/resources/logServ.png)