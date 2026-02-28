# Не удаётся получить доступ к части API из методических материалов

## Вопрос
Во время занятия некоторые API из методических материалов не открываются (таймауты, 403/429, блокировки в школьной сети), из-за чего демонстрация работы с API срывается.

## Причина
- Часть сервисов может быть ограничена по региону или провайдеру.
- Бесплатные API часто имеют нестабильную доступность или лимиты.
- Школьные/корпоративные сети иногда блокируют отдельные домены.

## Решение
Для демонстрации базовых GET-запросов используйте публичные API без ключей и VPN.

### Рекомендуемые публичные API (GET, без ключей)
1. JSONPlaceholder — `https://jsonplaceholder.typicode.com/posts/1`
2. Cat Fact — `https://catfact.ninja/fact`
3. Official Joke API — `https://official-joke-api.appspot.com/random_joke`
4. Quotable — `https://api.quotable.io/random`
5. Agify — `https://api.agify.io/?name=alex`
6. Genderize — `https://api.genderize.io/?name=alex`
7. Nationalize — `https://api.nationalize.io/?name=alex`
8. Dog CEO — `https://dog.ceo/api/breeds/image/random`
9. Universities API — `http://universities.hipolabs.com/search?country=Kazakhstan`
10. CoinDesk (BTC price) — `https://api.coindesk.com/v1/bpi/currentprice.json`

## Примеры кода (Python + requests)

### 1) Базовый GET + разбор JSON
```python
import requests

url = "https://jsonplaceholder.typicode.com/posts/1"
r = requests.get(url, timeout=10)
print("Status:", r.status_code)
print(r.json())
```

### 2) Параметры запроса (params)
```python
import requests

url = "https://api.agify.io/"
params = {"name": "alex"}
r = requests.get(url, params=params, timeout=10)
print(r.url)          # итоговый URL с query params
print(r.json())
```

### 3) Безопасная обработка ошибок
```python
import requests

try:
    r = requests.get("https://catfact.ninja/fact", timeout=10)
    r.raise_for_status()
    data = r.json()
    print("Факт:", data.get("fact"))
except requests.exceptions.Timeout:
    print("Превышено время ожидания")
except requests.exceptions.HTTPError as e:
    print("HTTP ошибка:", e)
except requests.exceptions.RequestException as e:
    print("Ошибка сети:", e)
```

### 4) Единый шаблон для урока (можно менять URL)
```python
import requests

API_URLS = [
    "https://official-joke-api.appspot.com/random_joke",
    "https://dog.ceo/api/breeds/image/random",
    "https://api.quotable.io/random",
]

for url in API_URLS:
    try:
        r = requests.get(url, timeout=10)
        r.raise_for_status()
        print(f"\n=== {url} ===")
        print(r.json())
    except requests.RequestException as e:
        print(f"\n=== {url} ===")
        print("Ошибка:", e)
```

## Примечания
- Перед уроком откройте 2–3 API в браузере и убедитесь, что они отвечают.
- Держите минимум 3 резервных API, чтобы быстро переключиться при блокировке.
- Для школьных занятий лучше использовать короткие JSON-ответы (легче объяснять структуру данных).

## Обсуждение
- GitHub Issue: [Не удаётся получить доступ к части API из методических материалов](https://github.com/ErnestMatskevich/kb-knowledge/issues?q=is%3Aissue+%22%D0%9D%D0%B5+%D1%83%D0%B4%D0%B0%D1%91%D1%82%D1%81%D1%8F+%D0%BF%D0%BE%D0%BB%D1%83%D1%87%D0%B8%D1%82%D1%8C+%D0%B4%D0%BE%D1%81%D1%82%D1%83%D0%BF+%D0%BA+%D1%87%D0%B0%D1%81%D1%82%D0%B8+API+%D0%B8%D0%B7+%D0%BC%D0%B5%D1%82%D0%BE%D0%B4%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D1%85+%D0%BC%D0%B0%D1%82%D0%B5%D1%80%D0%B8%D0%B0%D0%BB%D0%BE%D0%B2%22)
