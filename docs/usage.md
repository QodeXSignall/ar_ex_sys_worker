Примеры и сценарии использования
===============================

Важно: примеры требуют доступ к БД и корректно заполненные `core_settings`.
Тесты в репозитории используют переменные окружения для подключения.

Отправка актов в Signall (login/password)
-----------------------------------------

```python
from ar_external_sys_worker import main
import wsqluse.wsqluse

sql_shell = wsqluse.wsqluse.Wsqluse(
    dbname="db",
    user="user",
    password="pass",
    host="localhost"
)

worker = main.SignallActWorker(
    sql_shell=sql_shell,
    time_start="2024.01.01",
    trash_cats_list=["ТКО"],
    login="user@example.com",
    password="secret"
)

worker.send_unsend_acts()
```

Отправка актов в Signall (token)
-------------------------------

```python
from ar_external_sys_worker import main

worker = main.SignAllActsWorkerToken(
    sql_shell=sql_shell,
    time_start="2024.01.01",
    platform_id=1,
    gravity_ip="172.16.0.10",
    gravity_port=8080,
    test=False
)
worker.auth()
worker.send_unsend_acts()
```

Отправка КПП проходов
--------------------

```python
worker = main.SignAllKPPWorkerToken(
    sql_shell=sql_shell,
    platform_id=1,
    test=True
)
worker.auth()
worker.send_unsend_acts()
```

ASU: отправка актов и маршруты
-----------------------------

```python
worker = main.ASUActsWorker(
    sql_shell=sql_shell,
    time_start="2024.01.01",
    login="api",
    password="qwer1234",
    trash_cats_list=["ТКО"]
)
worker.send_unsend_acts()
```
