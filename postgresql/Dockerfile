ARG POSTGRESQL_VERSION=14
FROM bitnami/postgresql:${POSTGRESQL_VERSION}-debian-11

ENV DEBIAN_FRONTEND noninteractive

USER root

ARG TZ=UTC
ENV TZ=${TZ}

RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone

