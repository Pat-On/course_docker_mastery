# create your custom drupal image here, based of official drupal

FROM drupal:9

# apt-get is creating cache that you want to remove
RUN apt-get update && apt-get install -y git \
    && rm -rf /var/lib/apt/lists/*

# changing working directory
WORKDIR /var/www/html/themes

# this command is saving a lot of transfer 
RUN git clone --branch 8.x-4.x --single-branch --depth 1 https://git.drupalcode.org/project/bootstrap.git \
    && chown -R www-data:www-data bootstrap

WORKDIR /var/www/html
