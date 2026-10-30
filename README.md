# FORK: Kaspersky MDR Integration

Это форк проекта [klsecservices/MDR-integrations](https://github.com/klsecservices/MDR-integrations). Основная документация находится в оригинальном репозитории.

## Основные доработки

Реализован импорт информации об активах из MDR в KUMA. Данная интеграция позволяет отслеживать "здоровье" активов на стороне сервиса MDR и оперативно получать оповещения о потерях телеметрии, настраивать дополнительные правила корреляции и дашборды.

## Интеграция

Для корректной работы интеграции необходимо выполнить следующие условия:

1. Настроена "коробочная" интеграция KUMA с KSC для импорта активов.

2. К модели данных актива добавлены следующие настраиваемые поля (https://support.kaspersky.ru/kuma/4.6/242222):
   - `mdr_first_seen`
   - `mdr_last_seen`
   - `mdr_last_seen_hours`
   - `mdr_telemetry_lost_severity`
   - `mdr_isolation`
   - `mdr_status_reasons`
   - `mdr_status`

Данная интеграция не создаёт новые активы в KUMA. Импортированные из KSC в KUMA штатным образом активы обогащаются дополнительной информацией, которая получена по API MDR.
Данные из MDR записываются в дополнительные настраиваемые поля актива.

Так как в текущей версии KUMA (4.6) есть ограничения при определении условий активной категоризации: доступны только '=' и 'ilike', в метод import_assets() заложена дополнительная логика по обработке чисел и времени.

Сопоставление активов KUMA и MDR осуществляется по FQDN хостов.

## Требования

...

## Быстрый старт

Ниже рассмотрен пример, когда скрипт работает на сервере с KUMA Core.

В зависимости ОС может потребоваться установка следующих пакетов:

 - `yum install python3-pip`
 - `yum install git`
 - 
или

 - `apt install python3-pip`
 - `apt install git`

На портале MDR:

1. Сгенерировать client_id и refresh_token для работы с API MDR

В веб-консоли KUMA:

1. Сгенерировать токен для работы с API KUMA 
2. Получить ID тенанта KUMA, активы которого будут обновляться.

На сервере KUMA Core:

1. Скопировать репозиторий проекта 

`cd /opt/`

`git clone https://github.com/kmssrv/MDR-integrations.git`

2. Установить зависимости

`cd /opt/MDR-integrations/`
`pip install -r requirements.txt`

3. Настроить параметры интеграции

`cd /opt/MDR-integrations/mdr_integration/conf/`

`cp sample_config.yml config.yml`

`nano config.yml`

`General settings -> client_id`

`Modules settings -> kuma -> api_url: FQDN / IP адрес ядра KUMA либо localhost`

`Modules settings -> kuma -> api_token: токен для работы с API KUMA`

`Modules settings -> kuma -> token_id: ID тенанта KUMA` 

`Modules settings -> mdr_sync -> modules -> asset -> enable: true`

4. Скопировать `refresh_token`, полученный на портале MDR, в файл `/opt/MDR-integrations/mdr_integration/conf/.refresh_token`

`nano .refresh_token`