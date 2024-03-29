# REST API

Adspect предоставляет REST API для автоматизации различных задач:

* Создания, изменения и удаления потоков;
* Получения PHP-файлов интеграции;
* Управления гостевым доступом к статистике.

API поддерживает JSON- и XML-кодирование данных. В примерах ниже будет использовано JSON-кодирование для простоты.

## Аутентификация

Для аутентификации в Adspect REST API
используется [HTTP-аутентификация типа Basic](https://developer.mozilla.org/ru/docs/Web/HTTP/%D0%90%D0%B2%D1%82%D0%BE%D1%80%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F#Basic_authentication_scheme),
в которой ключ API передается в качестве имени пользователя, а пароль оставляется пустым. Ваш ключ API
находится в вашем профиле.

Каждый запрос к API должен содержать два обязательных заголовка:

1. `Authorization: Basic <authKey>` для аутентификации в Adspect;
2. `Content-Type` для выбора способа кодирования данных:
   * `Content-Type: application/json` для выбора JSON-кодирования;
   * `Content-Type: application/xml` для выбора XML-кодирования.

Поле `<authKey>` в заголовке Authorization формируется путем [base64-кодирования](https://ru.wikipedia.org/wiki/Base64)
строки, составленной из ключа API и добавленного в конце двоеточия.

Пример кода на PHP:

```php
<?php
$apiKey = 'SEbMw152aoe2ArffS7yjEJzJ_MFnd33e';
$authKey = base64_encode($apiKey . ':');
```

:::{tip}
Для удобства вы можете взять готовое значение `<authKey>` в вашем профиле в поле "Заголовок Authorization".
:::

Некоторые HTTP-клиенты имеют нативную поддержку аутентификации HTTP Basic и самостоятельно добавляют заголовок `Authorization`.
Например, [Python Requests](https://docs.python-requests.org/en/latest/) предоставляет класс `requests.auth.HTTPBasicAuth`.
В этом случае укажите ваш ключ API в качестве имени пользователя и оставьте пароль пустым. Пример для
[php-curl](https://www.php.net/manual/ru/book.curl.php):

```php
<?php
$apiKey = 'SEbMw152aoe2ArffS7yjEJzJ_MFnd33e';
$curl = curl_init();
curl_setopt($curl, CURLOPT_USERPWD, $apiKey . ':');
```

## Точки вызова

Базовый URL для всех точек вызова --- `https://api.adspect.net/v1`. Описания точек вызова ниже указывают пути
относительно этого базового URL. Сначала указан HTTP-метод, за которым следует относительный путь точки вызова.
Например, описание точки вызова `GET /streams` означает GET-запрос на URL `https://api.adspect.net/v1/streams`.

## Коллекции

Некоторые свойства объектов могут принимать значения только из строго заданных множеств --- коллекций. 
В таблице ниже приводятся точки вызова для получения коллекций:

| Точка вызова                      | Описание |
|:----------------------------------|:---------|
| `GET /collections/presets`        | Шаблоны потоков для различных источников трафика. |
| `GET /collections/os`             | Операционные системы для таргетинга и статистики. |
| `GET /collections/browsers`       | Браузеры для таргетинга и статистики. |
| `GET /collections/engines`        | Движки браузера для таргетинга и статистики. |
| `GET /collections/countries`      | Коды стран для таргетинга и статистики. |
| `GET /collections/languages`      | Коды языков для таргетинга и статистики. |
| `GET /collections/time-zones`     | Часовые пояса для таргетинга и статистики. |
| `GET /collections/stream-modes`   | Режимы потока. |
| `GET /collections/stream-actions` | Действия для контента и белой страницы (см. ниже). |
| `GET /collections/query-group-by` | Элементы разбивки для воронки продаж. |

### Действия потока

Следующая таблица описывает действия для контента и белой страницы:

| Действие  | Описание |
|:----------|:---------|
| `local`   | Локальный файл без редиректа. |
| `proxy`   | Проксирование. |
| `fetch`   | Подгрузка HTML-кода. |
| `iframe`  | Отображение в iframe. |
| `301`     | Редирект HTTP 301. |
| `302`     | Редирект HTTP 302. |
| `303`     | Редирект HTTP 303. |
| `noop`    | Без действия. |
| `refresh` | Редирект HTTP Refresh. |
| `meta`    | Редирект HTML meta refresh. |
| `return`  | Произвольный код ответа HTTP. |
| `php`     | Выполнить PHP-код. |
| `js`      | Выполнить JavaScript-код. |
| `xar`     | Заголовок `X-Accel-Redirect`. |
| `xsf`     | Заголовок `X-Sendfile`. |

## Управление потоками

Поток представляется в виде объекта со следующими свойствами:

:::{list-table} Свойства потока
:header-rows: 1

* - Свойство
  - Тип
  - Описание

* - `stream_id`
  - Строка
  - Идентификатор потока.

* - `account_id`
  - Строка
  - Идентификатор аккаунта. Только для чтения.

* - `name`
  - Строка
  - Имя потока.

* - `tags`
  - Массив строк
  - Теги, до 32 штук.

* - `mode`
  - Строка
  - Режим потока, один из:<br>
    `Filter` --- фильтр;<br>
    `Review` --- модерация;<br>
    `Money` --- контент;<br>
    `White` --- белая страница.

* - `notes`
  - Строка
  - Любые заметки, которые вы хотели бы записать.

* - `money_pages`
  - Массив объектов
  - Массив контент-страниц, до 254 объектов, каждый со следующими свойствами:<br>
    `page` --- URL / путь к файлу / код (в зависимости от действия), строка;<br>
    `action` --- действие, производимое с посетителем, строка;<br>
    `arg_passthru` --- включить проброс URL-параметров, логический;<br>
    `weight` --- вес при ротации, целочисленный;<br>
    `enabled` --- включить контент-страницу, логический.

* - `rotator`
  - Строка
  - Ротатор контент-страниц: `Split` (сплит) или `Timer` (таймер).

* - `safe_pages`
  - Массив из одного объекта
  - Белая страница.  Массив из одного объекта со следующими свойствами:<br>
    `page` --- URL / путь к файлу / код (в зависимости от действия), строка;<br>
    `action` --- действие, производимое с посетителем, строка;<br>
    `arg_passthru` --- включить проброс URL-параметров, логический.

* - `filter_level`
  - Целочисленный или строка
  - Уровень фильтрации, один из:<br>
    0 или `Off` --- минимальный;<br>
    1 или `Low` --- низкий;<br>
    2 или `Medium` --- средний;<br>
    3 или `High` --- высокий;<br>
    4 или `Paranoid` --- паранойя.

* - `enable_fp`
  - Логический
  - Включить фильтрацию по JavaScript-отпечаткам и обучение модели VLA™.

* - `enable_ua`
  - Логический
  - Включить фильтрацию по встроенным спискам user agent.

* - `require_unique`
  - Логический
  - Пропускать только уникальных посетителей.

* - `require_touch`
  - Логический
  - Требовать поддержку touchscreen (нужна фильтрация по JS-отпечаткам)

* - `allow_apps`
  - Логический
  - Разрешить трафик из мобильных приложений.

* - `allow_embed`
  - Логический
  - Разрешить трафик из фреймов (в том числе iframe), элементов embed и object.

* - `countries`
  - Массив строк
  - Разрешенные коды стран в [двухбуквенном формате](https://ru.wikipedia.org/wiki/ISO_3166-1_alpha-2).

* - `os`
  - Массив строк
  - Разрешенные операционные системы.

* - `browsers`
  - Массив строк
  - Разрешенные браузеры.

* - `engines`
  - Массив строк
  - Разрешенные движки браузера.

* - `languages`
  - Массив строк
  - Разрешенные коды языков браузера.

* - `timezones`
  - Массив целых чисел
  - Разрешенные часовые пояса в виде часовых сдвигов относительно UTC.

* - `tz_match_ip`
  - Логический
  - Проверять соответствие часового пояса браузера и местоположения.

* - `skipClicks`
  - Целочисленный
  - Число первых переходов, которые будут отфильтрованы (отложенный запуск).

* - `skip_clicks_mode`
  - Строка
  - Режим пропуска переходов, один из:<br>
    `all` --- все: счетчик пропущенных переходов уменьшается при каждом переходе;<br>
    `money` --- контент: счетчик переходов уменьшается с каждым легитимным посетителем;<br>
    `safe` --- белая: счетчик переходов уменьшается с каждым ботом, модератором и т.п.

* - `ip_list_mode`
  - Строка
  - Режим фильтрации, один из:<br>
    `black` --- черный: блокировать IP-адреса/ASN из черного списка, если их нет в белом списке;<br>
    `white` --- белый: блокировать IP-адреса/ASN из черного списка или не из белого списка;<br>
    `special` --- специальный: блокировать IP-адреса/ASN из черного списка, пропускать из белого списка.

* - `ip_on_review`
  - Логический
  - Заносить все IP-адреса в черный список в режиме "Модерация".

* - `extrapolate_ipv4`
  - Целочисленный в интервале 0 и 255
  - IPv4-экстраполяция.

* - `extrapolate_ipv6`
  - Целочисленный в интервале 0 и 64
  - IPv6-экстраполяция.

* - `cost_parameter`
  - Строка
  - Имя параметра цены перехода.

* - `sid_parameter`
  - Строка
  - Имя параметра субаккаунта.

* - `cid_parameter`
  - Строка
  - Имя параметра идентификатора перехода.

* - `url_rules`
  - Массив объектов
  - URL-правила.  См. [структуру объекта URL-правила](#url-rule-object).

* - `schedule`
  - Массив объектов
  - Расписание.  См. [структуру объекта расписания](#schedule-rule-object).

* - `ua_list_mode`
  - Строка
  - Режим фильтрации user agent, один из:<br>
    `black` --- черный: блокировать user agent из черного списка, если их нет в белом списке;<br>
    `white` --- белый: блокировать user agent из черного списка или не из белого списка;<br>
    `special` --- специальный: блокировать user agent из черного списка, пропускать из белого списка.

* - `ua_blacklist`
  - Строка или массив строк
  - Черный список user agent.

* - `ua_whitelist`
  - Строка или массив строк
  - Белый список user agent.

* - `referer_regex`
  - Строка
  - Регулярное выражение для фильтрации по referrer-у.
:::

Пример:

```json
{
  "stream_id": "1eacc6d0-875f-6f5c-bff8-00162501c2b4",
  "account_id": "1eaa2ce5-d4dd-63ec-b8a4-00162501c2b4",
  "name": "Example Stream",
  "tags": ["Tag1", "Tag2"],
  "mode": "Filter",
  "notes": "This is an example comment.",
  "money_pages": [
    {
      "page": "https://example.com/offer1?clid={clickid}",
      "action": "302",
      "arg_passthru": true,
      "weight": 10,
      "enabled": true
    },
    {
      "page": "https://example.com/offer2?clid={clickid}",
      "action": "302",
      "arg_passthru": true,
      "weight": 20,
      "enabled": true
    }
  ],
  "rotator": "Split",
  "safe_pages": [
    {
      "page": "safe.html",
      "action": "local",
      "arg_passthru": true
    }
  ],
  "filter_level": "High",
  "enable_fp": true,
  "enable_ua": true,
  "require_unique": true,
  "require_touch": false,
  "allow_apps": false,
  "allow_apps": true,
  "countries": ["AE", "HK"],
  "os": ["iOS", "macOS"],
  "browsers": ["Google Chrome"],
  "engines": ["Blink"],
  "languages": ["en", "fr", "es"],
  "timezones": [-5, -6, -7],
  "tz_match_ip": true,
  "ip_on_review": true,
  "cost_parameter": "cost",
  "sid_parameter": "zoneid",
  "cid_parameter": "clickid",
  "url_rules": [
    {
      "param": "zoneid",
      "op": "REGEX",
      "arg": "[[:alnum:]-_]{8,}",
      "enabled": true
    }
  ],
  "schedule": [
    {
      "days": ["Sun", "Sat"],
      "since": "15:30:00",
      "till": "22:15:00"
    }
  ]
}
```

### Объект URL-правила

Объект URL-правила имеет следующую структуру:

```json
{
  "param": "gclid",
  "op": "REGEX",
  "arg": "[[:alnum:]-_]{55,}",
  "enabled": true
}
```

:::{list-table} Свойства объекта URL-правила
:header-rows: 1

* - Свойство
  - Тип
  - Описание

* - `param`
  - Строка
  - Имя URL-параметра, с которым работает правило.

* - `op`
  - Строка
  - Оператор правила. См. таблицу операторов ниже.

* - `arg`
  - Строка
  - Аргумент оператора. См. таблицу операторов ниже.

* - `enabled`
  - Логический
  - Включает правило.
:::

:::{list-table} Операторы URL-правил
:header-rows: 1

* - Оператор
  - Аргумент
  - Описание

* - `ASSIGN`
  - Необязательный
  - Назначает параметру значение аргумента.

* - `RENAME`
  - Необязательный
  - Меняет имя параметра на значение аргумента.

* - `DELETE`
  - Игнорируется
  - Удаляет параметр.

* - `EXISTS`
  - Игнорируется
  - Проверяет, что параметр существует.

* - `NEXISTS`
  - Игнорируется
  - Проверяет, что параметр не существует.

* - `REGEX`
  - Необязательный
  - Проверяет, что значение параметра совпадает с регулярным выражением в аргументе.

* - `IREGEX`
  - Необязательный
  - Проверяет, что значение параметра совпадает с регулярным выражением в аргументе (без учета регистра).

* - `NREGEX`
  - Необязательный
  - Проверяет, что значение параметра не совпадает с регулярным выражением в аргументе.

* - `NIREGEX`
  - Необязательный
  - Проверяет, что значение параметра не совпадает с регулярным выражением в аргументе (без учета регистра).

* - `EQ`
  - Необязательный
  - Проверяет, что значение параметра равно аргументу.

* - `NEQ`
  - Необязательный
  - Проверяет, что значение параметра не равно аргументу.

* - `GT`
  - Необязательный
  - Проверяет, что значение параметра меньше аргумента.

* - `GE`
  - Необязательный
  - Проверяет, что значение параметра больше или равно аргументу.

* - `LT`
  - Необязательный
  - Проверяет, что значение параметра меньше аргумента.

* - `LE`
  - Необязательный
  - Проверяет, что значение параметра меньше или равно аргументу.
:::

### Объект расписания

Объект расписания имеет следующую структуру:

```json
{
  "days": ["Sun", "Sat"],
  "since": "15:30:00",
  "till": "22:15:00"
}
```

:::{list-table} Свойства объекта расписания
:header-rows: 1

* - Свойство
  - Тип
  - Описание

* - `days`
  - Массив строк
  - Дни недели, к которым относится правило:<br>
    `Mon` --- понедельник<br>
    `Tue` --- вторник<br>
    `Wed` --- среда<br>
    `Thu` --- четверг<br>
    `Fri` --- пятница<br>
    `Sat` --- суббота<br>
    `Sun` --- воскресенье

* - `since`
  - Строка
  - Пропускать посетителей с HH:MM:SS, например `15:30:00`.

* - `till`
  - Строка
  - Пропускать посетителей до HH:MM:SS, например `22:15:00`.
:::


### Список потоков

```
GET /streams
```

Эта точка вызова возвращает список потоков в аккаунте. Список разбивается на страницы, а потоки в нем
выводятся в возрастающем [лексикографическом порядке](https://ru.wikipedia.org/wiki/%D0%9B%D0%B5%D0%BA%D1%81%D0%B8%D0%BA%D0%BE%D0%B3%D1%80%D0%B0%D1%84%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B9_%D0%BF%D0%BE%D1%80%D1%8F%D0%B4%D0%BE%D0%BA)
их имен.

Поддерживаемые URL-параметры:

* `page=1` --- задает номер страницы для отображения, по умолчанию 1;
* `per-page=20` --- задает число потоков на страницу, от 1 до 100, по умолчанию 20;
* `name=substr` --- выводит только те потоки, в имени которых содержится `substr` (без учета регистра).

### Данные о списке потоков

```
HEAD /streams
```

Эта точка вызова возвращает заголовки с общей информацией о числе потоков и страниц:

* `X-Pagination-Total-Count: 1000` --- общее число потоков;
* `X-Pagination-Page-Count: 50` --- общее число страниц.

Поддерживаемые URL-параметры:

* `per-page=20` --- задает число потоков на страницу, от 1 до 100, по умолчанию 20.

### Получение потока

```
GET /streams/<id>
```

Эта точка вызова возвращает поток `<id>`.

### Создание потока

```
POST /streams
```

Эта точка вызова создает и возвращает новый поток. Укажите объект потока в теле запроса.
Все свойства объекта потока имеют значения по умолчанию, поэтому вам нужно указывать только те свойства,
значения которых вы хотите задать явно, например `{"name":"My Stream"}`.

### Обновление потока

```
PATCH /streams/<id>
```

Эта точка вызова обновляет поток `<id>`. Укажите объект потока в в теле запроса.
Вам нужно указать только те свойства, значения которых вы хотите изменить; все остальные свойства
останутся неизменными.

### Копирование потока

```
COPY /streams/<id>
```

Эта точка вызова копирует поток `<id>`. Укажите объект потока в теле запроса ---
указанные в нем свойства заменят свойства в скопированном потоке, что сэкономит вам дополнительный
вызов точки `PATCH`. Если свойства изменять не требуется, то укажите пустой объект `{}`.

### Удаление потока

```
DELETE /streams/<id>
```

Эта точка вызова удаляет поток `<id>`.

## PHP-файлы интеграции

Вы можете скачать файлы `index.php`, `filter.php` и `ajax.php` для любого потока при помощи запросов:

* `index.php` и `filter.php` (один и тот же файл) --- `GET https://clients.adspect.ai/getindex.php?sid=<id>`
* `ajax.php` --- `GET https://clients.adspect.ai/getindex.php?sid=<id>&mode=ajax`

Вместо `<id>` укажите ID конкретного потока в Adspect.

## Гостевой доступ к статистике

Гостевой доступ к статистике позволяет третьим лицам просматривать заранее определенные фрагменты вашей статистики
без необходимости входа в систему. Управление гостевым доступом производится при помощи сохраненных запросов:

1. Создается сохраненный запрос к статистике, в котором указываются параметры выгрузки отчета (даты и фильтры);
2. В ответ API передает ID сохраненного запроса;
3. Гость может выполнить этот сохраненный запрос на [странице статистики](https://clients.adspect.ai/reporting),
   указав ID сохраненного запроса в соответствующем поле, либо перейдя по ссылке вида
   `https://clients.adspect.ai/reporting?query_id=<id>`, где вместо `<id>` подставляется ID сохраненного запроса.

:::{note}
Можно создавать не более 100 сохраненных запросов.
:::

Сохраненный запрос представляется в виде объекта со следующими свойствами:

:::{list-table} Свойства сохраненного запроса
:header-rows: 1

* - Свойство
  - Тип
  - Описание

* - `query_id`
  - Строка
  - ID сохраненного запроса. Только для чтения.

* - `owner_id`
  - Строка
  - ID аккаунта владельца запроса. Только для чтения.

* - `date_from`
  - Целое число или строка
  - Минимальное Unix-время, начиная с которого выгружается отчет.

* - `date_to`
  - Целое число или строка
  - Максимальное Unix-время, до которого выгружается отчет.

* - `time_zone`
  - Строка
  - Часовой пояс по умолчанию для отображения дат и времени.

* - `group_by`
  - Массив строк
  - Разбивка воронки продаж по умолчанию.

* - `stream_id`
  - Массив строк
  - Фильтр по ID потоков.

* - `ip_address`
  - Массив строк
  - Фильтр по IP-адресам.

* - `asn`
  - Массив целых чисел или строк
  - Фильтр по [номерам автономных систем (ASN)](https://ru.wikipedia.org/wiki/%D0%90%D0%B2%D1%82%D0%BE%D0%BD%D0%BE%D0%BC%D0%BD%D0%B0%D1%8F_%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0_(%D0%98%D0%BD%D1%82%D0%B5%D1%80%D0%BD%D0%B5%D1%82)). ASN могут быть заданы строками вида `AS65536` или `AS1.10`.

* - `country_code`
  - Массив строк
  - Фильтр по кодам стран в [двухбуквенном формате](https://ru.wikipedia.org/wiki/ISO_3166-1_alpha-2).

* - `os`
  - Массив строк
  - Фильтр по операционным системам.

* - `browser`
  - Массив строк
  - Фильтр по браузерам.

* - `engine`
  - Массив строк
  - Фильтр по движкам браузера.

* - `sub_id`
  - Массив строк
  - Фильтр по субаккаунтам.

* - `click_id`
  - Массив строк
  - Фильтр по ID переходов.

* - `mode`
  - Массив строк
  - Фильтр по режимам потока.

* - `target`
  - Массив целых чисел
  - Фильтр по показанным страницам:<br>
    0 --- белая страница;<br>
    1--255 --- контент-страница с соответствующим порядковым номером.
:::

Все поля необязательны. Если тот или иной фильтр не требуется, то его можно не указывать, либо задать значение `null`
(или пустой массив `[]` там, где типом значения является массив).

Пример:

```json
{
  "query_id": "878efbf1-0fb9-4c69-ad36-40e714b0eeeb",
  "owner_id": "1eb5991f-a25b-68f4-b171-00162501c2b4",
  "date_from": 1577836800,
  "date_to": null,
  "time_zone": "Asia/Dubai",
  "group_by": [],
  "account_id": ["1eb5991f-a25b-68f4-b171-00162501c2b4"],
  "stream_id": ["6792f6ce-f153-439f-b223-f58749f1210f"],
  "ip_address": [],
  "asn": [],
  "country_code": ["HK", "AE"],
  "os": ["iOS", "macOS"],
  "browser": ["Apple Safari"],
  "engine": [],
  "sub_id": [],
  "click_id": [],
  "mode": ["Filter"],
  "target": []
}
```

### Список запросов

```
GET /reports
```

Эта точка вызова возвращает список запросов в аккаунте, разбитый на страницы.

Поддерживаемые URL-параметры:

* `page=1` --- задает номер страницы для отображения, по умолчанию 1;
* `per-page=20` --- задает число запросов на страницу, от 1 до 100, по умолчанию 20;

### Данные о списке запросов

```
HEAD /reports
```

Эта точка вызова возвращает заголовки с общей информацией о числе запросов и страниц:

* `X-Pagination-Total-Count: 1000` --- общее число запросов;
* `X-Pagination-Page-Count: 50` --- общее число страниц.

Поддерживаемые URL-параметры:

* `per-page=20` --- задает число запросов на страницу, от 1 до 100, по умолчанию 20.

### Получение запроса

```
GET /reports/<id>
```

Эта точка вызова возвращает сохраненный запрос `<id>`.

### Создание запроса

```
POST /reports
```

Эта точка вызова создает и возвращает новый запрос. Укажите объект запроса в теле HTTP-запроса.

### Обновление запроса

```
PATCH /reports/<id>
```

Эта точка вызова обновляет запрос `<id>`. Укажите объект запроса в теле HTTP-запроса.
Вам нужно указать только те свойства, значения которых вы хотите изменить; все остальные свойства
останутся неизменными.

### Копирование запроса

```
COPY /reports/<id>
```

Эта точка вызова копирует запрос `<id>`. Укажите объект запроса в теле HTTP-запроса ---
указанные в нем свойства заменят свойства в скопированном запросе, что сэкономит вам дополнительный
вызов точки `PATCH`.

### Удаление запроса

```
DELETE /reports/<id>
```

Эта точка вызова удаляет запрос `<id>`.
