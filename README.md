# docker nginx vhost

![image](https://github.com/pySatellite/docker-nginx-vhost/assets/87309910/878eaf6a-18bc-4467-8b3f-5086de8ff3a1)

# load-balancing
- https://www.nginx.com/resources/glossary/load-balancing/

# jenkins Dockerfile

## Dockerfile 생성

```
$ tree
.
├── README.md
├── lb
│   ├── Dockerfile
│   └── config
│       └── default.conf
├── serv-a
│   ├── Dockerfile
│   └── index.html
└── serv-b
    ├── Dockerfile
    └── index.html

# lb
FROM nginx
COPY config/default.conf /etc/nginx/conf.d/

# serv-a / serv-b
FROM nginx
COPY index.html /usr/share/nginx/html
```

## build & Run
```
docker build -t minhee84/lb:0.1.0 -f lb/Dockerfile lb/
docker build -t minhee84/serv-a:0.1.0 -f serv-a/Dockerfile serv-a/
docker build -t minhee84/serv-b:0.1.0 -f serv-b/Dockerfile serv-b/
```

## Docker login
```
docker login -u minhee84 -p dalsgml78!
```

## Docker push
```
docker push minhee84/serv-a:0.1.0
docker push minhee84/serv-b:0.1.0
docker push minhee84/lb:0.1.0
```

# network connet
```
docker run -d --name serv-a --network lb-net minhee84/serv-a:0.1.0
docker run -d --name serv-b --network lb-net minhee84/serv-b:0.1.0
docker run -d -p 8001:80 --name lb --network lb-net minhee84/lb:0.1.0
```

- test 2
```
$ curl http://localhost:8001
<h1>A</h1>
$ curl http://localhost:8001
<h1>B</h1>
$ curl http://localhost:8001
<h1>A</h1>
```

# ref
- https://hub.docker.com/
