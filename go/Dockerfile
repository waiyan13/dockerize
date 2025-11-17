ARG GO_VERSION=1.23
FROM golang:${GO_VERSION}-bookworm

USER root

RUN apt-get update && \
  apt-get upgrade -yqq && \
  apt-get install -yqq git

# Create non-root user
ARG PUID=1000
ENV PUID=${PUID}
ARG PGID=1000
ENV PGID=${PGID}

# Add go user
RUN set -xe; \
  groupadd --gid ${PGID} go && \
  useradd \
    --uid ${PUID} \
    --gid ${PGID} \
    --no-log-init \
    --create-home \
    go

# Create directory for projects
ARG PROJECTS_PATH=/var/www
ENV PROJECTS_PATH=${PROJECTS_PATH}

RUN mkdir -p ${PROJECTS_PATH} && \
  chown -R ${PUID}:${PGID} ${PROJECTS_PATH}

# Setup timezone
ARG TZ=UTC
ENV TZ=${TZ}

RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone

COPY --from=ca_certs ./rootCA.pem /usr/local/share/ca-certificates/rootCA.crt

RUN set -xe; \
  update-ca-certificates

USER go

ARG GO_PORT

EXPOSE ${GO_PORT}
