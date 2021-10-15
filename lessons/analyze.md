# Информация от том как tokenizer разбивает значения

~~~
POST _analyze
{
    "text": "Карты, деньги, 2 ствола...",
    "char_filter": [],
    "tokenizer": "standard",
    "filter": ["lowercase"]
}
~~~


# Добавление кастомного анализатора в индекс

~~~
PUT custom_analyzer
{
  "settings": {
    "analysis": {
      "analyzer": {
        "description": {
          "type": "custom",
          "char_filter": ["html_strip"],
          "tokenizer": "standard",
          "filter": [
            "lowercase",
            "stop",
            "asciifolding"
          ]
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "description": {
        "type": "text",
        "analyzer": "description"
      }
    }
  }
}
~~~

# Тест кастомного анализатора в индексе

~~~
POST custom_analyzer/_analyze
{
  "analyzer": "description", 
  "text": "Карты, <b>деньги</b>, 2 ствола... &copy;"
}
~~~


# Добавление анализатора в существующий индекс

Если не закрыть индекс то выпадет ошибка: Can't update non dynamic settings

~~~
POST stemming_test/_close

PUT stemming_test/_settings
{
  "analysis": {
    "analyzer": {
      "description_advanced": {
        "type": "custom",
        "char_filter": ["html_strip"],
        "tokenizer": "standard",
        "filter": [
          "lowercase",
          "stop",
          "asciifolding"
        ]
      }
    }
  }
}

POST stemming_test/_open
~~~
