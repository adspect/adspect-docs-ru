# Настройка потоков

Управление трафиком в Adspect организовано в контексте потоков. Поток --- это канал прохождения трафика, которым
можно управлять как единым целым, подобно кампании в рекламной сети или схеме в TDS. Потоки управляются в разделе "Потоки"
вашего личного кабинета и создаются по кнопке "Создать поток". Далее мы рассмотрим назначение каждой настройки в потоке.

**Обратите внимание**, что настройки по умолчанию являются адекватными для большинства источников трафика и сценариев
использования. Вам не нужно заполнять все доступные поля; обычно достаточно указать только контент и белую страницу,
а все остальное система Adspect сделает за вас сама.

## Название

Название потока --- это просто любое читабельное имя, которое позволит вам быстро отличить один поток от другого.
Мы рекомедуем называть потоки по именам рекламных сетей и кампаний в них для сохранения ясности связей между источниками
трафика и соответствующими потоками в Adspect. Мы также рекомендуем создавать отдельный поток для каждой страны,
для простоты получения статистики с разбивкой по странам.

## Режим

Это режим работы потока, главный рычаг управления. Всего есть четыре режима:

* "Фильтр" --- основной режим работы любого потока, в котором мы осуществляем фильтрацию хороших посетителей от опасных
  в реальном времени. Все технологии фильтрации Adspect, в том числе [VLA™](vla.md), работают именно в этом режиме.

* "Модерация" --- этот режим предназначен для тех моментов, когда рекламные кампании находятся на проверке у модераторов
  рекламных сетей. Каждому посетителю будет показана белая страница. В этом режиме доступны дополнительные функции,
  настройка которых будет описана ниже в этой главе.

* "Контент" --- вспомогательный режим, в котором всем посетителям показывается страница с основным контентом.
  Режим может быть удобен для тестирования доступности контент-страницы.

* "Белая страница" --- вспомогательный режим, в котором всем посетителям показывается "белая" страница.
  Режим может быть удобен для тестирования доступности белой страницы. Рекомендуем переводить в этот режим потоки
  при остановке их кампаний, так как система модерации многих рекламных сетей работает даже тогда,
  когда ваши кампании остановлены.

"Модерация" является режимом по умолчанию для вновь созданных потоков. Вам *следует* использовать этот режим при
прохождении модерации в рекламных сетях. После того, как кампания одобрена, переключите поток в режим "Фильтр"
прежде, чем сеть начнет поставлять трафик.

## Контент

Контент --- это ваш настоящий лэндинг или CPA-оффер, который вы собираетесь рекламировать. Словом, это то,
что должно принести вам прибыль. Вы можете указать до 254 контент-страниц для сплит-тестирования. Трафик будет распределяться
между ними в соответствии с правилами выбранного ротатора (см. "Ротатор" ниже).

Есть два типа значений, которые здесь можно указать: имя файла лэндинг-страницы или внешний URL. Имя файла страницы
это наиболее предпочтительный способ отображения контента. Это имя HTML- или PHP-файла вашего настоящего лэндинга,
который *должен* располагаться в той же папке, что и файл `index.php`, то есть в корневой папке вашего лэндинга.
Несоблюдение этого правила поломает отображение вашего лэндинга в браузерах посетителей.

Имя файла не должно быть легко угадываемым, иначе целеустремленные модераторы или конкуренты могут его угадать,
со всеми вытекающими последствиями. Выберите случайное длинное имя.

*Не называйте* файл страницы контента `index.html` или `index.htm`! Не считая того, что это легко угадываемые имена
и позволяют легко "пробить клоаку", они также могут конфликтовать с вашей конфигурацией веб-сервера и привести
к непредвиденным проблемам.

В сумме получается следующее: если у вас есть папка с лэндингом, основная страница которого называется `index.php`
(как это часто бывает), то сначала переименуйте файл `index.php` во что-то трудно угадываемое, например `re3NBX1XtH.php`,
а после создания потока скачайте наш файл `index.php` в ту же папку. Этот файл `index.php` переключит клик на целевой
`re3NBX1XtH.php`, если посетитель будет расценен как благонадежный.

Вы также можете указать URL конечной страницы, например прямую ссылку на оффер из партнерской программы. Это может
быть оптимально для некоторых кампаний, однако помните, что внешний URL означает лишний HTTP-редирект, со всеми
вытекающими последствиями --- потенциальным увеличением технических потерь трафика из-за увеличения времени
обработки клика, особенно на низкокачественных рекламных форматах вроде popunder-а.

Также поддерживаются различные не-HTTP URL-ы, при помощи которых вы можете выполнять специализированные задачи
на устройствах ваших посетителей. Несколько распространенных примеров:

* `mailto:user@example.com` откроет почтовую программу для составления e-mail на указанный адрес;
* `tel:+08001234567` наберет указанный номер на мобильных устройствах и некоторых десктопах с ПО для телефонии;
* `market://details?id=app` откроет страницу мобильного приложения в Google Play.

Эта функциональность особенно полезна для работы с т.н. deep-ссылками, которые ведут на контент внутри мобильных приложений.

### "Парам"

"Парам" --- это сокращение от "проброс URL-параметров". Если проброс параметров включен, то все параметры из входящей
ссылки будут добавлены к ссылке или имени файла контент-страницы.

Допустим, ваша страница указана в виде ссылки:
```
https://example.com/?utm_campaign=sweeps
```

Посетитель переходит на файл `index.php` потока по ссылке:
```
https://tracker.test/lander/index.php?utm_medium=ppc&utm_source=search
```

Если посетитель будет посчитан благонадежным, то он будет перенаправлен на контент-страницу с объединением параметров
из обеих ссылок выше:
```
https://example.com/?utm_campaign=sweeps&utm_medium=ppc&utm_content=search
```

### Вес

Каждая контент-страница имеет свой абстрактный вес, который по умолчанию равен 10. Этот параметр учитывается при сплит-тестировании
нескольких контент-страниц. Конкретное влияние этого параметра на распределение трафика зависит от выбранного ротатора
(см. "Ротатор" ниже).

### "Вкл"

Настройка "вкл" позволяет вам включать и выключать отдельные контент-страницы.

### URL-макросы

Adspect поддерживает макросы для использования в полях "Контент" и "Белая страница" (а также в URL-правилах, как будет
описано далее в этой главе):

* `{ip}` --- IP-адрес посетителя;
* `{asn}` --- номер [автономной системы](https://ru.wikipedia.org/wiki/%D0%90%D0%B2%D1%82%D0%BE%D0%BD%D0%BE%D0%BC%D0%BD%D0%B0%D1%8F_%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0_(%D0%98%D0%BD%D1%82%D0%B5%D1%80%D0%BD%D0%B5%D1%82))
  посетителя;
* `{agent}` --- [строка user agent](https://ru.wikipedia.org/wiki/User_agent) посетителя;
* `{referrer}` --- [referrer](https://developer.mozilla.org/ru/docs/Web/API/Document/referrer) посетителя;
* `{clickid}` --- уникальный идентификатор клика (внешний из параметра ссылки, либо сгенерированный Adspect);
* `{country}` --- [ISO 3166-1 alpha-2](https://ru.wikipedia.org/wiki/ISO_3166-1_alpha-2) код страны посетителя;
* `{os}` --- операционная система посетителя и ее версия в случае Windows и Android;
* `{browser}` --- название браузера посетителя;
* `{engine}` --- название движка браузера посетителя;
* `{epoch}` --- [Unix-время](https://ru.wikipedia.org/wiki/Unix-%D0%B2%D1%80%D0%B5%D0%BC%D1%8F) перехода;
* `{tags}` --- [теги обработки перехода](reporting.html#id3), если есть;
* `{p:parameter}` --- значение указанного параметра из URL запроса.

При отображении страниц без редиректа вы также можете добавить параметры ссылки с макросами после имени файла для
отображения, и они будут переданы в PHP, где будут доступны через
[суперглобальную переменную `$_GET`](https://www.php.net/manual/ru/reserved.variables.get.php).

Пример использования в ссылке для редиректа:
```
https://example.com/offer?clickid={clickid}&geo={country}&os={os}
```

Пример использования при отображении без редиректа:
```
page.php?clickid={clickid}&geo={country}&os={os}
```
```php
<!-- В коде файла page.php -->
<a href="https://example.com/offer?clickid=<?= $_GET['clickid'] ?>">Offer</a>
```

## Ротатор

Ротатор определяет алгоритм ротации контент-страниц, т.е. то, как система выбирает, какую контент-страницу показать
каждому конкретному посетителю. Если указана только одна контент-страница, то выбор ротатора ни на что не влияет.
На данный момент Adspect поддерживает два ротатора: "сплит" и "таймер".

### Ротатор "сплит"

Это ротатор по умолчанию, который распределяет трафик между включенными контент-страницами в соответствии с их весами:
чем больше вес страницы, тем пропорционально больше трафика она получит.

Например, если у вас есть три контент-страницы с весами 10, 15 и 25, то первая страница получит 20 % от всего
целевого трафика, вторая страница получит 30 %, а третья --- 50 %.

Так как этот ротатор имеет в основе генератор псевдослучайных чисел (PRNG), при небольшом числе входящих кликов могут
быть "перекосы" в распределении трафика относительно заданных весов. Однако, математические свойства PRNG гарантируют,
что на дистанции распределение трафика максимально точно достигнет заданных весов.

### Ротатор "таймер"

Этот ротатор переключается между контент-страницами, используя вес как число секунд, на которое активируется та или иная страница.

Например, если у вас указаны три страницы с весами 60, 120 и 180, то первая страница будет показываться посетителям в течение
одной минуты, затем ротатор будет 2 минуты показывать вторую страницу, затем переключится на третью и будет отображать ее 3
минуты, а затем снова вернется к первой, и так далее.

Этот ротатор удобен для автоматической смены доменов по времени.

## Белая страница

Это безопасная страница, которую можно показывать модераторам, роботам, скрейперам и т.п. Она не должна содержать
никакой чувствительный контент, который может поставить вашу рекламную кампанию под угрозу, например из-за нарушения
правил рекламной сети. Все, описанное выше для страницы контента, также относится и к белой странице: вы можете
использовать URL или имя файла для отображения. В случае с файлом, если ваша контент-страница также настроена как
файл, вам фактически потребуется совместить два лэндинга в одной папке, с разными именами HTML- или PHP-файлов.

Мы настоятельно рекомендуем использовать полноценный собственный лэндинг в качестве белой страницы. Это связано с тем,
что некоторые рекламные сети с подозрением относятся к любым редиректам, подвергая содержащие их кампании более тщательной
проверке, а некоторые и вовсе запрещают редиректы.

## Пробрасывать URL-параметры на белую страницу

Если проброс параметров включен, то все параметры из входящей ссылки будут добавлены к ссылке или имени файла белой страницы.
Эта настройка работает по тому же принципу, что настройка "парам" для контент-страниц.

## VLA™

VLA™ является аббревиатурой от "Virtual Learning Appliance". Это торговое название собственной системы машинного обучения
в основе технологии фильтрации трафика Adspect. Вы можете ознакомиться с системой более детально в [главе о VLA](vla.md).
95% является оптимальным начальным значением для точности VLA.

## Sub ID

Sub ID --- это параметр ссылки, по которому можно делать разбивку в статистике, выбрав критерий группировки "Sub ID".
Статистика подробно описана в [отдельной главе](reporting.md) данного руководства.

Принцип работы проще всего показать на примере. Возьмем рекламную сеть, у которой есть понятие зон --- номеров
площадок, на которых показываются рекламные объявления. Номер зоны, с которой пришел клик, помещается в трекинговую
ссылку для передачи в трекер при помощи макроса, например `{zoneid}`:

```
https://tracker.test/lander/index.php?subid={zoneid}
```

Для каждого клика рекламная сеть заменит этот макрос `{zoneid}` фактическим номером зоны, из которой пришел клик,
а далее трекер извлечет его из кликовой ссылки для сбора статистики. В данном примере `subid` является параметром
ссылки, в котором содержится номер зоны. Если вы укажете `subid` в поле "Sub ID" в потоке, то сможете получать
статистику по каждой отдельной зоне в потоке. Это может быть очень полезным для сбора черных списков зон с высокой
плотностью ботов.

В качестве sub ID можно использовать и другие атрибуты: страну, платформу (десктоп/мобайл), версию ОС, вообще любой
параметр ссылки. Вы можете комбинировать несколько макросов для получения составного параметра для sub ID, например:

```
https://tracker.test/lander/index.php?subid={zoneid}-{platform}
```

В этом примере каждый субаккаунт будет парой из зоны и платформы устройства посетителя.

## Click ID

Настройка Click ID работает по тому же принципу, что и Sub ID, но используется для уникальной идентификации отдельных
кликов с помощью параметра, генерируемого рекламной сетью или трекером. Если параметр указан, то его значения
извлекаются из ссылки при прохождении клика через Adspect и записываются в статистику вместе с другими показателями.
Это позволяет находить отдельные клики в сырых покликовых отчетах, которые можно выгрузить в формате CSV. Одним из
способов применения может быть сбор доказательной базы для выявления кликфрода в трафике.

Если параметр не указан, то Adspect сам сгенерирует индентификатор перехода для использования в [трекере](tracker.md).
Далее идентификатор перехода может быть передан на контент или белую страницу при помощи макроса `{clickid}`.

## Режим паранойи

Режим паранойи подключает дополнительные строгие проверки JavaScript-отпечатков, а также обширные черные списки IP-адресов
совокупным объемом 2 миллиарда адресов IPv4. Эти меры считаются "параноидальными" в том смысле, что несут в себе более
высокие риски ложноположительных срабатываний, но в то же время они предоставляют более высокий уровень защиты от модераторов.

**Мы рекомендуем включить этот режим при работе с Facebook и Google Ads.**

## Разрешить трафик из мобильных приложений

Эта настройка говорит нам пропускать трафик из мобильных приложений в общем порядке, не считая его априори фродовым.
Ярким примером такого трафика являются переходы, сделанные из браузера WebView на платформе Android. Этот трафик
является естественным для некоторых нишевых рекламных форматов, но в традиционных форматах рекламы он очень часто
оказывается накруткой (автоматическими кликами, выполняемыми зараженными вирусами мобильными устройствами) и поэтому
должен быть отфильтрован. Включайте настройку только в том случае, если ваш рекламный формат так или иначе основан
на мобильных приложениях.

## Страны, операционные системы, браузеры, движки, языки и часовые пояса

Эти настройки ручного таргетинга позволяют вам ограничить круг потенциальных посетителей контента только указанными
странами, операционными системами, браузерами, движками, языками и часовыми поясами. Обычно следует указывать те же
таргетинги, что и в рекламной кампании. Если та или иная настройка не задана (список пуст), то проверка по ней
не производится.

Настройка часовых поясов ограничена полночасовыми смещениями относительно UTC. Если часовой пояс посетителя смещен
относительно UTC на неполное число часов (например, Индия UTC+5.5), то смещение округляется до ближайшего полного часа
(в примере с Индией до UTC+5).

## Проверять соответствие часового пояса браузера и местоположения

Если эта настройка включена, то Adspect будет отфильтровывать всех посетителей, у которых часовой пояс браузера
не совпадает с часовым поясом их фактического местоположения, определенного при помощи нашей геолокации. Эта проверка
немного повышает вероятность ложноположительных срабатываний, однако значительно улучшает защиту от модераторов и ботов,
использующих VPN- и прокси-сервисы. При включенной проверке описанный выше ручной список часовых поясов игнорируется.
Мы рекомендуем включить эту настройку.

## Расписание

Расписание позволяет вам указывать временные интервалы и дни недели, в которые фильтрация трафика будет включена.
Все посетители в другие временные интервалы или дни будут заблокированы. Расписание включается только если указан
хотя бы один временной интервал. Если в интервале не указаны дни недели, то он применяется ко всем дням.

## Лимит переходов

Это максимальное разрешенное число переходов с одного IP-адреса. 0 означает отсутствие лимита. Посетители с IP-адресов,
превысивших этот лимит, будут отфильтрованы. Вы можете сбросить все счетчики переходов в потоке при помощи кнопки "Сброс".

## Заносить IP-адреса в черный список при достижении лимита переходов

Если эта настройка включена, то все IP-адреса, которые превысили лимит переходов, будут добавлены в черный список
IP/ASN (см. ниже).

## URL-правила

Эта секция позволяет вам создать до 30 собственных правил проверки и изменения URL-параметров. Каждое правило
описывается следующими составными частями:

* Параметр --- имя параметра в ссылке, который будет проверяться или изменяться;
* Оператор --- конкретная проверка или операция, которая будет произведена;
* Аргумент --- аргумент оператора, если он применим (поддерживаются макросы);
* Переключатель "Вкл" --- позволяет включать и выключать отдельные правила.

Поддерживаются следующие операторы:

* `EXISTS` --- проверяет, что параметр существует (аргумент игнорируется);
* `! EXISTS` --- проверяет, что параметр не существует (аргумент игнорируется);
* `REGEX` --- проверяет параметр на совпадение с
  [Perl-совместимым регулярным выражением (PCRE)](http://www.pcre.ru/docs/perl/text/intro/)
  в аргументе (с учетом регистра);
* `REGEX (no case)` --- проверяет параметр на совпадение с регулярным выражением в аргументе (без учета регистра);
* `! REGEX` --- проверяет параметр на несовпадение с регулярным выражением в аргументе (с учетом регистра);
* `! REGEX (no case)` --- проверяет параметр на несовпадение с регулярным выражением в аргументе (без учета регистра);
* =, ≠, >, ≥, <, ≤ --- сравнивают параметр с аргументом; целочисленные и вещественные значения сравниваются как числа,
  строки сравниваются в соответствии с [лексикографическим порядком](https://ru.wikipedia.org/wiki/%D0%9B%D0%B5%D0%BA%D1%81%D0%B8%D0%BA%D0%BE%D0%B3%D1%80%D0%B0%D1%84%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B9_%D0%BF%D0%BE%D1%80%D1%8F%D0%B4%D0%BE%D0%BA);
* `ASSIGN` --- назначает параметру значение из аргумента;
* `RENAME` --- переименовывает параметр в аргумент;
* `DELETE` --- удаляет параметр из ссылки (аргумент игнорируется);

Правила выполняются в следующем порядке:

1. Проверки: правила `EXISTS` и `REGEX`, =, ≠, >, ≥, <, ≤ --- не пройденная проверка отправляет на белую страницу;
2. Правила `ASSIGN`;
3. Правила `RENAME`;
4. Правила `DELETE`.

В аргументах правил поддерживаются все те же макросы, доступные для использования в полях "Контент" и "Белая страница".

## Фильтр user agent

Эта настройка позволяет вам указать собственное [Perl-совместимое регулярное выражение (PCRE)](http://www.pcre.ru/docs/perl/text/intro/)
для фильтрации посетителей по их [строке user agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent).
Сравнение производится с учетом регистра символов. По умолчанию поиск вхождения производится в любой части строки
user agent; вы можете использовать якоря `^` и `$` для привязки шаблона к началу или концу строки (см. примеры ниже).

Синтаксис PCRE очень богатый и мощный и находится за рамками данной документации. Отдельные выражения могут быть
объединены с помощью различных синтаксических конструкций, что позволяет создавать сколь угодно сложные шаблоны,
однако обратите внимание, что в текущей реализации длина регулярного выражения не может превышать 1023 символа.

Несколько примеров:

```
Firefox|Nexus|Miui
```

Это выражение совпадет с любым user agent, который содержит слова "Firefox", "Nexus" или "Miui". Его можно
использовать для фильтрации посетителей с Mozilla Firefox, Google Nexus и встроенного браузера Xiaomi.

```
^Mozilla/4[.]0
```

Это выражение совпадет с любым user agent, который начинается с "Mozilla/4.0". Оно отфильтрует всех подозрительных
посетителей, которые якобы используют очень старые браузеры, но тем не менее поддерживают современные конструкции
JavaScript (подразумевается тем, что посетитель смог сформировать машинный отпечаток.)

```
^Mozilla/5[.]0$
```

Это выражение отфильтрует тех посетителей, чей user agent строго совпадает с "Mozilla/5.0", то есть не содержит
сведений о конкретном бразуре, HTML-движке и платформе, что очень необычно и подозрительно.

Все выражения выше могут быть объединены в одно с использованием логического "или" (т.е. совпадет первое *или* второе
*или* третье) следующим образом:

```
Firefox|Nexus|Miui|^Mozilla/4[.]0|^Mozilla/5[.]0$
```

**Будьте осторожны! Неправильно сформированное регулярное выражение может привести к ошибочным срабатываниям
и фильтрации больших объемов хорошего трафика. Используйте эту настройку только если вы точно знаете что делаете.**

## Фильтр referer

Эта настройка работает по тому же принципу, что описанный выше фильтр user agent, но в отношении
[HTTP referer](https://developer.mozilla.org/ru/docs/Web/HTTP/%D0%97%D0%B0%D0%B3%D0%BE%D0%BB%D0%BE%D0%B2%D0%BA%D0%B8/Referer).
Adspect отфильтрует всех посетителей, чей referer совпадет с указанным Perl-совместимым регулярным выражением.
Сравнение производится с учетом регистра символов.

Распространенным сценарием использования является фильтрация пустых referer-ов:

```
^$
```

**Будьте осторожны! Неправильно сформированное регулярное выражение может привести к ошибочным срабатываниям
и фильтрации больших объемов хорошего трафика. Используйте эту настройку только если вы точно знаете что делаете.**

## Режим фильтрации IP/ASN

Adspect поддерживает фильтрацию трафика по спискам IP-адресов, диапазонов IP-адресов и/или
[номеров автономных систем (ASN)](https://ru.wikipedia.org/wiki/%D0%90%D0%B2%D1%82%D0%BE%D0%BD%D0%BE%D0%BC%D0%BD%D0%B0%D1%8F_%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0_(%D0%98%D0%BD%D1%82%D0%B5%D1%80%D0%BD%D0%B5%D1%82)).
Существуют два списка: белый и черный.

Режим фильтрации IP/ASN управляет тем, как черный и белый списки взаимодействуют для определения, следует ли
отфильтровать того или иного посетителя. Имеется два режима:

* Черный: посетитель будет отфильтрован только если его IP-адрес или ASN есть в черном списке и отсутствует в белом.
  Таким образом, белый список задает исключения для черного списка. Этот режим установлен по умолчанию.

* Белый: посетитель будет отфильтрован, если его IP-адрес или ASN отсутствует в белом списке или есть в черном.
  Таким образом, черный список задает исключения для белого списка.

Обратите внимание, что белые списки не работают как исключения для встроенных в систему фильтрационных баз.

## Черный список IP/ASN

Это черный список. Поддерживаются адреса IPv4 и IPv6,
[CIDR-нотация](https://ru.wikipedia.org/wiki/%D0%91%D0%B5%D1%81%D0%BA%D0%BB%D0%B0%D1%81%D1%81%D0%BE%D0%B2%D0%B0%D1%8F_%D0%B0%D0%B4%D1%80%D0%B5%D1%81%D0%B0%D1%86%D0%B8%D1%8F)
и произвольные диапазоны. Примеры:

* 192.0.2.1
* 192.0.2.0/24
* 192.0.2.0–192.0.2.255
* 2001:db8::1
* 2001:db8::/112
* 2001:db8::-2001:db8::ffff
* 721
* AS721

Отдельные элементы должны разделяться переносами строки или пробелами. Обратите внимание, что система автоматически
объединяет соседние или пересекающиеся диапазоны для оптимизации их хранения в памяти и для ускорения поиска.

## Заносить все IP-адреса в черный список в режиме "Модерация"

Если эта настройка включена, то Adspect будет автоматически заносить IP-адреса всех посетителей в потоке в черный
список, если поток работает в режиме "Модерация". Так как этот режим предназначен именно для прохождения модерации,
то будет справедливо считать всех посетителей модераторами, а следовательно запоминать и блокировать в дальнейшем.
Мы рекомендуем вам всегда включать эту опцию, но будьте внимательны и не пропустите момент, когда вашу кампанию
одобрят, --- вам нужно успеть переключить поток в режим "Фильтр" прежде, чем польется трафик, иначе в черный список
попадут IP-адреса обычных посетителей.

## Белый список IP/ASN

Это белый список. Он имеет тот же формат записи, что и черный список.
