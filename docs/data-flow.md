Потоки данных
=============

Отправка актов (Signall)
------------------------

```mermaid
sequenceDiagram
    participant Worker as SignallActWorker
    participant DB as SQL
    participant Log as ex_sys_data_send_reports
    participant API as SignallAPI
    Worker->>DB: SELECT records + joins
    DB-->>Worker: act data
    Worker->>Log: INSERT sent
    Worker->>API: POST /v1/acts/create_act_with_inn_and_kpp
    API-->>Worker: {act_id or error}
    Worker->>Log: UPDATE get, ex_sys_data_id
```

КПП (проходы)
------------

`SignAllKPPWorkerToken` берет данные из `kpp_arrivals`, добавляет фото, 
формирует JSON и отправляет в `/v1/gravity/passes/create`.

КПП инциденты (лифты)
--------------------

`SignAllKPPLiftsWorkerToken` получает записи из `kpp_lifts`, добавляет
фото и видео (thumb), и отправляет в `/v1/gravity/passes/incidents/create`.

ASU (акты)
----------

`ASUActsWorker` подготавливает JSON, конвертирует время, отправляет в
`/extapi/v2/landfill-fact/`, затем загружает фото прибытия/убытия.

Логирование
-----------

Таблица `ex_sys_data_send_reports` хранит:
- `sent` — время отправки,
- `get` — время получения ответа,
- `ex_sys_data_id` — ID во внешней системе,
- `data` — сериализованный payload.
