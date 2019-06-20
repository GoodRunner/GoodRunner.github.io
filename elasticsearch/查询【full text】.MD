### [查询语法](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html)

ES提供了一套基于`json`的Query DSL(Domain Specific Language) 来定义查询。

[全文本查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/full-text-queries.html)

包含了 match、match_phrase、match_phrase_prifix、multi_match、common、query_string、intervals query

[Match](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html)

```text
GET /blogs/sina/_search
{
    "query": {
        "match": {
          "title": "四川 震"
        }
    }
}
```
match查询是一种boolean查询，提供的搜索内容被解析过，`operator`操作符指定了各个词之间的关系（默认是or），这里也可以指定解析器默认是standard

```text
GET /blogs/sina/_search
{
    "query": {
        "match": {
          "content": {
            "query":"四川 震",
            "operator":"or",
            "analyzer":"simple"
          }
        }
    }
}
```

[Common查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-common-terms-query.html)

问题来源： 假如需要查询 "The brown fox"，假如这个需要三个查询"The"，"brown"，"fox",由于"The"是高频词，查询"the"可能要与更多文档匹配，以前，这个问题的解决方案是忽略高频率的术语。通过将其"the"视为停用词，我们减少了索引大小并减少了需要执行的术语查询的数量。这种方法的问题在于，虽然停用词对相关性的影响很小，但它们仍然很重要。如果我们删除了停用词，我们就会失去精确度（例如，我们无法区分"happy" 和"not happy"），并且我们会失去回忆（例如，文本在索引中"The The"或者 "To be or not to be"根本不存在）。

解决方案： 该common术语的查询将所述查询术语分为两组：更重要（即低频率词汇）和不太重要的（即，高频率而言 先前是将他们停用）。
 1. 首先，它搜索与更重要的术语匹配的文档。这些术语出现在较少的文档中，对相关性有较大影响。
    
 2. 然后，它对不太重要的术语执行第二次查询 - 这些术语经常出现并且对相关性的影响很小。但是，它不是计算所有匹配文档的相关性分数，而是仅计算_score已经与第一个查询匹配的文档。通过这种方式，高频项可以改善相关性计算，而无需支付性能不佳的成本。
 
 也就是说通过给出的查询词汇做两次操作，第一次根据重要词汇查询结果，第二次根据不重要的词汇计算查询出来的文档的 `score`

```text
GET /blogs/sina/_search
{
    "query": {
        "common": {
            "content": {
                "query": "this is bonsai cool",
                "cutoff_frequency": 0.001
            }
        }
    }
}
```

[query_string](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html)

[intervals_query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-intervals-query.html)