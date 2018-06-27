B(eat)-E(leasticsearch)-K(ibana) Docker Swarm Stack
===

Info
---

The Docker Stack file `stack.yml` can be used to deploy dynamic logging stack for Docker Swarm.
This stack deploys the following services:

* `Filebeat`: this service deployed to all hosts (Docker Swarm service global mode) to collect the json logs from docker container and feed it to eleasticsearch directly.
* `Elasticsearch`: Database to store the log data and query for it.
* `Kibana`: Web UI to display Elasticsearch logging data.


Instructions
---

1) Setup an Docker Swarm, label an swarm node for elasticsearch usage and
modify kernel settings, see [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/docker.html)

```
echo "vm.max_map_count=262144" > /etc/sysctl.d/99-docker-elasticsearch.conf
sysctl -p /etc/sysctl.d/99-docker-elasticsearch.conf
```

Initialize a docker swarm and add some nodes to cluster, see [Docker Swarm Docs](https://docs.docker.com/engine/swarm/swarm-tutorial/create-swarm/)

```
docker swarm init
```

Add Node label for elasticsearch

```
docker node update --label-add elasticsearch=es1 <node|node-uuid>
```

2) Build your customized filebeat docker image, filebeat version 6.3.0 is used.

```
cd filebeat-customized
docker build -t filebeat:6.3.0-custom .

```
Deploy the image to your docker swarm infrastructure via a (private) docker registry, like Docker Index aka [Docker Hub](https://hub.docker.com/)

3) Run the stack

```
docker stack deploy -c stack.yml bek
```

4) Use the Kibana UI for browsing through the logfiles

Open your favourite webbrowser and open the URI: http://<hostname-or-ip-of-docker-swarm-node>/

In Kibana, create a new index `filebeat-*` with `@timestamp` as the Time-field name.

:) Have fun.

This setup is tested with Ubuntu 16.04 LTS, Docker 17.09-CE and based on work [Hanzel Jesheen](https://github.com/botleg/swarm-monitoring)
