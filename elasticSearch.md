### Elasticsearch 入门

先介绍一些基本概念

| ElasticSearch概念 | Mysql 概念 | 说明                               |
| ----------------- | ---------- | ---------------------------------- |
| _index            | 库         | elastic中的_index相当于mysql中的库 |
| _type             | 表         | elastic中的_type相当于mysql中的表  |
| _id               | 主键       | elastic中的_id相当于mysql中的主键  |

基本的增删改查，elastic的命令是http的json格式的

#### 建库 

以下命令相当于在elastic中创建了一个名为 `mydata`的`index` 这个命令不是必须的，即使向没有创建的`index`中写数据，`elastic`会自动创建`index` 

```
PUT /mydata
```

#### 添加数据

以下命令相当于向`mydata`(库)的`student`(表)中添加了一个主键是1的数据

```
PUT /mydata/student/1
{"name":"ma","age":"18"}
```

#### 删除数据 

```
DELETE /mydata/student/1
```



#### 修改数据

只会修改doc中指定的属性的值

```
POST /mydata/student/1/_update
{"doc":{"name":"haha"}}
```

#### 查询数据

```
GET /mydata/student/1 查询一个

GET /mydata/student/_search  查询student表中所有的
```

#### 分页查询
`from`是指偏移量，`size`是指每页大小
```text
GET /ielts/paper/_search
{
  "from":"0",
  "size":"1"
}

```

#### 返回指定列
其中`source`是返回指定列明的数组
```text
GET /ielts/paper/_search
{
  "_source": ["name"]
}
```

#### 匹配查询
相当于`sql`中`where`后面的
```text
GET /bank/_search
{
  "query": { "match": { "account_number": 20 } }
}
这条语句返回account_number是20的记录
```

```text
GET /bank/_search
{
  "query": { "match": { "address": "mill" } }
}
这条语句返回address包含mill的记录
```

```text
GET /bank/_search
{
  "query": { "match": { "address": "mill lane" } }
}
这条语句返回address包含mill或者（or）lane的记录
```

```text
GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}
这条语句返回address包含mill和lane的记录
```

以上我们可以看到我们使用了bool查询，可以将一下小的查询组装成一个更大的查询语句。


介绍一下`bool`查询

[传送门](https://www.elastic.co/guide/en/elasticsearch/reference/7.1/query-dsl-bool-query.html)
![bool query](https://user-images.githubusercontent.com/10717670/58622560-75225880-82fe-11e9-9cd7-3b65c07a3716.png)

应用场景：

日志分析、全文检索、站内搜索、企业级数据搜索、舆情分析、全文分析、安全分析