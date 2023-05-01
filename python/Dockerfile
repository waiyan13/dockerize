FROM debian:11-slim AS installer

ENV DEBIAN_FRONTEND noninteractive

USER root

# Python build dependencies
RUN set -xe; \
  apt-get update -yqq && \
  apt-get install -yqq \
    build-essential \
    curl \
    git \
    libbz2-dev \
    libgdbm-dev \
    libffi-dev \
    liblzma-dev \
    libncursesw5-dev \
    libnss3-dev \
    libreadline-dev \
    libsqlite3-dev \
    libssl-dev \
    libxmlsec1-dev \
    libxml2-dev \
    llvm \
    make \
    tk-dev \
    wget \
    xz-utils \
    zlib1g-dev

# Add python user
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

USER python
ENV HOME=/home/python

RUN set -xe; \
  curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash

ARG INSTALL_VERSION=3.11.1
ENV INSTALL_VERSION=${INSTALL_VERSION}

RUN set -xe; \
  ${HOME}/.pyenv/bin/pyenv install ${INSTALL_VERSION}

# Final image
FROM debian:11-slim

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
    curl

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

USER python

ENV HOME=/home/python

RUN mkdir -p ${HOME}/.pyenv

# pyenv
ARG INSTALL_VERSION=3.11.1
ARG REF_VERSION=3.11
ENV INSTALL_VERSION=${INSTALL_VERSION}
ENV REF_VERSION=${REF_VERSION}

COPY --from=installer ${HOME}/.pyenv ${HOME}/.pyenv

RUN echo '\n\
export PYENV_ROOT="$HOME/.pyenv" \n\
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH" \n\
eval "$(pyenv init -)" \n\
export PATH="$PYENV_ROOT/shims:$HOME/.local/bin:$PATH" \n\
' >> ${HOME}/.bashrc

RUN echo '\n\
export PYENV_ROOT="$HOME/.pyenv" \n\
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH" \n\
eval "$(pyenv init -)" \n\
export PATH="$PYENV_ROOT/shims:$HOME/.local/bin:$PATH" \n\
' >> ${HOME}/.profile

# Install pipenv
ARG INSTALL_PIPENV=false
ENV INSTALL_PIPENV=${INSTALL_PIPENV}
ENV PIPENV_VENV_IN_PROJECT=1

RUN set -xe; \
  if [ ${INSTALL_PIPENV} = true ]; then \
    ${HOME}/.pyenv/bin/pyenv global ${INSTALL_VERSION} && \
    ${HOME}/.pyenv/bin/pyenv exec pip install --upgrade --user pip && \
    ${HOME}/.pyenv/bin/pyenv exec pip install --user pipenv \
  ;fi

# Install Potery
ARG INSTALL_POETRY=false
ENV INSTALL_POETRY=${INSTALL_POETRY}

RUN set -xe; \
  if [ ${INSTALL_POETRY} = true ]; then \
    ${HOME}/.pyenv/bin/pyenv global ${INSTALL_VERSION} && \
    ${HOME}/.pyenv/bin/pyenv exec pip install --upgrade --user pip && \
    curl -sSL https://install.python-poetry.org | ${HOME}/.pyenv/shims/python${REF_VERSION} - && \
    ${HOME}/.local/bin/poetry config virtualenvs.in-project true \
  ;fi

WORKDIR /var/www
