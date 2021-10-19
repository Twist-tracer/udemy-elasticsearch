# Навигация

~~~
GET /recipies/_search
{
  "size": 2,
  "from": 2,
  "query": {
    "match": {
      "title": "pasta"
    }
  }
}
~~~
