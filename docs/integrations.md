Интеграции: Signall и ASU
========================

Signall
-------

Базовый URL: `https://signall.qodex.tech`.

Основные операции:
- авторизация (`/v1/user/login`);
- отправка актов (`/v1/acts/create_act_with_inn_and_kpp`);
- обновление актов (`/v1/gravity/update_act/...`);
- удаление актов (`/v1/acts/delete_act_by_id/...`);
- синхронизация авто/перевозчиков/регоператоров.

Классы:
- `SignallActWorker` — отправка актов в Signall;
- `SignAllActsWorkerToken` — token‑авторизация через Gravity;
- `SignAllKPPWorkerToken` — отправка проходов КПП;
- `SignAllKPPLiftsWorkerToken` — отправка инцидентов (лифтов) КПП;
- `SignallActUpdater` — обновление акта;
- `SignallActReuploder` — удаление акта и чистка логов;
- `SignallAutoGetter`, `SignallClientsGetter` — импорт авто и перевозчиков.

Особенности:
- фото и видео встраиваются в JSON (base64);
- многие операции используют `ex_sys_data_send_reports` для учёта отправок;
- возможен тестовый стенд (`set_signall_test`).

ASU
---

Базовый URL: `https://bashkortostan.asu.big3.ru`.

Основные операции:
- авторизация (`/extapi/v2/token-auth/`);
- отправка актов (`/extapi/v2/landfill-fact/`);
- получение маршрутов/рейсов (`/extapi/v2/trip/`);
- справочники платформ (`/extapi/v2/platform`).

Классы:
- `ASUActsWorker` — отправка актов с конвертацией времени;
- `ASURoutesWorker` — получение маршрутов и создание записей в БД.

Особенности:
- требует преобразования времени в нужный TZ;
- маршрутные данные записываются в `records_routes_info`.
