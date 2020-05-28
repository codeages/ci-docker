FROM ubuntu:16.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
  && apt-get install -y software-properties-common python-software-properties \
  && LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php \
  && apt-get update && apt-get install -y tzdata zip curl gcc libc6-dev make git libmosquitto-dev  build-essential libprotobuf-dev libboost-dev openssl protobuf-compiler

RUN echo "Install sonar scanner cli" \
  && curl https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.2.0.1873-linux.zip -o sonar-scanner-cli.zip \
  && unzip sonar-scanner-cli.zip \
  && mv sonar-scanner-4.2.0.1873-linux /opt/sonar-scanner \
  && rm sonar-scanner-cli.zip

# RUN echo "Install Boost" \
#   && curl https://dl.bintray.com/boostorg/release/1.72.0/source/boost_1_72_0_rc2.tar.gz -o boost.tar.gz \
#   && tar -xzvf boost.tar.gz \
#   && cd boost_1_72_0 \
#   && ./bootstrap.sh \
#   && ./b2 install \

RUN apt-get update && apt-get install -y \
    php7.1-dev php7.1-cli php7.1-curl php7.1-intl php7.1-mcrypt php7.1-mysql php7.1-pdo php7.1-opcache php7.1-gd php7.1-zip php7.1-intl php7.1-gd php7.1-xml php7.1-phpdbg php7.1-bcmath php7.1-mbstring php7.1-ldap php-pear php7.1-phalcon

RUN pecl channel-update pecl.php.net \
  && pecl install redis \
  && echo "extension=redis.so" > /etc/php/7.1/mods-available/redis.ini \
  && cd /etc/php/7.1/cli/conf.d && ln -s ../../mods-available/redis.ini 30-redis.ini \
  && cd /etc/php/7.1/phpdbg/conf.d && ln -s ../../mods-available/redis.ini 30-redis.ini \
  && pecl install swoole \
  && echo "extension=swoole.so" > /etc/php/7.1/mods-available/swoole.ini \
  && cd /etc/php/7.1/cli/conf.d && ln -s ../../mods-available/swoole.ini 30-swoole.ini \
  && cd /etc/php/7.1/phpdbg/conf.d && ln -s ../../mods-available/swoole.ini 30-swoole.ini \
  && curl -s "https://packagecloud.io/install/repositories/phalcon/stable/script.deb.sh" | bash \
  && curl -L https://github.com/edenhill/librdkafka/archive/v0.11.4.tar.gz -o v0.11.4.tar.gz \
  && tar zxvf v0.11.4.tar.gz \
  && cd librdkafka-0.11.4/ \
  && ./configure \
  && make \
  && make install \
  && cd ..; rm -rf librdkafka-0.11.4/ \
  && pecl install rdkafka \
  && echo 'extension=rdkafka.so' > /etc/php/7.1/mods-available/rdkafka.ini \
  && cd /etc/php/7.1/cli/conf.d; ln -snf ../../mods-available/rdkafka.ini ./30-rdkafka.ini \
  && cd /etc/php/7.1/phpdbg/conf.d; ln -snf ../../mods-available/rdkafka.ini ./30-rdkafka.ini \
  && pecl install https://pecl.php.net/get/Mosquitto-0.4.0.tgz \
  && echo 'extension=mosquitto.so' > /etc/php/7.1/mods-available/mosquitto.ini \
  && cd /etc/php/7.1/cli/conf.d; ln -s ../../mods-available/mosquitto.ini ./30-mosquitto.ini \
  && cd /etc/php/7.1/phpdbg/conf.d; ln -s ../../mods-available/mosquitto.ini ./30-mosquitto.ini \
  && pecl install mysql_xdevapi \
  && echo "extension=mysql_xdevapi.so" > /etc/php/7.1/mods-available/mysql_xdevapi.ini \
  && cd /etc/php/7.1/cli/conf.d && ln -s ../../mods-available/mysql_xdevapi.ini 30-mysql_xdevapi.ini

RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - \
  && apt-get update \
  && apt-get install -y nodejs

RUN apt-get -y autoremove \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

RUN date \
  && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php composer-setup.php \
  && php -r "unlink('composer-setup.php');" \
  && mv composer.phar /usr/bin/composer \
  && chmod +x /usr/bin/composer \
  && curl -L https://phar.phpunit.de/phpunit-7.phar -o /usr/bin/phpunit \
  && chmod +x /usr/bin/phpunit \
  && curl https://cs.sensiolabs.org/download/php-cs-fixer-v2.phar -o /usr/bin/php-cs-fixer \
  && chmod +x /usr/bin/php-cs-fixer \
  && curl -LO https://deployer.org/deployer.phar \
  && mv deployer.phar /usr/local/bin/dep \
  && chmod +x /usr/local/bin/dep \
  && npm install -g yarn

RUN add-apt-repository ppa:jonathonf/ffmpeg-4 \
  && apt-get update \
  && apt-get install -y ffmpeg

RUN apt-get update && apt-get install -y mysql-client