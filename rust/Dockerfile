ARG RUST_VERSION=1.69
FROM rust:${RUST_VERSION}-bullseye

ENV DEBIAN_FRONTEND noninteractive

USER root

ARG TZ=UTC
ENV TZ=${TZ}

RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone
