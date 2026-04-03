ARG MARIADB_VERSION=11.8.6
FROM mariadb:${MARIADB_VERSION}-ubi

ENV DEBIAN_FRONTEND=noninteractive

USER root

RUN set -xe; \
  microdnf update -y && \
  microdnf clean all

# Add timezone
ARG TZ=UTC
ENV TZ=${TZ}

RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone
