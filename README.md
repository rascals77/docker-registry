# Setup

1. Enable docker to use the insecure registry by creating the ```/etc/docker/daemon.json``` file:

```
$ sudo vi /etc/docker/daemon.json
```

with this content:

```json
{
  "insecure-registries": [
    "localhost:5000"
  ]
}
```

2. Restart docker:

```
$ sudo systemctl restart docker
```

3. Download some required programs:

```
$ sudo curl -L -o /usr/local/bin/jq https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64
$ sudo curl -L -o /usr/local/bin/docker-compose https://github.com/docker/compose/releases/download/1.23.1/docker-compose-Linux-x86_64
$ sudo curl -L -k -o /usr/local/bin/cfssl https://pkg.cfssl.org/R1.2/cfssl_linux-amd64
$ sudo curl -L -k -o /usr/local/bin/cfssljson https://pkg.cfssl.org/R1.2/cfssljson_linux-amd64
$ sudo chmod +x /usr/local/bin/{jq,docker-compose,cfssl,cfssljson}
```

4. Create some directories:

```
$ mkdir -p registry/{certs,storage}
```

5. Create self-signed certs:

```
$ ./tls-gen.sh
$ cp -p ca/certs/registry-server.* registry/certs
```

6. Start the docker registry

```
$ docker-compose up -d
```

7. Test it out

```
$ docker pull alpine:3.8
$ docker tag alpine:3.8 localhost:5000/alpine:3.8
$ docker push localhost:5000/alpine:3.8
The push refers to repository [localhost:5000/alpine]
df64d3292fd6: Mounted from ...
3.8: digest: sha256:02892826401a9d18f0ea01f8a2f35d328ef039db4e1edcc45c630314a0457d5b size: 528
```
