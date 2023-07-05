ARG POSTGRES_VERSION=15
FROM postgres:${POSTGRES_VERSION}-alpine

RUN apk update && \
  apk upgrade && \
  apk add --no-cache \
    bash

# Setup timezone
ARG TZ=UTC
ENV TZ=${TZ}

RUN cp /usr/share/zoneinfo/${TZ} /etc/localtime && \
  echo ${TZ} > /etc/timezone
