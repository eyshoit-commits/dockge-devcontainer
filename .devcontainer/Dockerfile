FROM mcr.microsoft.com/devcontainers/base:ubuntu

# Install Docker and other dependencies
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
        curl \
        wget \
        unzip \
        ca-certificates \
    && curl -fsSL https://get.docker.com -o get-docker.sh \
    && sh get-docker.sh \
    && rm get-docker.sh

# Install Dockge directly
RUN wget https://github.com/louislam/dockge/releases/download/1.4.0/dockge_1.4.0_linux_amd64.zip \
    && unzip dockge_1.4.0_linux_amd64.zip -d /opt/dockge \
    && rm dockge_1.4.0_linux_amd64.zip \
    && chmod +x /opt/dockge/dockge \
    && mkdir -p /opt/stacks /app/data

# Create startup script
RUN echo '#!/bin/bash\n\
# Start Docker daemon\n\
sudo dockerd --iptables=false --bridge=none &\n\
sleep 5\n\
# Start Dockge\n\
/opt/dockge/dockge --host 0.0.0.0 --port 5001 --data-dir /app/data --stacks-dir /opt/stacks' > /usr/local/bin/start-services.sh \
    && chmod +x /usr/local/bin/start-services.sh
