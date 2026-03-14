FROM docker.io/sipeed/picoclaw:latest

# Labels para GHCR e metadados
LABEL org.opencontainers.image.source="https://github.com/Brook-sys/picoclaw:latest" \
      org.opencontainers.image.title="picoclaw-extended" \
      org.opencontainers.image.description="Picoclaw estendido com pacotes adicionais para homelab" \
      org.opencontainers.image.licenses="MIT"

USER root

RUN apk add --no-cache \
    jq \
    git \
    curl \
    openssh-client

# Garante que o usuário picoclaw existe e tem UID 1000
RUN id picoclaw >/dev/null 2>&1 || adduser -D -u 1000 -G picoclaw picoclaw && \
    test "$(id -u picoclaw)" -eq 1000

USER picoclaw
