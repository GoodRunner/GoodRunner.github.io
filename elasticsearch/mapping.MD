### mapping
这章我们大概介绍这几个问题
1. 什么是es的mapping
2. 如何自定义mapping
3. 如何查看index的mapping

> 什么是es的mapping

在es中也可以定义存储的文档的字段类型(可以理解为关系型数据库的表结构)，在es中大概有一下集中字段类型

1. 简单一点的类型 text、keyword、date、long、double、boolean、ip 等 [详见官网](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html)      
2. 支持json特性的类型 object 和 nested.
3. 特殊类型  geo_point, geo_shape 和 completion

> 自定义mapping 

定义了`blogs`这个index下`sina`这个type的四个字段的类型

```text

PUT /blogs
{
   "settings" : {
      "number_of_shards" : 3,
      "number_of_replicas" : 1
   },
   "mappings": {
     "sina":{
       "properties": {
               "date": {"type": "date"},
               "title": {"type": "text"},
               "content": {"type": "text"},
               "user_id": {"type": "long"}
            }
     }
   }
}
```

> 查看index的mapping


```GET /blogs/_mapping/sina```

#### 动态映射

如果你没有设置mapping而且向es中放入了数据，es会自己判断而设定一个类型。

#### 修改映射

如果该index中有数据，理论上是不能修改的，但是你可以创建一个新的index并用一下语句将文档索引进这个index

```text
POST _reindex
{
  "source":{
    "index":"index_name"
  },
  "dest":{
    "index":"index_name"
  }
}
```
