## Дипломный проект   

Дипломный проект представляет собой автоматизацию тестирования комплексного сервиса, взаимодействующего с СУБД и API Банка.

## Описание приложения

### Бизнес-часть

Приложение представляет собой веб-сервис, который предоставляет возможность купить тур по определённой цене с помощью двух способов:

1. Обычная оплата по дебетовой карте
1. Выдача кредита по данным банковской карты

Само приложение не обрабатывает данные по картам, а пересылает их банковским сервисам:

* сервису платежей (далее - Payment Gate)
* кредитному сервису (далее - Credit Gate)

Приложение должно в собственной СУБД сохранять информацию о том, каким способом был совершён платёж и успешно ли он был совершён (при этом данные карт сохранять не допускается).

### Техническая часть

Само приложение расположено в файле `aqa-shop.jar` и запускается стандартным способом `java -jar /artifacts/aqa-shop.jar` на порту 8080.

В файле `application.properties` приведён ряд типовых настроек:

* учётные данные и url для подключения к СУБД
* url-адреса банковских сервисов

### СУБД

Заявлена поддержка двух СУБД:

* MySQL
* PostgreSQL

Учётные данные и url для подключения задаются в файле `application.properties`.

### Банковские сервисы

В качестве банковского сервиса для работы приложения будет использоваться симулятор, который может принимать запросы в нужном формате и генерировать ответы.

Симулятор написан на Node.js, поэтому для его запуска необходим либо Docker, либо установленный Node.js. Симулятор расположен в каталоге gate-simulator.

Симулятор позволяет для заданного набора карт генерировать предопределённые ответы.

Набор карт представлен в формате JSON в файле data.json.

Данный сервис, симулирует как Payment Gate, так и Credit Gate.

## Как запускать тесты

**Условия**

На машине для запуска тестов установлены Docker, Gradle, MySQL/PostgreSQL, JDK 11, Google Chrome.

**Шаги**

1. Открыть терминал;

2. Скопировать репозиторий: `git clone https://github.com/IlyaMahnach/DiplomWork.git`

3. Запустить контейнер, в котором разворачивается база данных (далее БД) docker-compose up -d --force-recreate

4. Убедиться в том, что БД готова к работе (логи смотреть через `docker-compose logs -f <applicationName> (mysql или postgres)`

5. Запустить SUT во вкладке Terminal Intellij IDEA командой: `java -jar artifacts/aqa-shop.jar`

6. Для запуска авто-тестов в Terminal Intellij IDEA открыть новую сессию и ввести команду: `./gradlew clean test -Durl="jdbc:mysql://localhost:3306/app" -Duser="app" -Dpassword="pass" --info`


