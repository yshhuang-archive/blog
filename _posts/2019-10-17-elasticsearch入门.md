---
title: elasticsearch入门
tags: elasticsearch  
key: 20191017
published: false
---
+ ### PUT localhost:9200/image-feature/
```json
{
    "mappings": {
        "image": {
            "properties": {
                "md5": {
                    "type": "keyword"
                },
                "surfFeature": {
                    "type": "byte"
                }
            }
        }
    }
}
```