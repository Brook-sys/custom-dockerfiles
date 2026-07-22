# syntax=docker/dockerfile:1.7

ARG OPENCLAW_IMAGE=ghcr.io/openclaw/openclaw:latest
ARG GO_IMAGE=golang:1.26-bookworm


# Compila somente as ferramentas adicionais.
FROM ${GO_IMAGE} AS go-tools

ARG GOPLS_VERSION=v0.23.0
ARG GOIMPORTS_VERSION=v0.48.0

RUN mkdir -p /out && \
    GOBIN=/out go install \
        golang.org/x/tools/gopls@${GOPLS_VERSION} && \
    GOBIN=/out go install \
        golang.org/x/tools/cmd/goimports@${GOIMPORTS_VERSION}


# Imagem final baseada no OpenClaw.
FROM ${OPENCLAW_IMAGE}

USER root

# Copia o compilador e toda a toolchain oficial do Go.
COPY --from=go-tools /usr/local/go /usr/local/go

# Copia somente as ferramentas adicionais compiladas.
COPY --from=go-tools /out/gopls /usr/local/bin/gopls
COPY --from=go-tools /out/goimports /usr/local/bin/goimports

# Necessário para projetos Go que utilizam CGO.
# Para projetos 100% Go puro, este bloco pode ser removido.
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
        --no-install-recommends \
        build-essential && \
    rm -rf /var/lib/apt/lists/*

# Diretórios graváveis pelo usuário node.
RUN mkdir -p \
        /home/node/go/bin \
        /home/node/go/pkg/mod \
        /home/node/.cache/go-build && \
    chown -R node:node \
        /home/node/go \
        /home/node/.cache

ENV HOME=/home/node \
    GOROOT=/usr/local/go \
    GOPATH=/home/node/go \
    GOBIN=/home/node/go/bin \
    GOMODCACHE=/home/node/go/pkg/mod \
    GOCACHE=/home/node/.cache/go-build \
    PATH=/usr/local/go/bin:/home/node/go/bin:${PATH}

# Falha durante o build caso algo esteja incorreto.
RUN go version && \
    gopls version && \
    command -v goimports && \
    git --version

USER node
