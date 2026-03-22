FROM docker.io/sipeed/picoclaw:latest

# Labels para GHCR e metadados
LABEL org.opencontainers.image.source="https://github.com/Brook-sys/custom-dockerfiles" \
      org.opencontainers.image.title="picoclaw-extended" \
      org.opencontainers.image.description="Picoclaw estendido com pacotes adicionais para homelab" \
      org.opencontainers.image.licenses="MIT"

RUN apk add --no-cache \
    jq \
    git \
    curl \
    openssh-client \
    uv

ENV TZ=America/Sao_Paulo


