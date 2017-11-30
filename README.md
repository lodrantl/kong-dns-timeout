## Basic test case for the 20s on DNS resolution inside docker network

## Usage

Start kong (wait a bit between executions)
```
docker-compose up -d kong-database
docker-compose up kong-migration
docker-compose up -d kong
```

Register a client pointing to nginx
```
curl -i -X POST \
  --url http://localhost:8001/apis/ \
  --data 'name=test' \
  --data 'uris=/test' \
  --data 'upstream_url=http://nginx'
```

Try the request, it takes 20s.
```
curl -v http://localhost:8001/test
```

Proof it is a problem of DNS resolution
```
docker-compose exec kong bash
ping nginx
# write down the ip adress and create another client
curl -i -X POST \
  --url http://localhost:8001/apis/ \
  --data 'name=test_static' \
  --data 'uris=/test_static' \
  --data 'upstream_url=http://{ip_of_nginx_container}'
```

This one will work.


