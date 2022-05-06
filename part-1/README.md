# Beginner Course for Elasticsearch

## Part 1: Intro to Elasticsearch & Kibana

Material reference - [YouTube](https://www.youtube.com/watch?v=gS_nHTWZEJ8&list=PL_mJOmq4zsHZYAyK606y7wjQtC0aoE6Es&index=1)

### Contents
* Basic Operations
* CRUD Operations

## Basic Operations

### Get info about cluster health

```
GET _cluster/health
```
Response from Elasticsearch - 
```
{
  "cluster_name" : "elasticsearch-demo",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 9,
  "active_shards" : 9,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}
```

### Get info about nodes in a cluster
```
GET _nodes/stats
```
Response from Elasticsearch - 
```
{
  "_nodes" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "cluster_name" : "elasticsearch-demo",
  "nodes" : {
    "6hK2maHwQ5S-lXh_omRZiA" : {
      "timestamp" : 1651812520104,
      "name" : "node-1",
      "transport_address" : "127.0.0.1:9300",
      "host" : "127.0.0.1",
      "ip" : "127.0.0.1:9300",
      "roles" : [
        "data",
        "data_cold",
        "data_content",
        "data_frozen",
        "data_hot",
        "data_warm",
        "ingest",
        "master",
        "ml",
        "remote_cluster_client",
        "transform"
      ],
      .
      .
      .
```

## CRUD Operations

## Create

### Create Index
```
PUT favorite_car
```
Response from Elasticsearch - 
```
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "favorite_car"
}
```
### Index a document

When indexing a document, both HTTP verbs POST or PUT can be used.
* POST will autogenerate the id for the document

```
POST favorite_car/_doc
{
  "first_name": "Adam",
  "car": "Ford Raptor"
}
```
Response from Elasticsearch - 
```
{
  "_index" : "favorite_car",
  "_id" : "wka-l4ABfJgEdScgEvXH",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```
* PUT will allow you to add specific id to document
```
PUT favorite_car/_doc/1
{
  "first_name": "Betty",
  "car": "Viper"
}
```
Response from Elasticsearch - 
```
{
  "_index" : "favorite_car",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 1,
  "_primary_term" : 1
}
```

### _create Endpoint
When you index a document with an existing id, the existing document gets overwritten. To avoid this, we use _create endpoint.

```
PUT favorite_car/_create/1
{
  "first_name": "Finn",
  "car": "Range Rover"
}
```
We already have a document with id = 1

Response from Elasticsearch - 
```
{
  "error" : {
    "root_cause" : [
      {
        "type" : "version_conflict_engine_exception",
        "reason" : "[1]: version conflict, document already exists (current version [1])",
        "index_uuid" : "PXBUx9qTTwOhsNVoJkqFYw",
        "shard" : "0",
        "index" : "favorite_car"
      }
    ],
    "type" : "version_conflict_engine_exception",
    "reason" : "[1]: version conflict, document already exists (current version [1])",
    "index_uuid" : "PXBUx9qTTwOhsNVoJkqFYw",
    "shard" : "0",
    "index" : "favorite_car"
  },
  "status" : 409
}
```

## READ

### Read a document
```
GET favorite_car/_doc/1
```
Response from Elasticsearch - 
```
{
  "_index" : "favorite_car",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 1,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "first_name" : "Betty",
    "car" : "Viper"
  }
}
```

## UPDATE

### Update a document
```
POST favorite_car/_update/1
{
  "doc": {
    "car": "Maserati"
  }
}
```
Response from Elasticsearch - 
```
{
  "_index" : "favorite_car",
  "_id" : "1",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 2,
  "_primary_term" : 1
}
```

## DELETE

### Delete a document
```
DELETE favorite_car/_doc/1
```
Response from Elasticsearch - 
```
{
  "_index" : "favorite_car",
  "_id" : "1",
  "_version" : 3,
  "result" : "deleted",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 3,
  "_primary_term" : 1
}
```

## ASSIGNMENT

1. Create an index called places.
```
PUT places

{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "places"
}
```
2. Pick five of the places you want to visit after the pandemic is over. For each place, index a document containing the name and the country.
```
PUT places/_create/1
{
  "name": "Rishabh",
  "country": "Bali"
}

{
  "_index" : "places",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```
```
PUT places/_create/2
{
  "name": "Rishabh",
  "country": "UAE"
}

{
  "_index" : "places",
  "_id" : "2",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 1,
  "_primary_term" : 1
}
```
```
PUT places/_create/3
{
  "name": "Rishabh",
  "country": "Norway"
}

{
  "_index" : "places",
  "_id" : "3",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 2,
  "_primary_term" : 1
}
```
```
PUT places/_create/4
{
  "name": "Rishabh",
  "country": "UK"
}

{
  "_index" : "places",
  "_id" : "4",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 3,
  "_primary_term" : 1
}
```
```
PUT places/_create/5
{
  "name": "Rishabh",
  "country": "Sweden"
}

{
  "_index" : "places",
  "_id" : "5",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 4,
  "_primary_term" : 1
}
```

3. Read(GET) each document to check the content of the document.
```
GET places/_doc/3

{
  "_index" : "places",
  "_id" : "3",
  "_version" : 1,
  "_seq_no" : 2,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "Rishabh",
    "country" : "Norway"
  }
}
```

4. Update a field of a document.
```
POST places/_update/4
{
  "doc": {
    "country": "Thailand"
  }
}

{
  "_index" : "places",
  "_id" : "4",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 5,
  "_primary_term" : 1
}
```

5. Read(GET) the updated document to ensure that the field has been updated.
```
GET places/_doc/4

{
  "_index" : "places",
  "_id" : "4",
  "_version" : 2,
  "_seq_no" : 5,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "Rishabh",
    "country" : "Thailand"
  }
}
```
6. Delete a document of one place.
```
DELETE places/_doc/4

{
  "_index" : "places",
  "_id" : "4",
  "_version" : 3,
  "result" : "deleted",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 6,
  "_primary_term" : 1
}
```

7. Copy and paste the following request to return all documents from the places index. This is a great way to check whether all the CRUD operations you have performed thus far have worked!
```
GET places/_search
{
  "query": {
    "match_all": {}
  }
}
```
```
{
  "took" : 397,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "places",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "name" : "Rishabh",
          "country" : "Bali"
        }
      },
      {
        "_index" : "places",
        "_id" : "2",
        "_score" : 1.0,
        "_source" : {
          "name" : "Rishabh",
          "country" : "UAE"
        }
      },
      {
        "_index" : "places",
        "_id" : "3",
        "_score" : 1.0,
        "_source" : {
          "name" : "Rishabh",
          "country" : "Norway"
        }
      },
      {
        "_index" : "places",
        "_id" : "5",
        "_score" : 1.0,
        "_source" : {
          "name" : "Rishabh",
          "country" : "Sweden"
        }
      }
    ]
  }
}
```