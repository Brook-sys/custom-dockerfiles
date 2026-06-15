FROM ghcr.io/paperclipai/paperclip:latest

# ==============================================================================
# METADADOS
# ==============================================================================
LABEL org.opencontainers.image.source="https://github.com/Brook-sys/custom-dockerfiles" \
      org.opencontainers.image.title="paperclip-extended" \
      org.opencontainers.image.description="Paperclip extendido com Godot e ferramentas mobile." \
      org.opencontainers.image.licenses="MIT"

# ==============================================================================
# ASSUNTO 1: DEPENDÊNCIAS DO SISTEMA (C++, Media, X11, Java)
# ==============================================================================
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget unzip ca-certificates \
    build-essential gcc g++ make scons cmake \
    xvfb xauth imagemagick ffmpeg \
    openjdk-21-jdk-headless \
    && rm -rf /var/lib/apt/lists/*

# ==============================================================================
# ASSUNTO 2: ANDROID SDK (Obrigatório para build de APK/AAB no Godot)
# ==============================================================================
ENV ANDROID_HOME=/opt/android-sdk
ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV PATH=${PATH}:${ANDROID_HOME}/cmdline-tools/latest/bin:${ANDROID_HOME}/platform-tools

RUN mkdir -p ${ANDROID_HOME}/cmdline-tools && \
    wget -q "https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip" -O android-tools.zip && \
    unzip android-tools.zip -d ${ANDROID_HOME}/cmdline-tools && \
    rm android-tools.zip && \
    # A estrutura exige que os tools fiquem na pasta "latest"
    mv ${ANDROID_HOME}/cmdline-tools/cmdline-tools ${ANDROID_HOME}/cmdline-tools/latest && \
    # Aceitar as licenças automaticamente e instalar pacotes básicos do SDK
    yes | sdkmanager --licenses && \
    sdkmanager "platform-tools" "build-tools;34.0.0" "platforms;android-34"

# ==============================================================================
# ASSUNTO 3: GODOT ENGINE & EXPORT TEMPLATES
# ==============================================================================
ENV GODOT_VERSION=4.3
ENV GODOT_RELEASE=stable

# Baixa e instala o binário da Engine
RUN wget -q "https://github.com/godotengine/godot/releases/download/${GODOT_VERSION}-${GODOT_RELEASE}/Godot_v${GODOT_VERSION}-${GODOT_RELEASE}_linux.x86_64.zip" -O godot.zip && \
    unzip godot.zip && \
    mv Godot_v${GODOT_VERSION}-${GODOT_RELEASE}_linux.x86_64 /usr/local/bin/godot && \
    chmod +x /usr/local/bin/godot && \
    rm godot.zip

# Baixa e instala os Export Templates (Path absoluto é mais seguro no Docker)
RUN mkdir -p /root/.local/share/godot/export_templates/${GODOT_VERSION}.${GODOT_RELEASE} && \
    wget -q "https://github.com/godotengine/godot/releases/download/${GODOT_VERSION}-${GODOT_RELEASE}/Godot_v${GODOT_VERSION}-${GODOT_RELEASE}_export_templates.tpz" -O templates.tpz && \
    unzip templates.tpz -d temp_templates && \
    mv temp_templates/templates/* /root/.local/share/godot/export_templates/${GODOT_VERSION}.${GODOT_RELEASE}/ && \
    rm -rf templates.tpz temp_templates

#Ferramentas adicionais para manipulação e extração de arquivos compactados vindos de itch.io/opengameart e etc.
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        unrar-free \
        7zip \
    && rm -rf /var/lib/apt/lists/*

