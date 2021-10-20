# Простые агрегации

~~~
GET orders/_search
{
  "size": 0,
  "aggs": {
    "total_sales": {
      "sum": {
        "field": "total_amount"
      }
    },
    "avg_sale": {
      "avg": {
        "field": "total_amount"
      }
    },
    "min_sale": {
      "min": {
        "field": "total_amount"
      }
    },
    "max_sale": {
      "max": {
        "field": "total_amount"
      }
    }
  }
}
~~~

# Подсчет уникальных значений

~~~
GET orders/_search
{
  "size": 0,
  "aggs": {
    "total_salesmen": {
      "cardinality": {
        "field": "salesman.id"
      }
    }
  }
}
~~~


# Подсчет количества

~~~
GET orders/_search
{
  "size": 0,
  "aggs": {
    "values_count": {
      "value_count": {
        "field": "salesman.id"
      }
    }
  }
}
~~~

Используется в основном с другими агрегациями

# Stats (краткая форма для подсчета count, min, max, avg, sum)

~~~
GET orders/_search
{
  "size": 0,
  "aggs": {
    "amount_stats": {
      "stats": {
        "field": "total_amount"
      }
    }
  }
}
~~~

# Bucket aggregations

Для по счета документов по enum, например по статусу

~~~
GET orders/_search
{
  "size": 0, 
  "aggs": {
    "status_terms": {
      "terms": {
        "size": 10, 
        "field": "status.keyword",
        "missing": "N/A",
        "min_doc_count": 10,
        "order": {
          "_key": "asc"
        }
      }
    }
  }
}
~~~

! Если terms size будет меньше максимального кол-ва enums, то и подсчеты будут приблизительные

# Nested bucket aggregations

~~~
GET orders/_search
{
  "size": 0, 
  "aggs": {
    "status_terms": {
      "terms": {
        "field": "status.keyword"
      },
      "aggs": {
        "status_stats": {
          "stats": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
~~~

# Aggregation after filter

~~~
GET orders/_search
{
  "size": 0, 
  "aggs": {
    "low_value": {
      "filter": {
        "range": {
          "total_amount": {
            "lt": 50
          }
        }
      },
      "aggs": {
        "avg_amount": {
          "avg": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}

по сути тоже что и 

GET orders/_search
{
  "size": 0, 
  "query": {
    "bool": {
      "filter": {
        "range": {
          "total_amount": {
            "lt": 50
          }
        }
      }
    }
  },
  "aggs": {
    "avg_amount": {
      "avg": {
        "field": "total_amount"
      }
    }
  }
}
~~~

# Bucket rules. Ручное объявление категорий

~~~
GET recipies/_search
{
  "size": 0,
  "aggs": {
    "my_filter": {
      "filters": {
        "filters": {
          "pasta": {
            "match": {
              "title": "pasta"
            }
          },
          "spaghetti ": {
            "match": {
              "title": "spaghetti"
            }
          }
        }
      },
     "aggs": {
        "avg_rating": {
          "avg": {
            "field": "ratings"
          }
        }
      }
    }
  }
}
~~~

# Range aggregations

~~~
GET orders/_search
{
  "size": 0,
  "aggs": {
    "ammount_distributoin": {
      "range": {
        "field": "total_amount",
        "ranges": [
          {
            "to": 50
          },
          {
            "from": 50,
            "to": 100
          },
          {
            "from": 100
          }
        ]
      }
    }
  }
}
~~~

# Date range aggregations

~~~
GET orders/_search
{
  "size": 0,
  "aggs": {
    "purchased_rages": {
      "date_range": {
        "field": "purchased_at",
        "format": "yyyy-MM-dd",
        "keyed": true,
        "ranges": [
          {
            "from": "2016-01-01",
            "to": "2016-01-01||+6M",
            "key": "first"
          },
          {
            "from": "2016-01-01||+6M",
            "to": "2016-01-01||+1y",
            "key": "second"
          }
        ]
      },
      "aggs": {
        "bucket_stats": {
          "stats": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
~~~

# Histogram

~~~
GET orders/_search
{
  "size": 0,
  "query": {
    "range": {
      "total_amount": {
        "gte": 100
      }
    }
  }, 
  "aggs": {
    "amount_distibution": {
      "histogram": {
        "field": "total_amount",
        "interval": 25,
        "min_doc_count": 0,
        "extended_bounds": {
          "min": 0,
          "max": 600
        }
      }
    }
  }
}
~~~

# Date histogram

~~~
GET orders/_search
{
  "size": 0,
  "aggs": {
    "orders_over_time": {
      "date_histogram": {
        "field": "purchased_at",
        "interval": "month"
      
      }
    }
  }
}
~~~

# Global aggregations

(не зависят от запроса)

~~~
GET orders/_search
{
  "query": {
    "range": {
      "total_amount": {
        "gte": 100
      }
    }
  }, 
  "size": 0,
  "aggs": {
    "all_orders": {
      "global": {},
      "aggs": {
        "stats_amount": {
          "stats": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
~~~

# Missing values aggregations

~~~
GET orders/_search
{
  "size": 0,
  "aggs": {
    "orders_whitout_status": {
      "missing": {
        "field": "status.keyword"
      }
    }
  }
}
~~~

# Nested docs aggregations

~~~
GET department/_search
{
  "size": 0,
  "aggs": {
    "employees": {
      "nested": {
        "path": "employees"
      },
      "aggs": {
        "min_age": {
          "min": {
            "field": "employees.age"
          }
        }
      }
    }
  }
}
~~~
