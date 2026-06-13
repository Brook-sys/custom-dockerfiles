FROM ghcr.io/paperclipai/paperclip:latest

# Labels para GHCR e metadados
LABEL org.opencontainers.image.source="https://github.com/Brook-sys/custom-dockerfiles" \
      org.opencontainers.image.title="paperclip-extended" \
      org.opencontainers.image.description="Paperclip extendido com mais recursos uteis." \
      org.opencontainers.image.licenses="MIT"


# 1. Dependências do sistema (build, media e android base)
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget unzip ca-certificates \
    build-essential gcc g++ make scons \
    xvfb imagemagick ffmpeg \
    openjdk-17-jdk-headless \
    && rm -rf /var/lib/apt/lists/*

# 2. Variáveis para versão do Godot
ENV GODOT_VERSION=4.3
ENV GODOT_RELEASE=stable

# 3. Baixar e instalar Godot Engine (Headless/Linux64)
RUN wget -q "https://github.com/godotengine/godot/releases/download/${GODOT_VERSION}-${GODOT_RELEASE}/Godot_v${GODOT_VERSION}-${GODOT_RELEASE}_linux.x86_64.zip" -O godot.zip && \
    unzip godot.zip && \
    mv Godot_v${GODOT_VERSION}-${GODOT_RELEASE}_linux.x86_64 /usr/local/bin/godot && \
    chmod +x /usr/local/bin/godot && \
    rm godot.zip

# 4. Baixar e instalar Godot Export Templates (necessário para exportar os jogos)
RUN mkdir -p ~/.local/share/godot/export_templates/${GODOT_VERSION}.${GODOT_RELEASE} && \
    wget -q "https://github.com/godotengine/godot/releases/download/${GODOT_VERSION}-${GODOT_RELEASE}/Godot_v${GODOT_VERSION}-${GODOT_RELEASE}_export_templates.tpz" -O templates.tpz && \
    unzip templates.tpz -d temp_templates && \
    mv temp_templates/templates/* ~/.local/share/godot/export_templates/${GODOT_VERSION}.${GODOT_RELEASE}/ && \
    rm -rf templates.tpz temp_templates

# (Opcional) 5. Android SDK
# Se precisar exportar APKs diretamente sem depender só do template genérico, 
# você precisará do Android SDK Command Line Tools (cmdline-tools).
