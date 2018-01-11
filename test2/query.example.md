##### 范围价格查询

```javascript
curl -XGET 'http://mydocker:9200/_search?pretty' -H 'Content-Type: application/json' -d'
{   "_source":{ "includes":["basicInfo","basicInfo.goodsPicPath"]},
    "query": {
        "range" : {
            "basicInfo.currentPCPrice" : {
                "gte" : 285,
                "lte" : 9186,
                "boost" : 2.0
            }
        }
    }
}
'
```

##### 指定字段模糊匹配

```javascript

curl -XGET 'http://mydocker:9200/_search?pretty' -H 'Content-Type: application/json' -d'
{   "_source":{ "includes":["basicInfo.title"]},
    "query": {
        "match" : {
            "basicInfo.title" : "Bose"
        }
    },
    "from":0,
    "size":5
}
'

```

##### 多个字段模糊

```javascript


curl -XGET 'http://mydocker:9200/_search?pretty' -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "should": [
        { "match": { "data.title":  "手机" }}, //or的关系
        { "match": { "data.specification": "华为"   }},
        "operator": "and"
      ]
    }
  }
}
'


curl -XGET 'http://mydocker:9200/_search?pretty' -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "must": [
        { "term": { "data.title":  "华为" }}, 
        { "term": { "data.specification": "华为"   }}
      ]
    }
  }
}
'

curl -XGET 'http://localhost:9200/_search?pretty' -H 'Content-Type: application/json' -d'
{
"_source":{ "includes":["basicInfo"]},
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "basicInfo.specifications" : "26cm(含)-30cm(含)"
          }
        },
        {
          "match": {
            "basicInfo.title": "爱仕达不粘锅"
          }
        }
      ]
    }
  },
  "from":0,
  "size":10
}
'

curl -XGET 'http://localhost:9200/_search?pretty' -H 'Content-Type: application/json' -d'
{   "_source":{ "includes":["basicInfo"]},
    "query": {
        "match" : {
            "basicInfo.title" : "马桶"
        }
    },
    "from":0,
    "size":5
}
'

curl -XGET 'http://localhost:9200/_search?pretty' -H 'Content-Type: application/json' -d'
{   "_source":{ "includes":["basicInfo"]},
    "query": {
        "match" : {
            "basicInfo.title" : "马桶"
        }
    },
    "from":0,
    "size":5
}
'

curl -XGET 'http://localhost:9200/_search?pretty' -H 'Content-Type: application/json' -d'
{   "_source":{ "includes":["basicInfo"]},
    "query": {
        "match" : {
            "urlId" : "8cfb6239670e6579e1a47677b7334851"
        }
    }
}
'


curl -XGET 'http://localhost:9200/alice.standard.categories.features2/alice.standard.categories.features2/_search?pretty' -H 'Content-Type: application/json' -d'
{
    "query": {
        "match" : {
            "name" : "智能"
        }
    },
    "from":0,
    "size":5
}
'
```