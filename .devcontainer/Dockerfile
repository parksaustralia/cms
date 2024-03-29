# Update the VARIANT arg in docker-compose.yml to pick a PHP version: 7, 7.4, 7.3
ARG VARIANT=7.4
FROM mcr.microsoft.com/vscode/devcontainers/php:dev-${VARIANT}

# Install MariaDB client
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
  && apt-get install -y mariadb-client \
  && apt-get clean -y && rm -rf /var/lib/apt/lists/*

# Update args in docker-compose.yaml to set the UID/GID of the "vscode" user.
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN if [ "$USER_GID" != "1000" ] || [ "$USER_UID" != "1000" ]; then \
  groupmod --gid $USER_GID vscode \
  && usermod --uid $USER_UID --gid $USER_GID vscode \
  && chown -R $USER_UID:$USER_GID /home/vscode \
  && chown -R $USER_UID:root /usr/local/share/nvm; \
  fi

# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive && \
    apt-get -y install \
    libzip-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev

RUN docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install zip gd
