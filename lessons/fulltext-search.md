# Найдет все документы содержащие хотябы одно слово из поискового выражения (оператор по умолчанию)

~~~
GET recipies/_search
{
  "query": {
    "match": {
      "title": "Recipies with pasta or spaghetti"
    }
  }
}
~~~

или

~~~
GET recipies/_search
{
  "query": {
    "match": {
      "title": {
        "query": "Recipies with pasta or spaghetti",
        "operator": "or"
      }
    }
  }
}
~~~

# Найдет все документы содержащие все слова из поискового выражения

~~~
GET recipies/_search
{
  "query": {
    "match": {
      "title": {
        "query": "Recipies with pasta or spaghetti",
        "operator": "and"
      }
    }
  }
}
~~~

# Найти по точному совпадению фразы

~~~
GET recipies/_search
{
  "query": {
    "match_phrase": {
      "title": "spaghetti puttanesca"
    }
  }
}
~~~

# Поиск по нескольким полям

~~~
GET recipies/_search
{
  "query": {
    "multi_match": {
      "query": "pasta",
      "fields": ["title", "description"]
    }
  }
}
~~~
