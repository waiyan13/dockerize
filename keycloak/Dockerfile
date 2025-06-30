ARG KEYCLOAK_VERSION=26.2.5
FROM quay.io/keycloak/keycloak:${KEYCLOAK_VERSION} AS builder

# Enable health and metrics support
ENV KC_HEALTH_ENABLED=true
ENV KC_METRICS_ENABLED=true

# Configure a database vendor
ARG KEYCLOAK_DATABASE
ENV KC_DB=${KEYCLOAK_DATABASE}

WORKDIR /opt/keycloak
RUN set -xe; \
  /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:${KEYCLOAK_VERSION}

COPY --from=builder /opt/keycloak /opt/keycloak

ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start-dev"]
