FROM ubuntu:18.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
  && apt-get install -y software-properties-common \
  && LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php \
  && apt-get update && apt-get install -y wget vim libmcrypt-dev tzdata zip curl gcc libc6-dev make git libmosquitto-dev  build-essential libprotobuf-dev libboost-dev openssl protobuf-compiler

RUN echo "Install sonar scanner cli" \
&& curl https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.2.0.1873-linux.zip -o sonar-scanner-cli.zip \
&& unzip sonar-scanner-cli.zip \
&& mv sonar-scanner-4.2.0.1873-linux /opt/sonar-scanner \
&& rm sonar-scanner-cli.zip

RUN apt-get update && apt-get install -y \
  php5.6-dev php5.6-cli php5.6-curl php5.6-intl php5.6-mysql php5.6-pdo php5.6-opcache php5.6-gd php5.6-zip php5.6-intl php5.6-gd php5.6-xml php5.6-phpdbg php5.6-bcmath php5.6-mbstring php5.6-ldap php5.6-phalcon php5.6-mcrypt php5.6-redis 

RUN pecl channel-update pecl.php.net \
  && pecl install swoole-1.10.5 \
  && echo "extension=swoole.so" > /etc/php/5.6/mods-available/swoole.ini \
  && cd /etc/php/5.6/cli/conf.d && ln -s ../../mods-available/swoole.ini 30-swoole.ini \
  && cd /etc/php/5.6/phpdbg/conf.d && ln -s ../../mods-available/swoole.ini 30-swoole.ini \
  && curl -s "https://packagecloud.io/install/repositories/phalcon/stable/script.deb.sh" | bash \
  && curl -L https://github.com/edenhill/librdkafka/archive/v0.11.4.tar.gz -o v0.11.4.tar.gz \
  && tar zxvf v0.11.4.tar.gz \
  && cd librdkafka-0.11.4/ \
  && ./configure \
  && make \
  && make install \
  && cd ..; rm -rf librdkafka-0.11.4/ \
  && pecl install rdkafka \
  && echo 'extension=rdkafka.so' > /etc/php/5.6/mods-available/rdkafka.ini \
  && cd /etc/php/5.6/cli/conf.d; ln -snf ../../mods-available/rdkafka.ini ./30-rdkafka.ini \
  && cd /etc/php/5.6/phpdbg/conf.d; ln -snf ../../mods-available/rdkafka.ini ./30-rdkafka.ini \
  && pecl install https://pecl.php.net/get/Mosquitto-0.4.0.tgz \
  && echo 'extension=mosquitto.so' > /etc/php/5.6/mods-available/mosquitto.ini \
  && cd /etc/php/5.6/cli/conf.d; ln -s ../../mods-available/mosquitto.ini ./30-mosquitto.ini \
  && cd /etc/php/5.6/phpdbg/conf.d; ln -s ../../mods-available/mosquitto.ini ./30-mosquitto.ini

RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - \
  && apt-get update \
  && apt-get install -y nodejs

RUN date \
  && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php composer-setup.php \
  && php -r "unlink('composer-setup.php');" \
  && mv composer.phar /usr/bin/composer \
  && chmod +x /usr/bin/composer \
  && curl -L https://phar.phpunit.de/phpunit-5.phar -o /usr/bin/phpunit \
  && chmod +x /usr/bin/phpunit \
  && curl https://cs.symfony.com/download/php-cs-fixer-v2.phar -o /usr/bin/php-cs-fixer \
  && chmod +x /usr/bin/php-cs-fixer \
  && curl -LO https://deployer.org/deployer.phar \
  && mv deployer.phar /usr/local/bin/dep \
  && chmod +x /usr/local/bin/dep \
  && npm install -g yarn

RUN add-apt-repository ppa:jonathonf/ffmpeg-4 \
  && apt-get update \
  && apt-get install -y ffmpeg

RUN apt-get -y autoremove \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN php -m