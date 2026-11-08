FROM node:22-slim

# Installation des dépendances système et outils multimédias
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    curl \
    ca-certificates \
    xz-utils \
    graphicsmagick \
    imagemagick \
    webp \
    && rm -rf /var/lib/apt/lists/*

# Téléchargement et installation propre de FFmpeg statique
RUN curl -L https://johnvansickle.com/ffmpeg/releases/ffmpeg-release-amd64-static.tar.xz -o ffmpeg.tar.xz \
    && mkdir -p /tmp/ffmpeg \
    && tar -xf ffmpeg.tar.xz -C /tmp/ffmpeg --strip-components=1 \
    && cp /tmp/ffmpeg/ffmpeg /usr/local/bin/ \
    && cp /tmp/ffmpeg/ffprobe /usr/local/bin/ \
    && chmod +x /usr/local/bin/ffmpeg /usr/local/bin/ffprobe \
    && rm -rf ffmpeg.tar.xz /tmp/ffmpeg

# Clonage du projet OVL-MD-V2
RUN git clone https://github.com/Ainz-devs/OVL-MD-V2.git /ovl_bot

WORKDIR /ovl_bot

RUN npm install

EXPOSE 8000

CMD ["npm", "run", "Ovl"]
