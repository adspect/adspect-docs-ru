# Интеграция с Keitaro

Adspect поддерживает нативную интеграцию с трекером Keitaro при помощи фильтра потока. Принцип работы следующий: в кампании
в Keitaro создаются отдельные потоки для контента и белой страницы. К потоку белой страницы подключается фильтр Adspect, который
связывается с потоком в личном кабинете Adspect и использует его настройки для определения тех посетителей, которые должны быть
направлены на белую страницу. Все остальные посетители, которых пропустил фильтр Adspect, попадают на поток контента.

## Установка фильтра

Чтобы использовать фильтр Adspect в трекере Keitaro, его необходимо загрузить на сервер. Для этого выполните два простых шага:

1. [Скачайте файл фильтра AdspectFilter.php](https://clients.adspect.ai/static/dist/keitaro/AdspectFilter.php);
2. Загрузите файл AdspectFilter.php в директорию `/var/www/keitaro/application/filters` на сервере, где установлен ваш
   трекер Keitaro.

:::{note}
Установку фильтра нужно выполнить лишь один раз для конкретного сервера Keitaro, либо после переустановки трекера Keitaro.
:::

## Настройка кампании

Мы рекомендуем создавать отдельный поток в личном кабинете Adspect для каждой кампании в Keitaro.

1. Создайте поток в личном кабинете Adspect. Настройки контента и белой страницы игнорируются при интеграции через фильтр
   Keitaro, поэтому заполнять их не требуется; имеют значение только настройки, относящиеся к фильтрации трафика.

2. Создайте отдельный поток для белой страницы в вашей кампании в Keitaro и разместите его выше основного потока:
   ```{image} _static/keitaro/flows.png
   :alt: Пример организации потоков Keitaro
   ```

3. В настройках потока белой страницы на вкладке "Фильтры" добавьте фильтр "adspect" и укажите ID вашего потока из личного
   кабинета Adspect в поле "Adspect Stream ID":
   ```{image} _static/keitaro/filter.png
   :alt: Пример настройки фильтра потока Keitaro
   ```

4. Направьте трафик на URL вашей кампании в Keitaro.

:::{tip}
Обычно в кампании Keitaro контент (оффер) задается в замыкающем потоке, а белая страница показывается при помощи фильтра в более
приоритетном потоке. Вы можете настроить кампанию "зеркально", чтобы поток с фильтром Adspect являлся основным и принимал только
целевых посетителей, а замыкающим потоком сделать поток с белой страницей. Для этого в потоке контента переключите режим фильтра
Adspect с "Да" на "Нет".
:::
