# Вернуть только определенные поля из source

~~~
GET /recipies/_search
{
  "_source": {
    "excludes": ["description", "ingredients"]
  }, 
  "query": {
    "match": {
      "title": "pasta"
    }
  }
}
~~~

Можно указать _source=false, чтобы он вообще не возвращался
