ARG GO_VERSION=1.21
FROM golang:${GO_VERSION}-alpine as builder

USER root

RUN apk update && \
  apk upgrade && \
  apk add tzdata

# Create non-root user
ARG PUID=1000
ENV PUID=${PUID}
ARG PGID=1000
ENV PGID=${PGID}

# Add go user
RUN set -x ; \
  addgroup -g ${PGID} -S go ; \
  adduser -u ${PUID} -D -S -G go go && exit 0 ; exit 1

# Create directory for projects
ARG PROJECTS_PATH=/var/www
ENV PROJECTS_PATH=${PROJECTS_PATH}

RUN mkdir -p ${PROJECTS_PATH} && \
  chown -R ${PUID}:${PGID} ${PROJECTS_PATH}

# Setup timezone
ARG TZ=UTC
ENV TZ=${TZ}

RUN cp /usr/share/zoneinfo/${TZ} /etc/localtime && \
  echo ${TZ} > /etc/timezone

USER go

ARG GO_PORT

EXPOSE ${GO_PORT}
