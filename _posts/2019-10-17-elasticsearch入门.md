---
title: elasticsearch入门
tags: elasticsearch  
key: 20191017
published: false
---

## 基础概念
先来看看[官方定义](https://www.elastic.co/guide/en/elasticsearch/reference/6.4/getting-started-concepts.html)
+ 集群(Cluster)   
  A cluster is a collection of one or more nodes (servers) that together holds your entire data and provides federated indexing and search capabilities across all nodes. 
+ 节点(Node)   
  A node is a single server that is part of your cluster, stores your data, and participates in the cluster’s indexing and search capabilities.
+ 索引(Index)   
  An index is a collection of documents that have somewhat similar characteristics. 
+ 类型(Type)   
  A type used to be a logical category/partition of your index to allow you to store different types of documents in the same index, eg one type for users, another type for blog posts. 
+ 文档(Document)   
  A document is a basic unit of information that can be indexed. 

## 使用指南
### 1.健康检查
curl 'localhost:9200/_cat/health?v'

绿色表示一切正常, 黄色表示所有的数据可用但是部分副本还没有分配,红色表示部分数据因为某些原因不可用
### 2.查看节点列表
curl 'localhost:9200/_cat/nodes?v'

### 查看






+ ### PUT localhost:9200/image
```json
{
    "mappings": {
        "_doc": {
            "properties": {
                "url": {
                    "type": "keyword"
                },
                "ahash": {
                    "type": "binary",
                    "doc_values": true
                }
            }
        }
    }
}
```

+ ### PUT localhost:9200/image/_doc/http://f1.kkmh.com/FtkBwex9y_g7a9aKDUH2cFZ8Tn-3
```json
{
	"ahash":"ADEAMQAwADAAMAAwADEAMQAxADEAMAAwADAAMAAxADEAMQAxADAAMAAwADAAMQAxADEAMAAxADAAMAAwADEAMQAxADEAMQAwADAAMAAwADEAMQAxADAAMAAwADAAMQAxADEAMQAxADEAMAAwADEAMQAxADEAMQAxADEAMAAxADEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA="
}
```

+ ### POST localhost:9200/image/_search
```json
{
  "query": {
    "function_score": {
    "boost_mode" : "replace",
        "functions": [
          {
            "script_score": {
              "script": {
                  "source": "yshhuang",
                  "lang" : "vector-distance",
                  "params": {
                      "field": "ahash",
                      "method":"hamming",
                      "value" : "1100001111000011110000111010001111100001110000111111001111111011"
                  }
              }
            }
          }
        ]
    }
  }
}
```

+ ### 加字段  PUT localhost:9200/image/_mapping/_doc
```json
{
    "_doc": {
        "properties": {
            "dhash": {
                "type": "binary",
                "doc_values": true
            },
            "phash": {
                "type": "binary",
                "doc_values": true
            }
        }
    }
}
```

## 参考资料
1. [Elasticsearch入门篇——基础知识](https://segmentfault.com/a/1190000018466470#articleHeader4)
2. [Removal of mapping types](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/removal-of-types.html#removal-of-types)
3. [Basic Concepts](https://www.elastic.co/guide/en/elasticsearch/reference/6.4/getting-started-concepts.html)
4. [ElasticSearch 索引查询使用指南——详细版](https://www.cnblogs.com/pilihaotian/p/5830754.html)
5. [cat APIs](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat.html)