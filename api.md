# REST API

Adspect предоставляет REST API для программного управления потоками. API использует JSON-кодирование
данных и поддерживает несколько методов для всех основных операций над потоками. Для аутентификации
используется [HTTP-аутентификация типа Basic](https://developer.mozilla.org/ru/docs/Web/HTTP/%D0%90%D0%B2%D1%82%D0%BE%D1%80%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F#Basic_authentication_scheme),
в которой ключ API передается в качестве имени пользователя, а пароль оставляется пустым. Ваш ключ API
находится в вашем профиле.

Каждый запрос к API должен содержать два обязательных заголовка:

1. `Content-Type: application/json` для обозначения JSON-кодирования данных;
2. `Authorization: Basic ###` для аутентификации, где `###` заменяется на `base64(ключ API + ":")`.

Базовый URL для всех API-методов --- `https://api.adspect.net/v1/`. Описания методов ниже указывают пути
относительно этого базового URL. ID потоков в API-методах должны указываться как полные UUID, например
`cbb360ff-5a28-41d0-9ac8-9889a01149fa`.

На текущий момент реализованы только методы для управления потоками.

## Формат потоков

Каждый поток представляется в виде JSON-объекта, который содежит следующие свойства:

* `stream_id` --- полный ID потока в формате UUID;
* `account_id` --- полный ID аккаунта в формате UUID, только для чтения;
* `name` --- название потока, строка;
* `mode` --- режим потока, строка, одна из `Filter`, `Review`, `Money` или `White`;
* `money_pages` --- массив из одного или более (до 254) объектов контент-страниц, каждый в следующем формате:
  * `page` --- целевой URL или имя файла для отображения, строка;
  * `arg_passthru` --- флаг проброса URL-параметров на данную контент-страницу, логический;
  * `weight` --- относительный вес страницы для сплит-тестирования, целочисленный;
  * `enabled` --- включена ли данная контент-страница, логический;
* `white_page` --- URL белой страницы или имя файла для отображения, строка;
* `white_arg_passthru` --- флаг проброса URL-параметров на белую страницу, логический или целочисленный;
* `ml_precision` --- точность VLA в процентах, целочисленный;
* `cost_parameter` --- имя параметра цены клика, строка;
* `sid_parameter` --- имя параметра sub ID, строка;
* `cid_parameter` --- имя параметра click ID, строка;
* `enable_fp` --- флаг фильтрации по JavaScript-отпечаткам, логический или целочисленный;
* `paranoid` --- флаг режима паранойи, логический или целочисленный;
* `allow_apps` --- разрешены ли мобильные приложения, логический или целочисленный;
* `countries` --- массив строк разрешенных стран в формате [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2);
* `os` --- массив строк разрешенных операционных систем:
  * `Android 1`
  * `Android 2`
  * `Android 3`
  * `Android 4`
  * `Android 5`
  * `Android 6`
  * `Android 7`
  * `Android 8`
  * `Android 9`
  * `Android 10`
  * `Android 11`
  * `iOS`
  * `macOS`
  * `Linux`
  * `Other`
  * `Windows XP`
  * `Windows Vista`
  * `Windows 7`
  * `Windows 8`
  * `Windows 8.1`
  * `Windows 10`
  * `Windows Other`
* `browsers` --- массив строк разрешенных браузеров:
  * `Apple Safari`
  * `Google Chrome`
  * `Internet Explorer`
  * `Microsoft Edge`
  * `Mozilla Firefox`
  * `Opera`
  * `Other`
  * `Samsung Internet`
  * `UC Browser`
  * `WebView`
  * `Yandex Browser`
* `engines` --- массив строк разрешенных движков браузеров:
  * `Blink`
  * `EdgeHTML`
  * `Gecko`
  * `Other`
  * `Presto`
  * `Trident`
  * `WebKit`
* `languages` --- массив строк кодов разрешенных языков браузера;
* `timezones` --- массив разрешенных часовых поясов --- целочисленных часовых сдвигов относительно UTC;
* `tz_match_ip` --- проверять соответствие часового пояса браузера и местоположения, логический или целочисленный;
* `url_rules` --- массив URL-правил (до 64), каждое из которых в следующем формате:
  * `param` --- имя URL-параметра, строка;
  * `op` --- оператор правила, один из:
    * `EXISTS` --- параметр существует;
    * `NEXISTS` --- параметр не существует;
    * `REGEX` --- значение совпадает с регулярным выражением;
    * `IREGEX` --- значение совпадает с регулярным выражением (без учета регистра);
    * `NREGEX` --- значение не совпадает с регулярным выражением;
    * `NIREGEX` --- значение не совпадает с регулярным выражением (без учета регистра);
    * `EQ` --- значение равно аргументу;
    * `NEQ` --- значение не равно аргументу;
    * `GT` --- значение больше аргумента;
    * `GE` --- значение больше или равно аргументу;
    * `LT` --- значение меньше аргумента;
    * `LE` --- значение меньше или равно аргументу;
    * `ASSIGN` --- назначить новое значение параметру;
    * `RENAME` --- переименовать параметр;
    * `DELETE` --- удалить параметр;
  * `arg` --- аргумент правила, строка;
  * `enabled` --- включено ли правило, логический;
* `ua_regex` **(устарело, к удалению)** --- регулярное выражение для фильтрации по user agent, строка;
* `referer_regex` **(устарело, к удалению)** --- регулярное выражение для фильтрации по referrer, строка;
* `ip_on_review` --- заносить все IP-адреса в черный список в режиме "Модерация", логический или целочисленный.

Пример:

```json
{
   "stream_id": "1eacc6d0-875f-6f5c-bff8-00162501c2b4",
   "account_id": "1eaa2ce5-d4dd-63ec-b8a4-00162501c2b4",
   "name": "Example stream",
   "mode": "Filter",
   "money_pages": [
      {
         "page": "https://example.com/offer1?clid={clickid}",
         "arg_passthru": true,
         "weight": 10,
         "enabled": true
      },
      {
         "page": "https://example.com/offer2?clid={clickid}",
         "arg_passthru": true,
         "weight": 20,
         "enabled": true
      }
   ],
   "white_page": "white.html",
   "white_arg_passthru": 0,
   "ml_precision": 95,
   "cost_parameter": "cost",
   "sid_parameter": "sourceid",
   "cid_parameter": "",
   "enable_fp": 1,
   "paranoid": 0,
   "allow_apps": 1,
   "countries": [
      "CA",
      "US"
   ],
   "os": [
      "iOS",
      "macOS"
   ],
   "browsers": [
      "Google Chrome"
   ],
   "engines": [
      "Blink"
   ],
   "languages": [
      "en",
      "fr",
      "es",
   ],
   "timezones": [
      -5,
      -6,
      -7,
   ],
   "tz_match_ip": 1,
   "url_rules": [
      {
         "param": "secretkey",
         "op": "EQ",
         "arg": "4gHzQvF2IoqeQ",
         "enabled": true
      }
   ],
   "ua_regex": "",
   "referer_regex": "",
   "ip_on_review": 1
}
```

## GET /streams

Возвращает массив всех потоков в аккаунте.

## GET /streams/&lt;id&gt;

Возвращает указанный поток.

## POST /streams

Создает и возвращает новый поток. Укажите объект потока в JSON-формате в теле запроса.

## PATCH /streams/&lt;id&gt;

Обновляет поток. Укажите объект потока в JSON-формате в теле запроса.

## DELETE /streams/&lt;id&gt;

Удаляет поток.

## index.php, filter.php и ajax.php

Вы можете скачать файлы `index.php`, `filter.php` и `ajax.php` для любого потока при помощи запросов:

* `index.php` / `filter.php` --- `GET https://clients.adspect.ai/getindex.php?sid=<id>&mode=<mode>`
* `ajax.php` --- `GET https://clients.adspect.ai/getindex.php?sid=<id>&mode=ajax`

Где `<mode>` является одним из:

* `redirect` --- перенаправление на внешний URL при помощи кода ответа HTTP 302;
* `iframe` --- отображение внешнего URL на вашем домене в тэге `<iframe>`;
* `proxy` --- отображение внешнего URL на вашем домене путем HTTP-проксирования.

Файлы `index.php` и `filter.php` идентичны.
