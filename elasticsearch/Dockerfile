# This Dockerfile avoids the "setup" dependency to create certs here:
# https://www.elastic.co/guide/en/elasticsearch/reference/8.13/docker.html

ARG STACK_VERSION
FROM docker.elastic.co/elasticsearch/elasticsearch:${STACK_VERSION}

USER root

COPY instances.yml config/certs/instances.yml

RUN \
  echo "Creating CA"; \
  bin/elasticsearch-certutil ca --silent --pem -out config/certs/ca.zip; \
  unzip config/certs/ca.zip -d config/certs; \
  echo "Creating certs"; \
  bin/elasticsearch-certutil cert --silent --pem -out config/certs/certs.zip \
      --in config/certs/instances.yml \
      --ca-cert config/certs/ca/ca.crt \
      --ca-key config/certs/ca/ca.key; \
  unzip config/certs/certs.zip -d config/certs; \
  echo "Setting file permissions"; \
  chown -R root:root config/certs; \
  find config/certs -type d | xargs chmod 750; \
  find config/certs -type f | xargs chmod 640;

USER 1000:0

# end
