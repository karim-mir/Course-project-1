# Course project №1
Проект реализует информацию о курсе валют, акций через API, общую сумму расходов, а также предоставляет информацию о последних транзакциях по сумме платежа

## Конфигурация API

Для работы с проектом необходимо настроить доступ к API. Вам понадобятся следующие ключи и параметры из файла `.env`:
`EXCHANGE_API_KEY` - API ключ для цен на акции и данных о валютах
`STOCK_API_KEY` - API ключ данных о ценах на акции
`CURRENCY_API_KEY` - API ключ данных о валютах
`STOCK_API_URL=https://www.alphavantage.co/` - Параметры для API цен на акции
`CURRENCY_API_URL=https://api.apilayer.com/exchangerates_data` - URL API данных о валютах

## Функции модуля src.utils.py

Модуль `utils.py` содержит несколько функций для работы с данными транзакций, получения курсов валют и других полезных операций.

### Установка зависимостей
Для корректной работы модуля `utils.py` необходимо установить следующие зависимости:
```pip install pandas requests python-dotenv```

### Функции

#### `load_transactions()`
Загружает фиксированные данные транзакций в формате Pandas DataFrame.

Возвращает:
- `pd.DataFrame`: DataFrame с данными о транзакциях.

#### `get_expenses(start_date, end_date)`
Возвращает данные о расходах за указанный период.

Параметры:
- `start_date` (datetime): Начальная дата периода.
- `end_date` (datetime): Конечная дата периода.

Возвращает:
- `dict`: Словарь с общей суммой расходов и основными категориями расходов.

#### `get_income(start_date, end_date)`
Возвращает данные о доходах за указанный период.

Параметры:
- `start_date` (datetime): Начальная дата периода.
- `end_date` (datetime): Конечная дата периода.

Возвращает:
- `dict`: Словарь с общей суммой доходов и основными категориями доходов.

#### `get_currency_rates()`
Получает актуальные курсы валют в базе USD.

Возвращает:
- `list`: Список словарей с валютами и их курсами, или пустой список в случае ошибки.

#### `get_stock_prices(stock_symbol: str) -> dict`
Получает цены акций для заданных тикеров.

Параметры:
- `stock_symbol` (str): Строка с тикерами акций, разделенными запятыми.

Возвращает:
- `list`: Список словарей с датами, ценами открытия и закрытия, объёмом для последних двух дней. Возвращает `None` в случае ошибки.

#### `fetch_data(date_str, date_range="M", stock_symbol="AAPL")`
Получает данные о расходах, доходах, курсах валют и ценах акций за указанные даты и диапазоны.

Параметры:
- `date_str` (str): Дата в строковом формате.
- `date_range` (str): Диапазон данных: "W", "M", "Y", "ALL".
- `stock_symbol` (str): Тикер акции для получения цен.

Возвращает:
- `dict`: Словарь с расходами, доходами, курсами валют и ценами акций за указанные даты.

## Функции модуля src.views.py

Модуль `views.py` предоставляет функции для обработки запросов и формирования ответов в формате JSON. Эти функции интегрируют данные о транзакциях, курсах валют и ценах акций.

### Функции

#### `get_greeting() -> str`
Возвращает приветствие в зависимости от текущего времени суток.

Возвращает:
- `str`: Приветствие для пользователя.

#### `create_json_response(data)`
Формирует JSON-ответ из переданных данных.

Параметры:
- `data`: Данные, которые необходимо преобразовать в JSON.

Возвращает:
- `str`: Строка с форматом JSON.

#### `main_page(date_str: str, stock_symbol: str = None) -> str`
Формация главной страницы с данными о транзакциях, курсах валют и ценах акций.

Параметры:
- `date_str` (str): Дата в формате YYYY-MM-DD или YYYY-MM-DD HH:MM:SS.
- `stock_symbol` (str): Тикер акции (по умолчанию `None`).

Возвращает:
- `str`: JSON-ответ с данными о транзакциях, курсах валют и акциях.

#### `events_page(date_time_str: str, period: str = "M") -> str`
Формирует страницу событий, представляя данные за указанный период.

Параметры:
- `date_time_str` (str): Дата и время в формате YYYY-MM-DD HH:MM:SS.
- `period` (str): Период ("W", "M", "Y") для фильтрации данных.

Возвращает:
- `str`: JSON-ответ с информацией о расходах и доходах за указанный период.

#### `get_start_and_end_dates(input_date_time: datetime, period: str) -> (datetime, datetime)`
Возвращает начальную и конечную даты для заданного периода.

Параметры:
- `input_date_time` (datetime): Входная дата для определения диапазона.
- `period` (str): Период ("W", "M", "Y", "ALL").

Возвращает:
- `(datetime, datetime)`: Начальная и конечная даты для заданного периода.

#### `is_valid_datetime(dt_str: str) -> bool`
Проверяет, является ли строка даты и времени валидной.

Параметры:
- `dt_str` (str): Дата и время в формате YYYY-MM-DD HH:MM:SS.

Возвращает:
- `bool`: `True`, если строка соответствует формату даты, иначе `False`.

## Функции модуля src.main.py

Модуль `main.py` является точкой входа в приложение и предоставляет интерфейсы для взаимодействия с пользователем через командную строку, а также для обработки запросов к страницам.

### Функции

#### `validate_datetime(input_str: str) -> datetime`
Проверяет, является ли введенная строка корректной датой и временем.

Параметры:
- `input_str` (str): Дата и время в формате YYYY-MM-DD HH:MM:SS.

Возвращает:
- `datetime` или `None`: Объект datetime, если строка корректна, иначе `None`.

#### `handle_main_page()`
Обрабатывает действия на главной странице и выводит информацию о транзакциях, курсах валют и ценах акций.

Возвращает:
- `None`: Печатает JSON-ответы из функции `main_page`.

#### `handle_events_page()`
Обрабатывает действия на странице событий и выводит информацию о расходах и доходах за определенный период.

Возвращает:
- `None`: Печатает JSON-ответы из функции `events_page`.

#### `main()`
Основной цикл выполнения программы, который предлагает пользователю выбрать тип страницы для обработки.

Возвращает:
- `None`: Запускает обработчики страниц в зависимости от выбора пользователя.

#### Логирование
Логирование настроено для записи ошибок, связанных с запросами к API валют. Вся информация выводится в стандартный поток вывода.
Создание файла `.env` с необходимыми переменными окружения является обязательным.


## Тестирование

В этом проекте используются модульные тесты, написанные с использованием библиотеки `pytest`, `unittest` включая фикстуры и правильное использование `mock` и `patch` для изоляции тестируемого кода.

### Установка

Для запуска тестов требуется установить `pytest` и `unittest.mock`:

```pip install pytest```
```pip install unittest```

### Запуск тестов

Чтобы запустить все тесты, выполните следующую команду в терминале:

```pytest```

### Использование фикстур

Фикстуры в `pytest` позволяют использовать повторяющиеся настройки для тестов. Вот пример фикстуры:

```import pytest
from src.models import MyModel
@pytest.fixture
def sample_model():
    return MyModel(data={'key': 'value'})
   ```

### Использование Mock и Patch

Библиотека `unittest.mock` предоставляет возможность замещения объектов во время выполнения. Вот пример использования `mock` и `patch`:
```
from unittest.mock import patch, Mock
from src.services import MyService
def test_my_service():
    with patch('src.services.external_api') as mock_api:
    mock_api.return_value = Mock(status_code=200, json=lambda: {"key": "value"})
    service = MyService()
    result = service.get_data()&#13
    assert result['key'] == 'value'
```

### Структура тестов

Тесты организованы в директории `tests`. Каждый файл начинается с префикса `test_`, чтобы `pytest` мог их обнаружить.

### Покрытие тестами

Для проверки покрытия тестами используйте `pytest-cov`. Установите библиотеку:

```pip install pytest-cov```

И выполните тесты с дополнительной опцией:
```pytest --cov=src```
