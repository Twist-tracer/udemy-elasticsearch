# Добавить документ со случайным id

~~~
POST products/_doc
{
    "name": "Coffee Maker",
    "price": 64,
    "in_stock": 10
}
~~~

# Добавить документ со своим id

~~~
PUT products/_doc/1
{
    "name": "Coffee Maker",
    "price": 64,
    "in_stock": 10
}
~~~

# Получить документ по id

~~~
GET products/_doc/2
~~~

# Обновить документ (PATCH)

~~~
POST products/_update/1?if_primary_term=1&if_seq_no=1
{
    "doc": {
        "in_stock": 10
    }
}
~~~

if_primary_term и if_seq_no гарантируют что обновляем только тот документ, который запросили

# Скриптинг

## Обычное обновление

~~~
POST products/_update/2
{
    "script": {
        "source": "ctx._source.in_stock += params.in_stock",
        "params": {
            "in_stock": 10
        }
    },
    "upsert": {
        "name": "Coffee Maker 5",
        "price": 64,
        "in_stock": 10
    }
}
~~~

upsert - если документ не создан то создаст новый с этими параметрами

## Обновление по условию

~~~
POST products/_update_by_query
{
    "conflicts": "proceed",
    "script": {
        "source": "ctx._source.in_stock--"
    },
    "query": {
        "match_all": {}
    }
}
~~~

## Удаление по условию

~~~
POST products/_delete_by_query
{
  "query": {
    "match_all": {}
  }
}
~~~
