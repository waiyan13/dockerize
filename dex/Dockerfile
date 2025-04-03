ARG DEX_VERSION=2.41.0
FROM dexidp/dex:v${DEX_VERSION}-alpine

USER root

RUN apk update && \
  apk upgrade && \
  apk add tzdata

# Create non-root user
ARG PUID=1000
ENV PUID=${PUID}
ARG PGID=1000
ENV PGID=${PGID}

# Add dex user
RUN set -x; \
  addgroup -g ${PGID} -S dex ; \
  adduser -u ${PUID} -D -S -G dex dex && exit 0 ; exit 1

ARG TZ=UTC
ENV TZ=${TZ}

RUN cp /usr/share/zoneinfo/${TZ} /etc/localtime && \
  echo ${TZ} > /etc/timezone

USER dex

ARG DEX_PORT

EXPOSE ${DEX_PORT}
