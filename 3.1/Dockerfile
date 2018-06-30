FROM babim/elastic-stack:base

# ENV ELASTIC 1.7.6
# ENV LOGSTASH 1.5.6
ENV ELASTIC 2.4.2
ENV LOGSTASH 2.4.1
ENV KIBANA 3.1.3

RUN set -x \
  && cd /tmp \
  && echo "Download Elastic Stack ======================================================" \
  && echo "Download Elasticsearch..." \
  && wget -q --no-check-certificate -O elasticsearch-$ELASTIC.tar.gz https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/tar/elasticsearch/$ELASTIC/elasticsearch-$ELASTIC.tar.gz \
  && tar -xzf elasticsearch-$ELASTIC.tar.gz \
  && mv elasticsearch-$ELASTIC /usr/share/elasticsearch \
  && echo "Download Logstash..." \
  && wget -q --no-check-certificate -O logstash-$LOGSTASH.tar.gz \
    https://download.elastic.co/logstash/logstash/logstash-$LOGSTASH.tar.gz \
  && tar -xzf logstash-$LOGSTASH.tar.gz \
  && mv logstash-$LOGSTASH /usr/share/logstash \
  && echo "Download Kibana..." \
  && wget -q --no-check-certificate -O kibana-$KIBANA.tar.gz https://download.elastic.co/kibana/kibana/kibana-$KIBANA.tar.gz \
  && tar -xzf kibana-$KIBANA.tar.gz \
  && mv kibana-$KIBANA /usr/share/kibana \
  && echo "Configure [Elasticsearch] ===================================================" \
  && for path in \
  	/usr/share/elasticsearch/data \
  	/usr/share/elasticsearch/logs \
  	/usr/share/elasticsearch/config \
  	/usr/share/elasticsearch/config/scripts \
  	/usr/share/elasticsearch/plugins \
  ; do \
  mkdir -p "$path"; \
  done \
  && echo "Configure [Logstash] ========================================================" \
  && if [ -f "$LS_SETTINGS_DIR/logstash.yml" ]; then \
  		sed -ri 's!^(path.log|path.config):!#&!g' "$LS_SETTINGS_DIR/logstash.yml"; \
  	fi \
  && echo "Configure [Kibana] ==========================================================" \
  && mkdir -p /var/www \
  && ln -s /usr/share/kibana /var/www/kibana \
  && ls -lah /var/www/kibana \
  && ls -lah /usr/share/kibana \
  && sed -i 's/9200"/"+ window.location.port/g' /var/www/kibana/config.js \
  && chown -R elstack:elstack /usr/share/elasticsearch \
  && chown -R elstack:elstack /usr/share/logstash \
  && chown -R elstack:elstack /usr/share/kibana \
  && echo "Clean Up..." \
  && rm -rf /tmp/* \
  && apk del --purge .build-deps ca-certificates tar wget

# Add custom supervisor config
COPY config/supervisord/supervisord.conf /etc/supervisor/

VOLUME ["/usr/share/kibana/app/dashboards"]
