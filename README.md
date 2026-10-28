# FORK: Kaspersky MDR Integration

Это форк проекта [klsecservices/MDR-integrations](https://github.com/klsecservices/MDR-integrations). Основная документация находится в оригинальном репозитории.

## Основные доработки

Реализован импорт информации об активах из MDR в KUMA. Данная интеграция позволяет отслеживать "здоровье" активов на стороне сервиса MDR и оперативно получать оповещения о потерях телеметрии, настраивать дополнительные правила корреляции и дашборды.

## Интеграция

Для корректной работы интеграции необходимо выполнить следующие условия:

1. Настренна "коробочная" интеграциия KUMA с KSC для импорта активов.

2. К модели данных актива добавлены следующие настраевыемые поля:
   - `mdr_first_seen`
   - `mdr_last_seen`
   - `mdr_last_seen_hours`
   - `mdr_telemetry_lost_severity`
   - `mdr_isolation`
   - `mdr_status_reasons`
   - `mdr_status`

https://support.kaspersky.ru/kuma/4.6/242222