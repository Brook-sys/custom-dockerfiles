FROM docker.io/sipeed/picoclaw:launcher

# Labels para GHCR e metadados
LABEL org.opencontainers.image.source="https://github.com/Brook-sys/custom-dockerfiles" \
      org.opencontainers.image.title="picoclaw-extended" \
      org.opencontainers.image.description="Picoclaw estendido com pacotes adicionais para homelab" \
      org.opencontainers.image.licenses="MIT"

ENV GOPATH=/root/go
ENV PATH="${PATH}:/root/go/bin"

RUN apk add --no-cache \
    ca-certificates \
    jq \
    git \
    curl \
    openssh-client \
    uv \
    tar \
    go

#Instalando engram
RUN curl -L \
    -o /tmp/engram.tar.gz \
    https://github.com/Gentleman-Programming/engram/releases/download/v1.16.1/engram_1.16.1_linux_amd64.tar.gz \
    && tar -xzf /tmp/engram.tar.gz -C /tmp \
    && chmod +x /tmp/engram \
    && mv /tmp/engram /usr/local/bin/engram \
    && rm -f /tmp/engram.tar.gz



