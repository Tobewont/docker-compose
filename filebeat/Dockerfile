ARG ELK_VERSION=7.5.1

# https://github.com/elastic/beats-docker
# FROM docker.elastic.co/beats/filebeat:${ELK_VERSION}
FROM elastic/filebeat:${ELK_VERSION}
COPY config/filebeat.yml /usr/share/filebeat/filebeat.yml
