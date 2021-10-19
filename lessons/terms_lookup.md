# Terms lookup mechanism

~~~
PUT /users/_doc/1
{
  "name": "John Roberts",
  "following" : [2, 3]
}
~~~

~~~
PUT /stories/_doc/1
{
  "user": 1,
  "content": "Wow look, a penguin!"
}
~~~

~~~
GET /stories/_search
{
  "query": {
    "terms": {
      "user": {
        "index": "users",
        "id": "1",
        "path": "following"
      }
    }
  }
}
~~~
