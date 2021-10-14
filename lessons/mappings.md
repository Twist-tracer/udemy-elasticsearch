# В ElasticSearch типы полей обычно задаются автоматом, но можно их указать и самостоятельно при создании индекса

~~~
PUT reviews
{
  "mappings": {
    "properties": {
      "rating": {"type": "float"},
      "content": {"type": "text"},
      "product_id": {"type": "integer"},
      "author": {
        "properties": {
          "first_name": { "type": "text" },
          "last_name": { "type": "text" },
          "email": { "type": "keyword" }
        }
      }
    }
  }
}
~~~

# Просмотр mappings

~~~
GET reviews/_mapping
~~~

# Конкретный mapping

~~~
GET reviews/_mapping/field/author.email
~~~

# Добавление нового мапинга в существующий индекс

~~~
PUT reviews/_mapping
{
  "properties": {
    "created_at": {"type": "date" }
  }
}
~~~

# Реиндекс

Удалить или обновить тип мапинга невозможно, но можно создать новый индекс с нужными мапингами и перетащить в него
данные ис старого

~~~
PUT reviews
{
  "mappings": {
      "properties": {
        "created_at": {"type": "text" }
      }
  }
}

POST _reindex
{
  "source": {
    "index": "reviews",
    "_source": ["rating", "product_id"],
    "query": {
      "match_all": {}
    }
  },
  "dest": {
    "index": "reviews_new"
  }
}
~~~

