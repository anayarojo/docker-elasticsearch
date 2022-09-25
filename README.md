# Docker Elasticsearch

### Get started

Initialize single node elasticsearch docker container
```bash
docker compose -f docker-compose.single.yaml up --build
```

Initialize basic elasticsearch and kibana docker containers
```bash
docker compose -f docker-compose.basic.yaml up --build
```

### Useful request queries

Elasticsearch status API

```bash
curl -X GET  http://elastic:secret@localhost:9200/_cluster/health
curl -X GET  http://elastic:secret@localhost:9200/_cluster/health?pretty
curl -X GET  http://elastic:secret@localhost:9200/_cluster/stats?pretty
curl -X GET  http://elastic:secret@localhost:9200/_cluster/stats?human
curl -X GET  http://elastic:secret@localhost:9200/_cluster/stats?human
curl -X GET  http://elastic:secret@localhost:9200/_cluster/stats?human
curl -X GET  http://elastic:secret@localhost:9200/_cat/master?v
curl -X GET  http://elastic:secret@localhost:9200/_cat/master?v
curl -X GET  http://elastic:secret@localhost:9200/_cat/shards?v
```

Elasticsearch indices and documents API

Index
```bash
curl -X PUT 'http://elastic:secret@localhost:9200/twitter/tweet/1?pretty' -H 'Content-Type: application/json' -d'
{
    "user" : "admin",
    "post_date" : "2018-07-08T11:02:12",
    "message" : "Test in Elasticsearch Tweet1"
} '

curl -X PUT 'http://elastic:secret@localhost:9200/twitter/tweet/2?pretty' -H 'Content-Type: application/json' -d'
{
    "user" : "user1",
    "post_date" : "2018-07-08T11:03:09",
    "message" : "Test in Elasticsearch Tweet2"
} '


curl -X PUT 'http://elastic:secret@localhost:9200/twitter/tweet/3?pretty' -H 'Content-Type: application/json' -d'
{
    "user" : "user1",
    "post_date" : "2018-07-08T11:02:12",
    "message" : "Test in Elasticsearch Tweet3"
} '

curl -X PUT 'http://elastic:secret@localhost:9200/twitter/tweet/4?pretty' -H 'Content-Type: application/json' -d'
{
    "user" : "user1",
    "post_date" : "2018-07-08T11:05:47"
    "message" : "Test in Elasticsearch Tweet4"
} '

curl -X PUT 'http://elastic:secret@localhost:9200/twitter/tweet/5?pretty' -H 'Content-Type: application/json' -d'
{
    "user" : "user1",
    "post_date" : "2018-07-08T11:08:31",
    "message" : "Test in Elasticsearch Tweet5"
} '

```

Get
```bash
curl -X GET 'http://elastic:secret@localhost:9200/twitter/tweet/1?pretty'
```

Delete
```bash
curl -X DELETE 'http://elastic:secret@localhost:9200/twitter/tweet/5?pretty'
```

Update
```bash
curl -X PUT 'http://elastic:secret@localhost:9200/twitter/tweet/3?pretty' -H 'Content-Type: application/json' -d'
{
    "script" : "ctx._source.likes = \"enable\""
} '
```

Search
```bash
curl -X GET 'http://elastic:secret@localhost:9200/twitter/_search?q=user:user1&pretty'
curl -X GET 'http://elastic:secret@localhost:9200/_search?q=likes:enable&pretty'

curl -X POST 'http://elastic:secret@localhost:9200/twitter/tweet/_search?pretty' -H 'Content-Type: application/json' -d'
{
    "query" : {
        "term": { "user": "user1" }
    }
} '
```

Documentation: 

### Links

#### Elasticsearch

http://localhost:9200

http://elastic:secret@localhost:9200

#### Kibana

http://localhost:5601


### Documentation

https://www.elastic.co/guide/en/elasticsearch/reference/8.4/docker.html

https://www.elastic.co/guide/en/kibana/8.4/docker.html

