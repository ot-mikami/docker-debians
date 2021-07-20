FROM debian:buster-slim

SHELL [ "/bin/bash", "-l", "-c" ]

RUN set -x && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        wget \
        curl \
        ca-certificates \
        git \
        zip \
        unzip \
        vim && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/*

# install nvm
ARG NVM_DIR="/.nvm"
ENV NVM_DIR="${NVM_DIR}"
RUN mkdir -p "${NVM_DIR}" && \
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash

ARG NODE_VERSION="lts/*"
ENV NODE_VERSION="${NODE_VERSION}"
RUN nvm install "${NODE_VERSION}" && \
    nvm use "${NODE_VERSION}"

WORKDIR /workspace
