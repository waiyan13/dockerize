ARG PYTHON_VERSION=3.12
FROM python:${PYTHON_VERSION}-bookworm

ENV DEBIAN_FRONTEND noninteractive

USER root

# Set timezone
ARG TZ=UTC
ENV TZ ${TZ}
 
RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone
 
# Create non-root user
ARG PUID=1000
ENV PUID=${PUID}
ARG PGID=1000
ENV PGID=${PGID}

RUN set -xe; \
  groupadd --gid ${PGID} python && \
  useradd \
    --uid ${PUID} \
    --gid ${PGID} \
    --no-log-init \
    --create-home \
    python

RUN set -xe; \
  apt-get update -yqq && \
  apt-get install -yqq \
    ca-certificates \
    curl \
    git \
    gnupg \
    libsqlite3-dev \
    lsb-release

# Install Tesseract
RUN set -xe; \
  install -m 0755 -d /etc/apt/keyrings && \
  curl -fsSL https://notesalexp.org/debian/alexp_key.asc -o /etc/apt/keyrings/alexp_key.asc && \
  chmod a+r /etc/apt/keyrings/alexp_key.asc && \
  echo "deb [signed-by=/etc/apt/keyrings/alexp_key.asc] https://notesalexp.org/tesseract-ocr5/$(lsb_release -cs)/ \
  $(lsb_release -cs) main" | \
  tee /etc/apt/sources.list.d/notesalexp.list > /dev/null && \
  apt-get update -yqq && \
  apt-get install -yqq tesseract-ocr

# Create directory for projects
ARG PROJECTS_PATH=/var/www
ENV PROJECTS_PATH=${PROJECTS_PATH}

RUN mkdir -p ${PROJECTS_PATH} && \
  chown -R ${PUID}:${PGID} ${PROJECTS_PATH}

# Set system environment variables
ENV LANG=C.UTF-8 \
  LC_ALL=C.UTF-8 \
  PYTHONDONTWRITEBYTECODE=1 \
  PYTHONFAULTHANDLER=1
ENV PATH="/home/python/.local/bin/:$PATH"

USER python

ARG UV_VERSION=0.6.5
ENV UV_VERSION=${UV_VERSION}
ENV HOME=/home/python

WORKDIR /home/python

# Install uv
RUN set -xe; \
  curl -LsSf https://astral.sh/uv/${UV_VERSION}/install.sh | sh

WORKDIR /var/www
