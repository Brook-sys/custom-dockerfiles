# syntax=docker/dockerfile:1.7

ARG OPENCLAW_IMAGE=ghcr.io/openclaw/openclaw:2026.7.1
ARG GO_IMAGE=golang:1.26-bookworm


# ------------------------------------------------------------
# Compilação das ferramentas Go
# ------------------------------------------------------------

FROM ${GO_IMAGE} AS go-tools

ARG GOPLS_VERSION=latest
ARG GOIMPORTS_VERSION=latest
ARG STATICCHECK_VERSION=latest

RUN mkdir -p /out && \
    GOBIN=/out go install \
        golang.org/x/tools/gopls@${GOPLS_VERSION} && \
    GOBIN=/out go install \
        golang.org/x/tools/cmd/goimports@${GOIMPORTS_VERSION} && \
    GOBIN=/out go install \
        honnef.co/go/tools/cmd/staticcheck@${STATICCHECK_VERSION}


# ------------------------------------------------------------
# OpenClaw com ferramentas Go
# ------------------------------------------------------------

FROM ${OPENCLAW_IMAGE}

USER root

COPY --from=go-tools /usr/local/go /usr/local/go
COPY --from=go-tools /out/gopls /usr/local/bin/gopls
COPY --from=go-tools /out/goimports /usr/local/bin/goimports
COPY --from=go-tools /out/staticcheck /usr/local/bin/staticcheck

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
        --no-install-recommends \
        build-essential && \
    rm -rf /var/lib/apt/lists/*

ENV HOME=/home/node \
    GOROOT=/usr/local/go \
    GOPATH=/home/node/go \
    GOBIN=/home/node/go/bin \
    GOMODCACHE=/home/node/go/pkg/mod \
    GOCACHE=/home/node/.cache/go-build \
    XDG_CACHE_HOME=/home/node/.cache \
    PATH=/usr/local/go/bin:/home/node/go/bin:${PATH}

RUN mkdir -p \
        /home/node/go/bin \
        /home/node/go/pkg/mod \
        /home/node/.cache/go-build \
        /home/node/.cache/gopls \
        /home/node/.cache/staticcheck && \
    chown -R node:node \
        /home/node/go \
        /home/node/.cache && \
    chmod -R u+rwX \
        /home/node/go \
        /home/node/.cache

# É importante trocar para node ANTES de executar gopls.
# Assim nenhum cache é criado como root.
USER node

RUN go version && \
    gopls version && \
    goimports -help >/dev/null 2>&1 || true && \
    staticcheck -version && \
    test -w /home/node/.cache/gopls

WORKDIR /app
