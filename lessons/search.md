# Получить все

~~~
GET products/_search
{
  "query": {
    "match_all": {}
  }
}
~~~

# Query string

~~~
GET products/_search
{
  "query": {
    "query_string": {
      "query": "tags:Meat AND name:Tuna "
    }
  }
}
~~~

# Explane

~~~
GET products/_search?explain=true
{
  "query": {
    "query_string": {
      "query": "tags:Meat AND name:Tuna "
    }
  }
}
~~~

# Совпадение

~~~
GET products/_search
{
  "query": {
    "match": {
      "name": "Lobster"
    }
  }
}
~~~

# Точное совпадение (не пропускать запрос через анализатор)

~~~
GET products/_search
{
  "query": {
    "term": {
      "name": "Lobster"
    }
  }
}
~~~

# Поиск по id

~~~
GET products/_search
{
  "query": {
    "ids": {
      "values": [1,2,3]
    }
  }
}
~~~

# Поиск по диапазону

~~~
GET products/_search
{
  "query": {
    "range": {
      "in_stock": {
        "gte": 1,
        "lte": 5
      }
    }
  }
}
~~~

# Поиск непустых значений

~~~
GET products/_search
{
  "query": {
    "exists": {
      "field": "tags"
    }
  }
}
~~~

# Поиск по префиксу

~~~
GET products/_search
{
  "query": {
    "prefix": {
      "tags.keyword": "Vege"
    }
  }
}
~~~

# Поиск по маске (как LIKE)

~~~
GET products/_search
{
  "query": {
    "wildcard": {
      "tags.keyword": "Veg*ble"
    }
  }
}
~~~

~~~
GET products/_search
{
  "query": {
    "wildcard": {
      "tags.keyword": "Veget?ble"
    }
  }
}
~~~

# Регулярочки

~~~
GET products/_search
{
  "query": {
    "regexp": {
      "tags.keyword": "Veget[a-z]+le"
    }
  }
}
~~~
