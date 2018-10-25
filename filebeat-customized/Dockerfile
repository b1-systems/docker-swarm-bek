# a customized filebeat docker image based on offical filebeat
FROM docker.elastic.co/beats/filebeat:6.4.2

# we start filebeat under root, because we need the permission to
# read the information from the docker container path, mounted inside it
USER root

# modified filebeat config
COPY filebeat.yml /usr/share/filebeat/filebeat.yml
