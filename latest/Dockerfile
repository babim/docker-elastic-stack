FROM babim/elastic-stack:base

ENV STACK 7.0.0
ENV STACK_NEW true
ENV BIT x86_64
ENV ES_DOWNLOAD_URL "https://artifacts.elastic.co/downloads/elasticsearch"
ENV LS_DOWNLOAD_URL "https://artifacts.elastic.co/downloads/logstash"
ENV KB_DOWNLOAD_URL "https://artifacts.elastic.co/downloads/kibana"
ENV ES_TARBAL "${ES_DOWNLOAD_URL}/elasticsearch-oss-${STACK}-linux-${BIT}.tar.gz"
ENV LS_TARBAL "${LS_DOWNLOAD_URL}/logstash-oss-${STACK}.tar.gz"
ENV KB_TARBAL "${KB_DOWNLOAD_URL}/kibana-oss-${STACK}-linux-${BIT}.tar.gz"
ENV SOFT stack

# install
RUN curl -s https://raw.githubusercontent.com/babim/docker-tag-options/master/z%20ElasticStack%20install/${SOFT}_install.sh | bash

ENV PATH /usr/share/elasticsearch/bin:$PATH
ENV PATH /usr/share/logstash/bin:$PATH
ENV PATH /usr/share/kibana/bin:$PATH
ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk

# fixes mktemp issue in alpine
ENV ES_TMPDIR /usr/share/elasticsearch/tmp

VOLUME ["/usr/share/kibana/app/dashboards"]
VOLUME ["/usr/share/elasticsearch/data"]
VOLUME ["/etc/logstash/conf.d"]
VOLUME ["/etc/nginx"]

EXPOSE 80 443 5601 9200 9300
