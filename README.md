# presto-gateway
A load balancer / proxy / gateway for prestodb

## How to build and start 

run `mvn clean install` to build `presto-gateway`

Edit the config file and update backend urls `https://github.com/lyft/presto-gateway/blob/master/gateway/src/main/resources/config.yml.template#L9` 

```
cd gateway/target/
java -jar gateway-{{VERSION}}-jar-with-dependencies.jar server  /Users/pjaiswal/src/presto-gateway/gateway/src/main/resources/config.yml.template
```


## Gateway API

### Get all backends behind the gateway

`curl -X GET localhost:8090/gateway/backend/all | python -m json.tool`
```
[
    {
        "includeInRouter": true,
        "localPort": 8082,
        "name": "presto2",
        "prefix": "/",
        "preserveHost": "true",
        "proxyTo": "http://presto2.lyft.com",
        "trustAll": "true"
    },
    {
        "includeInRouter": true,
        "localPort": 8081,
        "name": "presto1",
        "prefix": "/",
        "preserveHost": "true",
        "proxyTo": "http://presto1.lyft.com",
        "trustAll": "true"
    }
]
```

### Get active backends behind the Gateway

`curl -X GET localhost:8090/gateway/backend/active | python -m json.tool`
```
[
    {
        "includeInRouter": true,
        "localPort": 8081,
        "name": "presto1",
        "prefix": "/",
        "preserveHost": "true",
        "proxyTo": "http://presto1.lyft.com",
        "trustAll": "true"
    },
    {
        "includeInRouter": true,
        "localPort": 8082,
        "name": "presto2",
        "prefix": "/",
        "preserveHost": "true",
        "proxyTo": "http://presto2.lyft.com",
        "trustAll": "true"
    }
]
```
### Deactivate a backend 

`curl -X POST localhost:8090/gateway/backend/deactivate/presto2`

Verify this by calling get active backends
```
curl -X GET localhost:8090/gateway/backend/active | python -m json.tool
[
    {
        "includeInRouter": true,
        "localPort": 8081,
        "name": "presto1",
        "prefix": "/",
        "preserveHost": "true",
        "proxyTo": "http://presto1.lyft.com",
        "trustAll": "true"
    }
]
```
### Activate a backend 

curl -X POST localhost:8090/gateway/backend/activate/presto2

Verify this by calling get active backends
```
curl -X GET localhost:8090/gateway/backend/active | python -m json.tool

[
    {
        "includeInRouter": true,
        "localPort": 8082,
        "name": "presto2",
        "prefix": "/",
        "preserveHost": "true",
        "proxyTo": "http://presto2.lyft.com",
        "trustAll": "true"
    },
    {
        "includeInRouter": true,
        "localPort": 8081,
        "name": "presto1",
        "prefix": "/",
        "preserveHost": "true",
        "proxyTo": "http://presto1.lyft.com",
        "trustAll": "true"
    }
]
```