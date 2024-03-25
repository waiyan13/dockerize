ARG RUST_VERSION=1.76
FROM rust:${RUST_VERSION}-bullseye

ENV DEBIAN_FRONTEND noninteractive

USER root

RUN apt-get update && \
  apt-get upgrade -yqq

ARG PUID=1000
ENV PUID=${PUID}
ARG PGID=1000
ENV PGID=${PGID}

# Add node user
RUN set -xe; \
  groupadd --gid ${PGID} rust && \
  useradd \
    --uid ${PUID} \
    --gid ${PGID} \
    --no-log-init \
    --create-home \
    rust

# Add timezone
ARG TZ=UTC
ENV TZ=${TZ}

RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone

# Create directory for projects
ARG PROJECTS_PATH=/var/www
ENV PROJECTS_PATH=${PROJECTS_PATH}

RUN mkdir -p ${PROJECTS_PATH} && \
  chown -R ${PUID}:${PGID} ${PROJECTS_PATH}

USER rust

ARG RUST_PORT

EXPOSE ${RUST_PORT}
