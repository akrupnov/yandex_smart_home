# Устранение проблем и ошибок

!!! bug "При возникновении любых проблем и ошибок сперва загляните в журнал Home Assistant: `Настройки` --> `Система` --> [`Журнал сервера`](https://my.home-assistant.io/redirect/logs/)"

## Устройство не появляется в УДЯ { id=missing-device }
* Убедитесь, что:
     1. Объект, для которого будет создано устройство, выбран в [cписке объектов для передачи в УДЯ](../config/filter.md)
     2. Объект есть в [списке поддерживаемых устройств](../supported-devices.md) (особенно актуально для датчиков без `device_class`)
     3. В [`Журнале сервера`](https://my.home-assistant.io/redirect/logs/) нет ошибок

* Перезапустите Home Assistant
* Выполните ручное [Обновление списка устройств](../quasar.md#discovery)

Если это не помогло - cоздайте [issue](https://github.com/dext0r/yandex_smart_home/issues) или напишите в [чат](https://t.me/yandex_smart_home). 
К сообщению приложите:

  * **ID** и **атрибуты** проблемных объектов: их можно найти в `Панель разработчика` > `Состояния`
    ![](../assets/images/entity-state.png){ width=700 }
  
  * YAML конфигурацию `yandex_smart_home` (если имеется, лучше целиком, или только `filter` и `entity_config` для проблемного объекта)
  * Для облачного подключения:
    * Первые 6 символов вашего ID (можно посмотреть в [настройках интеграции](../config/getting-started.md#gui))
    * Дату и время обновления списка устройств

## Устройство в УДЯ поддерживает не все функции { id=blinking-features }
Некоторые устройства изменяют список поддерживаемых фукнций в зависимости от своего состояния. Особенно часто это поведение замечено за `media_player`.

Если в УДЯ у устройства отсутствует функция, которая точно должна быть - попробуйте [Обновить список устройств](../quasar.md#discovery) при включенном устройстве.

Так же может помочь настройка [`features`](../config/entity.md#features) для явного указания поддерживаемых устройством функций.

## Устройство отвязалось { id=unlinked }
Необходимо вручную [Обновить список устройств](../quasar.md#discovery), но сделать это не из списка навыков, а **зайти в навык** и нажать там кнопку `Обновить список устройств` или `Привязать к Яндексу`.

## State requested for unexposed entity { id=unexposed-entity } 
В УДЯ присутствует устройство, но объект по которому оно создано не выбран в [объектах для передачи в УДЯ](../config/filter.md). 

Для исправления: удалите устройство из УДЯ или добавьте [объект](../faq.md#get-entity-id-quasar) в список устройств для передачи.

## Unsupported mode "XXX" { id=unsupported-mode }
Не удалось связать режимы между УДЯ и Home Assistant. Если нет необходимости выбирать проблемные режимы - ошибку можно игноривать. 

Для исправления: настройте [связь режимов](../config/modes.md) УДЯ <--> HA

## В УДЯ нет графиков / устройства не выбираются в условиях { id=no-notifier }
Только для прямого подключения: [настройте отправку уведомлений](../advanced/direct-connection.md#notifier) об изменении состояний устройств

## Notification request failed: UNKNOWN_USER { id=unknown-user }
* Облачное подключение: [Обновите список устройств](../quasar.md#discovery)
* Прямое подключение:
    1. [Включите отладку](debug.md#debug-logger)
    2. [Обновите список устройств](../quasar.md#discovery) в УДЯ
    3. В `home-assistant.log` найдите запрос `/user/devices` и убедитесь, что `user_id` в нём совпадает с `user_id` в конфигурации [нотификатора](../advanced/direct-connection.md#notifier)

## Notification request failed: Unauthorized { id=notifier-unathorized }
Только для прямого подключения: обновите [oauth_token](../advanced/direct-connection.md#notifier-401)