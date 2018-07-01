FROM babim/elastic-stack:base

ENV STACK 5.2.10

RUN set -x \
  && cd /tmp \
  && echo "Download Elastic Stack ======================================================" \
  && echo "Download Elasticsearch..." \
  && wget --no-check-certificate -O elasticsearch-$STACK.tar.gz https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-$STACK.tar.gz \
  && tar -xzf elasticsearch-$STACK.tar.gz \
  && mv elasticsearch-$STACK /usr/share/elasticsearch \
  && echo "Download Logstash..." \
  && wget --no-check-certificate -O logstash-$STACK.tar.gz \
    https://artifacts.elastic.co/downloads/logstash/logstash-$STACK.tar.gz \
  && tar -xzf logstash-$STACK.tar.gz \
  && mv logstash-$STACK /usr/share/logstash \
  && echo "Download Kibana..." \
  && wget --no-check-certificate -O kibana-$STACK.tar.gz https://artifacts.elastic.co/downloads/kibana/kibana-$STACK-linux-x86_64.tar.gz \
  && tar -xzf kibana-$STACK.tar.gz \
  && mv kibana-$STACK-linux-x86_64 /usr/share/kibana \
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
  && sed -i "s|$bundled|$apline_node|g" /usr/share/kibana/bin/kibana-plugin \
  && sed -i "s|$bundled|$apline_node|g" /usr/share/kibana/bin/kibana \
  && rm -rf /usr/share/kibana/node \
  && echo "Make Ngins SSL directory..." \
  && mkdir -p /etc/nginx/ssl \
  && chown -R elstack:elstack /usr/share/elasticsearch \
  && chown -R elstack:elstack /usr/share/logstash \
  && chown -R elstack:elstack /usr/share/kibana \
  && echo "Clean Up..." \
  && rm -rf /tmp/* \
  && apk del .build-deps wget
