![el-stack-logo](https://raw.githubusercontent.com/blacktop/docker-elastic-stack/master/docs/img/el_stack_logo.png)

# Elastic Stack Dockerfile
### Thanks blacktop

This repository contains a **Dockerfile** of the [Elastic Stack](https://www.elastic.co/products).

## Dependencies

- [alpine:3.7](https://hub.docker.com/_/alpine/)
- [openjdk8-jre](https://pkgs.alpinelinux.org/package/v3.4/community/x86_64/openjdk8-jre)
- [Elasticsearch](https://www.elastic.co/products/elasticsearch)
- [Logstash](https://www.elastic.co/products/logstash)
- [Kibana](https://www.elastic.co/products/kibana)


> **NOTE:** tag **geoip** is the same as tag **latest**, but includes the _ingest-geoip_ and the _ingest-user-agent_ plugins.

## Getting Started

```bash
$ docker run -d --name elstack -p 80:80 -p 9200:9200 babim/elastic-stack
```

### Now Navigate To

- With [Docker for Mac](https://docs.docker.com/engine/installation/mac/) : `http://localhost`
- With [docker-machine](https://docs.docker.com/machine/) : `http://$(docker-machine ip)`
- With [docker-engine](https://docker.github.io/engine/installation/) : `$(docker inspect -f '{{ .NetworkSettings.IPAddress }}' elstack)`

## Documentation

- [Add some demo data](docs/add-data.md)
- [Enable SSL](docs/ssl.md)
- [Change nginx password](docs/change-pass.md)
- [Build a multi-node Elastic Stack cluster](docs/mutil-node.md)

## Known Issues :warning:

I have noticed when running the new **5.0** version on a **linux** host you need to increase the memory map areas with the following command

```bash
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
sudo sysctl -w vm.max_map_count=262144
```

## Credits

Heavily (if not entirely) influenced by all the elastic official docker images

## Todo

- [x] Install/Run Elastic Stack
- [x] Start Daemon and watch folder with supervisord
- [x] Expose Logstash config folder as well as Nginx sites folder as Volumes
- [x] Build ES test data docker image
- [x] Add Nginx entrypoint to pass USER/PASS in as env vars
- [x] Add SSL (auto-create certs if not found)
- [x] Add back a 3.0 version of the stack (elk stack)
- [ ] Integrate with Bro-IDS
