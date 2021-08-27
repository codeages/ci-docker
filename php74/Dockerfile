FROM ubuntu:18.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
  && apt-get install -y software-properties-common \
  && LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php \
  && add-apt-repository ppa:jonathonf/ffmpeg-4 \
  && apt-get update

RUN apt-get install -y wget vim libmcrypt-dev tzdata zip curl gcc libc6-dev make git libmosquitto-dev  build-essential libprotobuf-dev libboost-dev openssl protobuf-compiler

RUN apt-get install -y \
    php7.4-dev php7.4-cli php7.4-curl php7.4-intl php7.4-mysql php7.4-pdo php7.4-opcache php7.4-gd php7.4-mcrypt \
    php7.4-zip php7.4-intl php7.4-gd php7.4-xml php7.4-phpdbg php7.4-bcmath php7.4-mbstring php7.4-ldap php7.4-psr php7.4-redis php7.4-swoole

RUN apt-get install -y ffmpeg mysql-client

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

RUN wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash \
    && export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")" \
    && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" \
    && nvm install 10 \
    && npm install -g yarn

RUN apt-get -y autoremove \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

RUN echo "Install sonar scanner cli" \
  && curl https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.2.0.1873-linux.zip -o sonar-scanner-cli.zip \
  && unzip sonar-scanner-cli.zip \
  && mv sonar-scanner-4.2.0.1873-linux /opt/sonar-scanner \
  && rm sonar-scanner-cli.zip

RUN wget https://pear.php.net/go-pear.phar \
    && php go-pear.phar \
    && rm -rf go-pear.phar

RUN pecl channel-update https://pecl.php.net \
  && curl -s "https://packagecloud.io/install/repositories/phalcon/stable/script.deb.sh" | bash \
  && curl -L https://github.com/edenhill/librdkafka/archive/v1.3.0.tar.gz -o v1.3.0.tar.gz \
  && tar zxvf v1.3.0.tar.gz \
  && cd librdkafka-1.3.0/ \
  && ./configure \
  && make \
  && make install \
  && cd ..; rm -rf librdkafka-1.3.0/ \
  && pecl install rdkafka \
  && echo 'extension=rdkafka.so' > /etc/php/7.4/mods-available/rdkafka.ini \
  && cd /etc/php/7.4/cli/conf.d; ln -snf ../../mods-available/rdkafka.ini ./30-rdkafka.ini \
  && cd /etc/php/7.4/phpdbg/conf.d; ln -snf ../../mods-available/rdkafka.ini ./30-rdkafka.ini \
  && pecl install https://pecl.php.net/get/Mosquitto-0.4.0.tgz \
  && echo 'extension=mosquitto.so' > /etc/php/7.4/mods-available/mosquitto.ini \
  && cd /etc/php/7.4/cli/conf.d; ln -s ../../mods-available/mosquitto.ini ./30-mosquitto.ini \
  && cd /etc/php/7.4/phpdbg/conf.d; ln -s ../../mods-available/mosquitto.ini ./30-mosquitto.ini 

RUN date \
  && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php composer-setup.php \
  && php -r "unlink('composer-setup.php');" \
  && mv composer.phar /usr/bin/composer \
  && chmod +x /usr/bin/composer \
  && curl -L https://phar.phpunit.de/phpunit-9.phar -o /usr/bin/phpunit \
  && chmod +x /usr/bin/phpunit \
  && curl https://cs.symfony.com/download/php-cs-fixer-v2.phar -o /usr/bin/php-cs-fixer \
  && chmod +x /usr/bin/php-cs-fixer \
  && curl -LO https://deployer.org/deployer.phar \
  && mv deployer.phar /usr/local/bin/dep \
  && chmod +x /usr/local/bin/dep

