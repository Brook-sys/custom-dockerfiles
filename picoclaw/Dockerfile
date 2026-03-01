FROM docker.io/sipeed/picoclaw:latest

USER root

RUN apk add --no-cache \
    jq \
    git \
    curl \
    openssh-client

# Criar o usuário caso não exista na imagem base
RUN addgroup -g 1000 picoclaw || true && \
    adduser -D -u 1000 -G picoclaw picoclaw || true

USER picoclaw
