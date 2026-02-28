FROM docker.io/sipeed/picoclaw:latest

USER root

RUN apk add --no-cache \
    jq \
    git 

# Criar o usuário caso não exista na imagem base
RUN addgroup -S picoclaw 2>/dev/null || true && \
    adduser -S -G picoclaw -u 1000 picoclaw 2>/dev/null || true

USER picoclaw
