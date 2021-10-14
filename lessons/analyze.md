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
