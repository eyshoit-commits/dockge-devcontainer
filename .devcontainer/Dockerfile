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

# Install Dockge - use the Docker image to extract the binary
RUN docker pull louislam/dockge:1 \
    && docker create --name extract louislam/dockge:1 \
    && docker cp extract:/app /opt/dockge \
    && rm -rf /opt/dockge/data \
    && mv /opt/dockge/app/* /opt/dockge/ \
    && rmdir /opt/dockge/app \
    && chmod +x /opt/dockge/dockge \
    && docker rm extract \
    && mkdir -p /opt/stacks /app/data

# Create startup script
RUN echo '#!/bin/bash\n\
# Start Docker daemon\n\
sudo dockerd --iptables=false --bridge=none &\n\
sleep 5\n\
# Start Dockge\n\
/opt/dockge/dockge --host 0.0.0.0 --port 5001 --data-dir /app/data --stacks-dir /opt/stacks' > /usr/local/bin/start-services.sh \
    && chmod +x /usr/local/bin/start-services.sh
