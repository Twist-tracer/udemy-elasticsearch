# Простая сортировка

~~~
GET /recipies/_search
{
  "_source": ["created"], 
  "size": 2,
  "from": 2,
  "query": {
    "match": {
      "title": "pasta"
    }
  },
  "sort": [
   {
    "created": "desc" 
   }
  ]
}
~~~

# Сортировка по полям с множеством значений

~~~
GET /recipies/_search
{
  "_source": ["ratings"], 
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "ratings": {
        "order": "desc",
        "mode": "avg"
      }
    }
  ]
}
~~~
