# Docker Elasticsearch

## Get started

Initialize single node elasticsearch docker container
```bash
docker compose up --build -d
```

Initialize single node elasticsearch and kibana docker containers
```bash
docker compose -f docker-compose.single.yaml up --build -d
```

Initialize basic elasticsearch and kibana docker containers
```bash
docker compose -f docker-compose.basic.yaml up --build -d
```

Initialize elasticsearch cluster and kibana docker containers
```bash
docker compose -f docker-compose.cluster.yaml up --build -d
```

## Clean up

Remove single node elasticsearch docker container
```bash
docker compose down -v
```

Remove single node elasticsearch and kibana docker containers
```bash
docker compose -f docker-compose.single.yaml down -v
```

Remove elasticsearch and kibana docker containers
```bash
docker compose -f docker-compose.basic.yaml down -v
```

Remove elasticsearch cluster and kibana docker containers
```bash
docker compose -f docker-compose.cluster.yaml down -v
```

## Useful request queries

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

## Links

|Configuration|Container|Credentials|
|---|---|---|
|Single|[Elasticsearch](http://localhost:9200)<br/>[Kibana](http://localhost:5601)|None|
|Basic|[Elasticsearch](http://elastic:secret@localhost:9200)<br/>[Kibana](http://localhost:5601)|user: elastic <br/> password: secret|
|Cluster|[Elasticsearch](https://elastic:secret@localhost:9200)<br/>[Kibana](https://localhost:5601)|user: elastic<br/>password: secret<br/>|

### Known errors

**Error 1:**

> bootstrap check failure [1] of [2]: memory locking requested for elasticsearch process but memory is not locked

**Solution**

Include the following configuration in elasticsearch node containers:

```yaml
    mem_limit: ${MEM_LIMIT}
    ulimits:
      memlock:
        soft: -1
        hard: -1
```

[Reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#_prepare_the_environment)

**Error 2:**

> bootstrap check failure [2] of [2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

**Solution:**

Run the following commands in your local computer:

```bash
wsl -d docker-desktop
sysctl -w vm.max_map_count=262144
```

[Reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#_windows_with_docker_desktop_wsl_2_backend)

### Documentation

#### 8.4

https://www.elastic.co/guide/en/elasticsearch/reference/8.4/docker.html

https://www.elastic.co/guide/en/kibana/8.4/docker.html

#### 7.17

https://www.elastic.co/guide/en/elasticsearch/reference/7.17/docker.html

https://www.elastic.co/guide/en/kibana/7.17/docker.html