FROM babim/elastic-stack:base

ENV ES_VERSION 2.4.4
ENV LS_VERSION 2.4.1
ENV KB_VERSION 4.6.4
ENV STACK_NEW false
ENV ES_DOWNLOAD_URL "https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/tar/elasticsearch/$ES_VERSION"
ENV LS_DOWNLOAD_URL "https://download.elastic.co/logstash/logstash"
ENV KB_DOWNLOAD_URL "https://download.elastic.co/kibana/kibana"

# install
RUN wget -O - https://raw.githubusercontent.com/babim/docker-tag-options/master/z%20ElasticStack%20install/stack_install.sh | bash
# remove packages
RUN wget -O - https://raw.githubusercontent.com/babim/docker-tag-options/master/z%20ElasticStack%20install/stack_clean.sh | bash

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

CMD ["/sbin/tini","--","/usr/bin/supervisord","-c", "/etc/supervisor/supervisord.conf"]