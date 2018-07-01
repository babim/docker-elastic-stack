FROM babim/alpinebase

## alpine linux
RUN apk add --no-cache wget bash && cd / && wget --no-check-certificate https://raw.githubusercontent.com/babim/docker-tag-options/master/z%20SCRIPT%20AUTO/option.sh && \
    chmod 755 /option.sh

RUN apk add --no-cache openjdk8-jre tini su-exec

RUN apk add --no-cache libzmq bash nodejs supervisor nginx apache2-utils openssl
RUN mkdir -p /usr/local/lib \
	&& ln -s /usr/lib/*/libzmq.so.3 /usr/local/lib/libzmq.so
RUN apk add --no-cache wget ca-certificates gnupg \
	&& echo "Create elstack user..." \
	&& adduser -DH -s /sbin/nologin elstack

ENV PATH /usr/share/elasticsearch/bin:$PATH
ENV PATH /usr/share/logstash/bin:$PATH
ENV PATH /usr/share/kibana/bin:$PATH
ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk

# Add custom elasticsearch config
COPY config/elastic /usr/share/elasticsearch/config
COPY config/elastic/logrotate /etc/logrotate.d/elasticsearch

# Add custom logstash config
COPY config/logstash/conf.d/ /etc/logstash/conf.d/
COPY config/logstash/patterns/ /opt/logstash/patterns/
COPY config/logstash/logstash.yml /etc/logstash/

# necessary for 5.0+ (overriden via "--path.settings", ignored by < 5.0)
ENV LS_SETTINGS_DIR /etc/logstash

# fixes mktemp issue in alpine
ENV ES_TMPDIR /usr/share/elasticsearch/tmp

# Add custom nginx config
COPY config/nginx/nginx.conf /etc/nginx/nginx.conf
COPY config/nginx/kibana.conf /etc/nginx/conf.d/
COPY config/nginx/ssl.kibana.conf /etc/nginx/conf.d/

# Add custom supervisor config
COPY config/supervisord/supervisord.conf /etc/supervisor/

# Add entrypoints
COPY entrypoints/elastic-entrypoint.sh /
COPY entrypoints/logstash-entrypoint.sh /
COPY entrypoints/kibana-entrypoint.sh /
COPY entrypoints/nginx-entrypoint.sh /

VOLUME ["/usr/share/elasticsearch/data"]
VOLUME ["/etc/logstash/conf.d"]
VOLUME ["/etc/nginx"]

EXPOSE 80 443 5601 9200 9300

CMD ["/sbin/tini","--","/usr/bin/supervisord","-c", "/etc/supervisor/supervisord.conf"]
