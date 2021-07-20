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

# install anyenv
ARG ANYENV_DIR='/.anyenv'
ENV ANYENV_DIR="${ANYENV_DIR}" \
    PATH="${PATH}:${ANYENV_DIR}/bin"
RUN mkdir -p "${ANYENV_DIR}" && \
    git clone https://github.com/anyenv/anyenv "${ANYENV_DIR}" && \
    ${ANYENV_DIR}/bin/anyenv install --force-init && \
    echo 'eval "$(anyenv init -)"' >> ~/.bashrc && \
    source ~/.bashrc && \
    # install nodenv
    ${ANYENV_DIR}/bin/anyenv install nodenv

WORKDIR /workspace
