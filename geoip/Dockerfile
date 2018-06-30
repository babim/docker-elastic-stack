FROM babim/elastic-stack:latest

# Install geoip/user-agent plugin
RUN elasticsearch-plugin install -b ingest-geoip
RUN elasticsearch-plugin install -b ingest-user-agent
