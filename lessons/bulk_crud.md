# Массовое создание документов

~~~
POST _bulk
{"index": {"_index": "products","_id": 200}}
{"name": "Coffee Maker","price": 64,"in_stock": 10}
{"create": {"_index": "products","_id": 201}}
{"name": "Milk Forther", "price": 149, "in_stock": 14}
~~~

create вызавед ошибку если док с таким id уже есть
index тоже самое но ошибку не вызовет

# Массовое обновление документов

~~~
POST products/_bulk
{"update": {"_id": 201}}
{"doc": {"name": "Milk Forther 3"}}
~~~

~~~
POST _bulk
{"update": {"_index": "products","_id": 201}}
{"doc": {"name": "Milk Forther 2", "price": 149, "in_stock": 14}}
~~~


# Массовое удаление документов

~~~
POST _bulk
{"update": {"_index": "products","_id": 200}}
~~~