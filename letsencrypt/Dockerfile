FROM node:latest AS code-server-builder
WORKDIR /opt

# Install code-server build dependencies
RUN set -eux; \
    apt-get update; \
    apt-get install -y \
        build-essential \
        pkg-config \
        libx11-dev \
        libxkbfile-dev \
        libsecret-1-dev

# Install (and build) code-server
RUN yarn add code-server

#
# Actual runtime environment definition
#
FROM ubuntu:latest
WORKDIR /opt

# Install development dependencies
RUN set -eux; \
    apt-get update; \
    apt-get install -y \
    apt-transport-https \
    git \
    curl \
    gnupg \
    nano;

# Install node.js
RUN set -eux; \
    curl -sL https://deb.nodesource.com/setup_14.x | bash -; \
    apt-get update; \
    apt-get -y install nodejs

# Copy the actually built code-server
COPY --from=code-server-builder /opt/node_modules /opt/node_modules

# Add the home directory of our user as a volume
VOLUME /user-home

# Create the "developer" user in the container
RUN groupadd --gid 5000 developer && \
    useradd --home-dir /user-home \
        --uid 5000 \
        --gid 5000 \
        --shell /bin/bash \
        developer

# Switch to the new user account
USER developer

# Start code-server with our config file
CMD [ "/opt/node_modules/.bin/code-server", "--config", "/code-server.yaml" ]
