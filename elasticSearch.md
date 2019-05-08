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