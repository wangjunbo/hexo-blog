title: Elasticsearch的mapping设置
tags:
  - Elasticsearch
categories: []
date: 2016-01-27 18:08:00
---
```elasticsearch
PUT /my_index
{
    "mappings": {
        "my_type": {
            "properties": {
                "title":  {
					"type": "string",
					"index":    "analyzed",
					"analyzer": "ik_smart"
				}
            }
        }
    }
}
```