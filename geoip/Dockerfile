FROM babim/alpinebase

## alpine linux
RUN apk add --no-cache wget bash && cd / && wget --no-check-certificate https://raw.githubusercontent.com/babim/docker-tag-options/master/z%20SCRIPT%20AUTO/option.sh && \
    chmod 755 /option.sh

ENV STACK 6.3.2
ENV STACK_NEW true
ENV ES_DOWNLOAD_URL "https://artifacts.elastic.co/downloads/elasticsearch"
ENV LS_DOWNLOAD_URL "https://artifacts.elastic.co/downloads/logstash"
ENV KB_DOWNLOAD_URL "https://artifacts.elastic.co/downloads/kibana"

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

# Install geoip/user-agent plugin 
RUN elasticsearch-plugin install -b ingest-geoip 
RUN elasticsearch-plugin install -b ingest-user-agent

# necessary for 5.0+ (overriden via "--path.settings", ignored by < 5.0)
ENV LS_SETTINGS_DIR /etc/logstash

VOLUME ["/usr/share/elasticsearch/data"]
VOLUME ["/etc/logstash/conf.d"]
VOLUME ["/etc/nginx"]

EXPOSE 80 443 5601 9200 9300

CMD ["/sbin/tini","--","/usr/bin/supervisord","-c", "/etc/supervisor/supervisord.conf"]
