ARG VERSION
FROM redmine:${VERSION}

ENV DOCKERIZE_VERSION=v0.6.1 \
    REDMINE_DIR=/usr/src/redmine

RUN set -eux \
    && : "Install dockerize" \
    && wget -O dockerize.tar.gz "https://github.com/jwilder/dockerize/releases/download/${DOCKERIZE_VERSION}/dockerize-linux-amd64-${DOCKERIZE_VERSION}.tar.gz" \
    && tar -xzvf dockerize.tar.gz -C /usr/local/bin \
    && rm dockerize.tar.gz

RUN cd ${REDMINE_DIR}/plugins \
    && git clone -b redmine4-support https://github.com/LaurensRietveld/redmine_mentions.git \
    && rm -r redmine_mentions/.git

COPY config.ru ${REDMINE_DIR}
COPY tmpl.configuration.yml ${REDMINE_DIR}/config
COPY sub-uri.rb ${REDMINE_DIR}/config

RUN cat ${REDMINE_DIR}/config/sub-uri.rb >> ${REDMINE_DIR}/config/environment.rb

ENTRYPOINT dockerize -template ${REDMINE_DIR}/config/tmpl.configuration.yml:${REDMINE_DIR}/config/configuration.yml -wait tcp://${DB_HOST}:${DB_PORT} -timeout 60s /docker-entrypoint.sh passenger start
