Контракты БД и настройки
=======================

Модуль опирается на существующую БД AR и ожидает наличие ряда таблиц и
настроек. Ниже перечислены ключевые структуры, которые используются в коде.

Основные таблицы
---------------

- `records` — акты с весовыми данными.
- `photos`, `record_photos`, `photo_types` — фото актов.
- `kpp_arrivals` — проходы КПП.
- `kpp_lifts` — инциденты КПП.
- `videos` — видео для инцидентов КПП.
- `records_routes_info` — маршрутные данные (ASU).
- `ex_sys_data_send_reports` — лог отправок.
- `ex_sys_tables` — справочник таблиц для логирования.
- `external_systems` — справочник внешних систем.

Настройки в `core_settings`
---------------------------

Используются для авторизации и конфигурации:
- `signall_login` / `signall_pass` — учетка Signall.
- `asu_api_login` / `asu_api_pass` — учетка ASU.
- `asu_poligon_name` — идентификатор полигона для ASU.

Примеры SQL (упрощённо)
----------------------

Получение неотправленных актов:
```
SELECT ... FROM records r
LEFT JOIN ex_sys_data_send_reports esdsr ON esdsr.local_id = r.id
WHERE not time_out is null
AND r.id NOT IN (
  SELECT local_id FROM ex_sys_data_send_reports
  WHERE table_id=<records_table_id> AND ex_sys_id=<ex_sys_id>
  AND not get is null
)
```

Логирование отправки:
```
INSERT INTO ex_sys_data_send_reports (ex_sys_id, local_id, sent, table_id)
VALUES (<ex_sys_id>, <local_id>, <timestamp>, <records_table_id>);
```
