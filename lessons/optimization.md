# Slop for match_phrase

Можно указать максимальное расстояние между словами в фразе

~~~
GET proximity/_search
{
  "query": {
    "match_phrase": {
      "title": {
        "query": "spicy sauce",
        "slop": 2
      }
    }
  }
}
~~~

# Обработка запросов с ошибками

~~~
GET products/_search
{
  "query": {
    "match": {
      "name": {
        "query": "l0bster",
        "fuzziness": "auto"
      }
    }
  }
}
~~~

fuzziness=auto - чем длиннее слово тем больше ошибок допускается (но максимум две)
можно самому указать от 0 до 2

~~~
GET products/_search
{
  "query": {
    "match": {
      "name": {
        "query": "lboster love",
        "operator": "and", 
        "fuzziness": 1
      }
    }
  }
}
~~~

Перестановка так же считается за одну ошибку (можно выключить данное поведение флагом fuzzy_transpositions)