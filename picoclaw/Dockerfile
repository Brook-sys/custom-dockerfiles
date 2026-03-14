FROM docker.io/sipeed/picoclaw:latest

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
