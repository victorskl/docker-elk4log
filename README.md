# docker-elk4log

Yet another Docker ELK stack. Making it ready for logging peer containers. 

### On Localhost

```
git https://github.com/victorskl/docker-elk4log.git
cd docker-elk4log
mkdir data
chown 1000:1000 data

docker-compose --project-name=elk build
docker-compose --project-name=elk up -d
docker ps
```

Wait awhile to bootstrap the ElasticSearch. Meantime, the `logspout` container will keep restarting until ELK stack is ready. 

To check ElasticSerach is ready:

```
curl http://localhost:9200
```

You should see like this message:

```
{
  "name" : "fLAxBuh",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "mTzETulvRVmVGEkH5YRwCw",
  "version" : {
    "number" : "6.2.1",
    "build_hash" : "7299dc3",
    "build_date" : "2018-02-07T19:34:26.990113Z",
    "build_snapshot" : false,
    "lucene_version" : "7.2.1",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

Once ElasticSearch is ready, next is to check the Kibana:

```
curl http://localhost:5601
```

And you should get:

```
<script>var hashRoute = '/app/kibana';
var defaultRoute = '/app/kibana';

var hash = window.location.hash;
if (hash.length) {
  window.location = hashRoute + hash;
} else {
  window.location = defaultRoute;
}</script>%
```

At this point, you can visit to [http://localhost:5601/app/kibana#/management/kibana/index?_g=()](http://localhost:5601/app/kibana#/management/kibana/index?_g=())

You should see the index pattern with name `logstash-2018.02.20` by auto-scan. Provide the name in the index pattern text box `logstash*`, add `@timestamp` filter and create the index pattern.


Next visit to [http://localhost:5601/app/kibana#/discover?_g=()](http://localhost:5601/app/kibana#/discover?_g=()) and see the containers logs of ELK stack itself.


### Running with HAProxy

Running with HAProxy in front of ELK stack requires to modify the Kibana `basePath`. Open `kibana/config/kibana.yml` and uncomment `#server.basePath: "/kibana"`. 

```
docker-compose --project-name=elk down
vi kibana/config/kibana.yml

grep basePath kibana/config/kibana.yml                                                                       
    server.basePath: "/kibana"
    
docker-compose --project-name=elk up -d
```

Wait for ElasticSearch bootstrapping `curl http://localhost:9200`. When ready, `curl http://localhost:5601` and you should get:

```
<script>var hashRoute = '/kibana/app/kibana';
var defaultRoute = '/kibana/app/kibana';

var hash = window.location.hash;
if (hash.length) {
  window.location = hashRoute + hash;
} else {
  window.location = defaultRoute;
}</script>%
```

_Note that the path now changed to `/kibana/app/kibana`_.


Then in your HAProxy configuration [should go like this](https://stackoverflow.com/questions/32992330/how-to-access-kibana-dashboard-via-haproxy/46902097#46902097):

```
frontend web
    bind *:80
    acl kibana_path path_beg -i /kibana
    use_backend kibana if kibana_path

backend kibana
    mode http
    reqrep ^([^\ ]*)\ /kibana[/]?(.*) \1\ /\2
    server kibana_1 127.0.0.1:5601
```

This will give you, `http://app-dev.ur-domain-name.com/kibana`

### REF

Inspired and make use of the following projects.

- https://www.docker.elastic.co
- https://github.com/elastic/elasticsearch-docker
- https://github.com/elastic/logstash-docker
- https://github.com/elastic/kibana-docker
- https://github.com/deviantony/docker-elk
- https://github.com/looplab/logspout-logstash


