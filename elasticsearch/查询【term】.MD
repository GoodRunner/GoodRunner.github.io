### term 查询

[term](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-term-query.html)

可以使用term查询根据精确值（例如价格，产品ID或用户名）查找文档。

**避免使用字段term查询text，要搜索text字段值，请改用match查询。**



```text
GET /blogs/sina/_search
{
  "query":{
    "term":{
      "user_id":{
        "value":276377
      }
    }
  }
}
```

[terms](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-terms-query.html)

```text
GET /blogs/sina/_search
{
  "query":{
    "terms":{
      "user_id":[276381,276380]
    }
  }
}
```

[terms_set](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-terms-set-query.html)

只要匹配某一个term就是要查询的文档

```text
GET /my-index/_search
{
    "query": {
        "terms_set": {
            "codes" : {
                "terms" : ["abc", "def", "ghi"],
                "minimum_should_match_field": "required_matches"
            }
        }
    }
}
```

[范围查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html)

```text
GET /blogs/sina/_search
{
    "query": {
        "range" : {
            "user_id" : {
                "gte" : 276379,
                "lte" : 276381
            }
        }
    }
}
```

[前缀查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-prefix-query.html)

前缀查询只能作用在 text 和 keyword 的类型上

通配符查询 和 正则表达式查询

ids 查询

```text
GET /blogs/sina/_search
{
    "query": {
        "ids" : {
            "values" : ["1", "2", "3"]
        }
    }
}
```