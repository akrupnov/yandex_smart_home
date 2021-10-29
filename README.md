[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg)](https://github.com/custom-components/hacs)
[![codecov](https://codecov.io/gh/dmitry-k/yandex_smart_home/branch/master/graph/badge.svg?token=5ET7CQ3JTB)](https://codecov.io/gh/dmitry-k/yandex_smart_home)

# Компонент Yandex Smart Home для Home Assistant
Компонент позволяет добавить устройства из Home Assistant в платформу [умного дома Яндекса](https://yandex.ru/dev/dialogs/smart-home) (УДЯ)
и управлять ими с любого устройства с Алисой: умные колонки, приложение на телефоне, веб интерфейс [квазар](https://yandex.ru/quasar/iot).

- [Установка](#установка)
- [Подготовка к настройке](#подготовка-к-настройке)
  - [Названия устройств](#названия-устройств)
  - [Комнаты](#комнаты)
  - [Фильтры](#фильтры)
- [Настройка интеграции](#настройка-интеграции)
  - [Изменение типа подключения](#изменение-типа-подключения)
- [Расширенные настройки и возможности](#расширенные-настройки-и-возможности)
- [Проблемы](#проблемы)
  - [Устройство не появляется в УДЯ](#устройство-не-появляется-в-удя)
  - [Ошибка "Что-то пошло не так" при частых действиях](#ошибка-что-то-пошло-не-так-при-частых-действиях)
  - [Как отвязать навык (производителя)](#как-отвязать-навык-производителя)
- [Полезные ссылки](#полезные-ссылки)


## Установка
Для работы компонента требуется Home Assistant версии **2021.7** или новее.

**Способ 1:** [HACS](https://hacs.xyz/)
> HACS > Интеграции > Добавить > Yandex Smart Home

**Способ 2:**
Вручную скопируйте папку `custom_components/yandex_smart_home` из [latest release](https://github.com/dmitry-k/yandex_smart_home/releases/latest) в директорию `/config/custom_components`

После установки перезапустите Home Assistant.

## Подготовка к настройке
В Умном доме Яндекса существует ряд особенностей и ограничений, которые необходимо знать для максимально безпроблемной эксплуатации компонента.

### Названия устройств
В названиях устройств возможны **только** русские символы и цифры. Во избежание ручного переименования, рекомендуется сразу задать правильные названия в Home Assistant. Способы сделать это:
  1. На странице "Настройки" > "Объекты" используя поле "Название"
  2. Через атрибут `friendly_name` в [customization.yaml](https://www.home-assistant.io/docs/configuration/customizing-devices/)
  3. Через параметр `name` в [расширенной настройке устройств](docs/advanced-settings.md#параметры-устройств-entity_config)
  4. Через параметр `alias` для скриптов

### Комнаты
Для нового устройства в УДЯ комната может назначаться автоматически, для этого она должна быть указана в Home Assistant. К именам комнат предъявляются те же требования, что и к именам устройств (только русские символы и цифры). Способы добавить устройство в комнату:
  1. На странице "Настройки" > "Пространства" создайте нужные комнаты. Выберите комнату в свойствах устройства на странице "Настройки" > "Объекты" или "Настройки" > "Устройства"
  2. Через параметр `room` в [расширенной настройке устройств](docs/advanced-settings.md#параметры-устройств-entity_config)

**Важно!** Комнаты в УДЯ нужно создать **вручную** через [квазар](https://yandex.ru/quasar/iot) **перед** добавлением устройств: нажмите иконку "плюс" в нижнем правом углу > выберите "Комнату".

При ручном обновлении списка устройств важно **не выбирать** "Дом", а просто понажимать стрелку "Назад":

| <img src="docs/images/quasar_discovery_1.png" width="350"> | <img src="docs/images/quasar_discovery_2.png" width="350"> |
|:---:|:---:|
| Нажать "Далее" | **Не нажимать** "Выбрать", вместо этого нажимать стрелку назад.<br>После выхода в список устройств обновите страницу |

### Фильтры
Во время настройки интеграции вам будет предложено выбрать устройства, которые будут добавлены в УДЯ (фильтры). Сперва рекомендуется выбрать как можно меньше устройств, а остальные добавлять постепенно.

Причина - фильтры используется только при добавлении новых устройств. Если устройство уже добавлено в УДЯ, его исключение с помощью фильтров не даст никакого эффекта и его придётся удалять из УДЯ вручную. Для удаления **всех устройств** - [отвяжите навык/производителя](#как-отвязать-навык-производителя).

Для продвинутых пользователей есть возможность настраивать фильтры [через YAML](docs/advanced-settings.md#фильтрация-устройств-filter).

## Настройка интеграции
Интеграция поддерживает два типа подключения:
1. Прямое подключение: **только для продвинутых пользователей**, УДЯ обращается напрямую к вашему Home Assistant, сложная многоступенчатая настройка - [подробнее](docs/direct-connection.md)
2. Через облако (бета-тест): доступно с версии 0.3.0, настройка в несколько кликов, не требуется доступ к Home Assistant из интернета, полностью бесплатно

Для настройки интеграции:
* В Home Assistant: Настройки > Интеграции > Добавить интеграцию > Yandex Smart Home. Если интеграции нет в списке - обновите страницу.
* **Внимательно** следуйте указаниям мастера настройки.

### Изменение типа подключения
* Тип подключения можно выбрать **только** при добавлении интеграции. Для перехода с прямого подключения на облачное или наоборот:
  * [Отвяжите навык/производителя](#как-отвязать-навык-производителя) с полным удалением всех устройств.
  * Удалите интеграцию на странице "Интеграции" и добавьте заново с нужным типом подключения.
* **Не удаляйте интеграцию** с облачным подключением без надобности. При её удалении происходит отвязка от УДЯ и при повторной настройке интеграции потребуется снова выполнять привязку к Яндексу через квазар (уже с новыми реквизитами).
* При изменении типа подключения конфигурацию в YAML менять или удалять не требуется. Настройка `notifier` в облачном подключении не используется, можно удалить её из YAML.

## Расширенные настройки и возможности
Компонент поддерживает расширенную настройку устройств и фильтров через [configuration.yaml](https://www.home-assistant.io/docs/configuration/).

Для применения изменений из YAML перезагрузите интеграцию на странице "Настройки" -> "Интеграции" или "Настройки" -> "Сервер" (не забудьте включить "Расширенный режим" в профиле пользователя).

* [Тонкая настройка через YAML](docs/advanced-settings.md)
* [Режимы и пользовательские умения](docs/capabilities.md)
* [Датчики](docs/sensors.md)
* [Пример YAML конфигурации](docs/config-example.yaml)

## Проблемы

### Устройство не появляется в УДЯ
* Убедитесь, что устройство не исключено в фильтрах (в настройках интеграции через GUI или YAML).
* Перезапустите Home Assistant.
* Выполните ручное "Обновление списка устройств" в УДЯ через [квазар](https://yandex.ru/quasar/iot): нажмите иконку "плюс" в правом нижнем углу > "Устройство умного дома" > Найти/выбрать ваш диалог > "Обновить список устройств"
* Если это не помогло cоздайте [issue](https://github.com/dmitry-k/yandex_smart_home/issues) или напишите в [чат](https://t.me/yandex_smart_home).
  К сообщению приложите:
  * **ID** и **атрибуты** проблемных устройств. Их можно найти в "Панель разработчика" (Developer Tools) > "Состояния" (States).
  * YAML конфигурацию `yandex_smart_home` (если имеется, лучше целиком, или только `filter` и `entity_config` для проблемного устройства).
  * Для прямого подключения:
    * Крайне желательно (но можно не сразу) [приложить лог](docs/direct-connection.md#получение-лога-обновления-списка-устройств-из-удя) обновления списка устройств
    * Если в окне отладки пусто, а УДЯ выдает ошибку "Не получилось обновить список устройств" - нужен [лог запросов и ответов](docs/direct-connection.md#получение-лога-обновления-списка-устройств-из-home-assistant) со стороны Home Assistant
  * Для облачного подключения:
    * Первые 8-10 символов вашего ID (можно посмотреть в настройках интеграции)
    * Дату и время обновления списка устройств

### Ошибка "Что-то пошло не так" при частых действиях
Если попытаться "быстро" управлять устройством, например изменять температуру многократными нажатиями "+", выскочит ошибка:
"Что-то пошло не так. Попробуйте позднее ещё раз".

Это **нормально**. УДЯ ограничивает количество запросов, которые могут придти от пользователя в единицу времени. Нажимайте кнопки медленнее :)

### Как отвязать навык (производителя)
В некоторых случаях может потребоваться полностью отвязать диалог/навык/производителя от УДЯ и удалить все устройства. Это может быть полезно если в УДЯ выгрузили много лишнего из Home Assistant, и удалять руками каждое устройство не хочется.

Для отвязки через [квазар](https://yandex.ru/quasar/iot):
* Нажмите иконку "плюс" в правом нижнем углу > "Устройство умного дома" > Найти/выбрать ваш диалог
* Нажмите корзинку в правом верхнем углу
* Поставьте галочку "Удалить устройства" и нажмите "Отвязать от Яндекса"

## Полезные ссылки
* https://t.me/yandex_smart_home - Чат по компоненту в Телеграме
* https://github.com/AlexxIT/YandexStation - Управление колонками с Алисой из Home Assistant и проброс устройств из УДЯ в Home Assistant
* https://github.com/allmazz/yandex_smart_home_ip - Список IP адресов платформы умного дома Яндекса
* https://stats.uptimerobot.com/QX83nsXBWW - Мониторинг доступности облачного подключения
