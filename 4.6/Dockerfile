FROM babim/elastic-stack:base

ENV ELASTIC 2.4.4
ENV LOGSTASH 2.4.1
ENV KIBANA 4.6.4

RUN set -x \
  && cd /tmp \
  && echo "Download Elastic Stack ======================================================" \
  && echo "Download Elasticsearch..." \
  && wget --no-check-certificate -O elasticsearch-$ELASTIC.tar.gz https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/tar/elasticsearch/$ELASTIC/elasticsearch-$ELASTIC.tar.gz \
  && tar -xzf elasticsearch-$ELASTIC.tar.gz \
  && mv elasticsearch-$ELASTIC /usr/share/elasticsearch \
  && echo "Download Logstash..." \
  && wget --no-check-certificate -O logstash-$LOGSTASH.tar.gz \
    https://download.elastic.co/logstash/logstash/logstash-$LOGSTASH.tar.gz \
  && tar -xzf logstash-$LOGSTASH.tar.gz \
  && mv logstash-$LOGSTASH /usr/share/logstash \
  && echo "Download Kibana..." \
  && wget --no-check-certificate -O kibana-$KIBANA.tar.gz https://download.elastic.co/kibana/kibana/kibana-$KIBANA-linux-x86_64.tar.gz \
  && tar -xzf kibana-$KIBANA.tar.gz \
  && mv kibana-$KIBANA-linux-x86_64 /usr/share/kibana \
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
  # the default "server.host" is "localhost" in 5+
  && sed -ri "s!^(\#\s*)?(server\.host:).*!\2 '0.0.0.0'!" /usr/share/kibana/config/kibana.yml \
  && grep -q "^server\.host: '0.0.0.0'\$" /usr/share/kibana/config/kibana.yml \
  # usr alpine nodejs and not bundled version
  && bundled='NODE="${DIR}/node/bin/node"' \
  && apline_node='NODE="/usr/bin/node"' \
  && sed -i "s|$bundled|$apline_node|g" /usr/share/kibana/bin/kibana \
  && rm -rf /usr/share/kibana/node \
  && echo "Make Ngins SSL directory..." \
  && mkdir -p /etc/nginx/ssl \
  && chown -R elstack:elstack /usr/share/elasticsearch \
  && chown -R elstack:elstack /usr/share/logstash \
  && chown -R elstack:elstack /usr/share/kibana \
  && echo "Clean Up..." \
  && rm -rf /tmp/* \
  && apk del --purge wget ca-certificates gnupg openssl
