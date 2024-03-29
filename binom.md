# Интеграция с Binom

Трекер Binom версий 1.16&ndash;1.17.122 включает в себя нативную интеграцию с Adspect (интеграция с версиями 2.x появится
в ближайшее время). Дистрибутив трекера содержит наше фильтрующее программное обеспечение Adspect Sieve™, которое устанавливается
локально на тот же сервер, что и сам трекер. Это ПО отличается от облачной версии сервиса Adspect в нескольких важных аспектах:

1. Фильтрация трафика происходит полностью локально на вашем сервере без обращений в облако Adspect
   в реальном времени. Это существенно ускоряет процесс обработки каждого клика. ПО Adspect Sieve™
   написано на языке C++ и обладает колоссальной вычислительной мощностью, которая ограничена лишь
   аппаратными ресурсами вашего сервера и пропускной способностью его сетевого канала.

2. Ни данные о переходах, ни какие-либо иные сведения не отправляются в Adspect. Ваша статистика
   не покидает вашего трекера и таким образом остается полностью конфиденциальной. Adspect Sieve™
   лишь синхронизирует фильтры и информацию о подписке время от времени в фоновом режиме. Даже если
   связь с Adspect теряется или срок подписки заканчивается, то Sieve продолжит работу автономно в
   течение трех дней.

3. Потоки облачного сервиса Adspect не используются; их заменяют кампании Binom.  Вам не нужно ничего
   делать в личном кабинете Adspect, кроме приобретения подписки и получения ID аккаунта и ключа API.

Adspect Sieve в сущности является локальной версией нашего облачного фильтрующего движка и включает
в себя [все технологии, составляющие ядро системы Adspect](how-it-works.md): черные списки, фильтрацию
по JavaScript-отпечаткам и [машинное обучение VLA™](how-it-works.md#vla). Вся прочая функциональность облачного
сервиса (таргетинг, собственные списки IP-адресов, URL-правила и т.п.) реализуется средствами Binom.

## Установка

Дистрибутив Adspect Sieve входит в состав Binom. Установка проходит в три простых шага:

1. [Подключитесь к вашему серверу через SSH](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-to-connect-to-a-remote-server-ru)

2. Выполните следующую команду с привилегиями root (используйте `sudo`):
   ```bash
   bash /root/binom_install.sh adspect
   ```

3. В ответ на вопрос об установке (см. скриншот) введите `1` и нажмите Enter.
   ```{image} _static/binom/installation.png
   :alt: Успешная установка заканчивается сообщением "Done!"
   ```

## Настройка

Для того, чтобы использовать Adspect Sieve, вам понадобятся ID учетной записи в Adspect и ключ API.
Оба реквизита можно найти в [вашем профиле](https://clients.adspect.ai/profile) в личном кабинете:

```{image} _static/binom/credentials1.png
:alt: Реквизиты доступа для Adspect Sieve в профиле
```

В Binom, перейдите в настройки кампании и раскройте меню **Protection**. Поставьте галочку у пункта
**Adspect** и нажмите **Add account**. В появившемся окне укажите ID вашего аккаунта и ключ API:

```{image} _static/binom/credentials2.png
:alt: Реквизиты доступа для Adspect Sieve в Binom
```

Чтобы изменить этот аккаунт, перейдите в настройки трекера: **Settings > API**.

## Включение фильтрации

Фильтрация Adspect Sieve включается на уровне отдельных кампаний при помощи [правил](https://docs.binom.org/campaign-rules.php).
Вы можете создавать как правила **Bot**, так и **NOT Bot** для распределения трафика по нужной вам логике:

```{image} _static/binom/bot-rule.png
:alt: Пример Binom-правила Bot
```

Как и облачная версия платформы, Adspect Sieve поддерживает технологию JavaScript-фингерпринтинга для
глубокого сканирования посетителей по их браузерным отпечаткам. Ее можно включить в настройках кампании
в меню **Protection**, установив галочку **Use fingerprint**.

[Click API](https://docs.binom.org/click-api.php) в Binom полностью поддерживает Adspect Sieve,
в том числе в режиме включенного JS-фингерпринтинга.

### JS Protection

Включение фильтрации Adspect Sieve в кампании изменяет ее код [JS Protection](https://docs.binom.org/js-protection.php),
поэтому после включения или выключения не забудьте заменить скрипт JS Protection на новый.
