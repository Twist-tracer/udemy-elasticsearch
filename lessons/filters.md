# Фильтрация

~~~
GET /recipies/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "title": "pasta"
          }
        }
      ],
      "filter": [
        {
          "range": {
            "preparation_time_minutes": {
              "lte": 15
            }
          }
        }
      ]
    }
  }
}
~~~

Фильтрация в отличие от поиска не влияет на score и работает быстрее. ЕЕ лучше использовать для точных запросов, а поиск
для полнотекстовых
