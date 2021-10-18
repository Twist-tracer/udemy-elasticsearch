# Найти все документы удовлетворяющие условиям

~~~
GET recipies/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "ingredients.name": "parmesan"
          }  
        }
      ],
      "must_not": [
        {
          "match": {
            "ingredients.name": "tuna"
          }
        }
      ],
      "should": [
        {
          "match": {
            "ingredients.name": "parsley"
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

- must - поля которые должны быть в документе. Влияет на оценку (score),
то есть чем больше вхождений из must тем релевантней документ.
Эквивалент operator=and в полнотекстовом запросе
- filter - тоже что и must, но не влияем на оценку (score)
- must_not - антоним must
- should - если есть вхождения из то оценка будет выше,
в отличие от must если вхождений не будет совсем то документ все ровно найдется
Эквивалент operator=or в полнотекстовом запросе
