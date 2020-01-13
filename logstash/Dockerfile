ARG ELK_VERSION=7.5.1

# https://github.com/elastic/logstash-docker
# FROM docker.elastic.co/logstash/logstash:${ELK_VERSION}
FROM logstash:${ELK_VERSION}

# Add your logstash plugins setup here
# Example: RUN logstash-plugin install logstash-filter-json

RUN logstash-plugin install logstash-output-zabbix
