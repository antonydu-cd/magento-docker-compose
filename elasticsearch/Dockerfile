FROM docker.elastic.co/elasticsearch/elasticsearch:7.6.2

RUN echo "xpack.security.enabled: false" >> /usr/share/elasticsearch/config/elasticsearch.yml
RUN echo "discovery.type: single-node" >> /usr/share/elasticsearch/config/elasticsearch.yml
RUN bin/elasticsearch-plugin install -b analysis-icu && \
    bin/elasticsearch-plugin install -b analysis-phonetic

EXPOSE 9200 9300