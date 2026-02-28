# [← Python](Python)

## Симптомы

При выполнении HTTP-запроса через requests возникает ошибка:

```bash
SSL: CERTIFICATE_VERIFY_FAILED
unable to get local issuer certificate
```

Запросы к API (например, https://api.agify.io) не выполняются.

Код, который вызывает ошибку:
```bash
import requests

response = requests.get("https://api.agify.io/?name=alex")
print(response.json())
```

## Где чаще всего возникает

- школьные компьютеры
- сети с «безопасным интернетом»
- фильтрация HTTPS-трафика
- прокси / DPI / MITM

## Причина проблемы

Во многих школьных сетях используется система контроля интернет-трафика:

- HTTPS-соединение перехватывается

- серверу подсовывается другой сертификат

- этот сертификат подписан локальным корневым центром

- Windows ему доверяет

- ❌ Python — нет

### Почему?

- requests использует собственный список сертификатов (certifi)

- системные сертификаты Windows по умолчанию не учитываются

- В результате Python считает соединение небезопасным.

## Решение 1️⃣ (временное, не рекомендуется)
Отключить проверку SSL:
```py
import requests
import urllib3

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

response = requests.get(
    "https://api.agify.io/?name=alex",
    verify=False
)
print(response.json())
```

### Почему работает

- проверка сертификатов полностью отключается

### Минусы

- небезопасно

- можно перехватить данные

- ❌ не рекомендуется для учебных проектов

## Решение 2️⃣ (рекомендуемое ✅)

Установить поддержку системных сертификатов:

```bash
pip install pip-system-certs
```

После этого открыть-закрыть IDE, код при этом менять **не нужно**

### Почему это работает

- requests начинает использовать
системное хранилище сертификатов Windows

- локальные школьные сертификаты становятся доверенными

- SSL-проверка проходит корректно

## Альтернативное решение

Обновить пакет сертификатов:

```bash
pip install --upgrade certifi
```

⚠️ Может не помочь, если проблема именно в локальных сертификатах сети.

**Обсуждение**: https://github.com/ErnestMatskevich/kb-knowledge/issues/5

### Ключевые слова

`requests`, `ssl`, `certificate_verify_failed`, `школьная сеть`, `windows`, `https`, `pip-system-certs`