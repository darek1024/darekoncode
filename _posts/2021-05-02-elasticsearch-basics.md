---
title: 'Introduction to Elasticsearch Architecture'
date: 2021-05-02 07:30:00
---

Elasticsearch is a distributed storage engine optimized for search operations.

Data is stored in Elasticsearch as JSON documents organized in indices. In older versions (prior 7.x) indices was additionaly splitted into types, but they were dropped.

Indices can define mappings (like table schema in SQL) for stored documents, but it is not mandatory. It is possible to set mapping validation as strict - meaning only documents matching the mapping can be indexed (saved) into the database.

One Elasticsearch instance (cluster) can contain a number of indices. One index is built on top of multiple shards. Shard is a minimal non-dividable piece of storage. Shards of one index can be physically located on different machines (nodes).

When defining and index one must define how many shards and replicas it should be built of. While it's possible to change replica factor on the ongoing index, it is not possible to change the number of primary shards, so you must think carefully what would be the best number for you.

Elasticsearch should be started on multiple nodes, preferably at least 3. That number would make it easy to ensure high availability on the network errors.

Elasticsearch exposes REST API for communication and uses its own language for querying the data.
The best resource to learn it is the official [Elasticsearch Guide](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html).

In order to start playing with Elasticsearch you can do it with Docker Image.

```shell
$ docker run -p 9200:9200 -e "discovery.type=single-node" \
	elastic/elasticsearch:7.12.0
```

Index (save) first document.

```shell
$ echo '{"tweet": "I am back", "author": "Michael Jordan"}' \
	| http POST localhost:9200/tweets/_doc

HTTP/1.1 201 Created
Location: /tweets/_doc/pc6kLHkB_gnelhcLdgbo
content-encoding: gzip
content-length: 159
content-type: application/json; charset=UTF-8

{
    "_id": "pc6kLHkB_gnelhcLdgbo",
    "_index": "tweets",
    "_primary_term": 1,
    "_seq_no": 0,
    "_shards": {
        "failed": 0,
        "successful": 1,
        "total": 2
    },
    "_type": "_doc",
    "_version": 1,
    "result": "created"
}	
```

Search for all documents.

```shell
$ http localhost:9200/_search

{
  "_shards": {
    "failed": 0,
    "skipped": 0,
    "successful": 1,
    "total": 1
  },
  "hits": {
    "hits": [
      {
        "_id": "pc6kLHkB_gnelhcLdgbo",
        "_index": "tweets",
        "_score": 1.0,
        "_source": {
          "author": "Michael Jordan",
          "tweet": "I am back"
        },
        "_type": "_doc"
      }
    ],
    "max_score": 1.0,
    "total": {
      "relation": "eq",
      "value": 1
    }
  },
  "timed_out": false,
  "took": 4
}
```

Search with a query.


```shell
$ echo '{"query": { "match" : { "author": "jordan" } } }' \
	| http POST localhost:9200/tweets/_search

{
    "_shards": {
        "failed": 0,
        "skipped": 0,
        "successful": 1,
        "total": 1
    },
    "hits": {
        "hits": [
            {
                "_id": "pc6kLHkB_gnelhcLdgbo",
                "_index": "tweets",
                "_score": 0.2876821,
                "_source": {
                    "author": "Michael Jordan",
                    "tweet": "I am back"
                },
                "_type": "_doc"
            }
        ],
        "max_score": 0.2876821,
        "total": {
            "relation": "eq",
            "value": 1
        }
    },
    "timed_out": false,
    "took": 22
}	
```

## Wrap up
Elasticsearch is a widely-adopted search engine and in this article you learned the basics of it. Please share it with one person for whom it may be useful. Thanks 🙌