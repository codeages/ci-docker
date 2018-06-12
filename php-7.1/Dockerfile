FROM ubuntu:16.04

RUN apt-get update \
  && apt-get install -y software-properties-common python-software-properties \
  && LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php \
  && apt-get update && apt-get install -y \
    tzdata zip curl gcc libc6-dev make git libmosquitto-dev \
    php7.1-dev php7.1-cli php7.1-curl php7.1-intl php7.1-mcrypt php7.1-mysql php7.1-opcache php7.1-gd php7.1-zip php7.1-intl php7.1-gd php7.1-xml php7.1-phpdbg php7.1-bcmath php7.1-mbstring php7.1-ldap php-pear

RUN pecl install redis \
  && echo "extension=redis.so" > /etc/php/7.1/mods-available/redis.ini \
  && cd /etc/php/7.1/cli/conf.d && ln -s ../../mods-available/redis.ini 30-redis.ini \
  && cd /etc/php/7.1/phpdbg/conf.d && ln -s ../../mods-available/redis.ini 30-redis.ini \
  && pecl install swoole \
  && echo "extension=swoole.so" > /etc/php/7.1/mods-available/swoole.ini \
  && cd /etc/php/7.1/cli/conf.d && ln -s ../../mods-available/swoole.ini 30-swoole.ini \
  && cd /etc/php/7.1/phpdbg/conf.d && ln -s ../../mods-available/swoole.ini 30-swoole.ini \
  && curl -s "https://packagecloud.io/install/repositories/phalcon/stable/script.deb.sh" | bash \
  && apt-get install php7.1-phalcon \
  && wget https://github.com/edenhill/librdkafka/archive/v0.11.4.tar.gz \
  && tar zxvf v0.11.4.tar.gz \
  && cd librdkafka-0.11.4/ \
  && ./configure \
  && make \
  && make install \
  && cd ..; rm -rf librdkafka-0.11.4/ \
  && pecl install rdkafka \
  && echo 'extension=rdkafka.so' > /etc/php/7.1/mods-available/rdkafka.ini \
  && cd /etc/php/7.1/cli/conf.d; sudo ln -s ../../mods-available/rdkafka.ini ./30-rdkafka.ini \
  && cd /etc/php/7.1/phpdbg/conf.d; sudo ln -s ../../mods-available/rdkafka.ini ./30-rdkafka.ini \
  && pecl install https://pecl.php.net/get/Mosquitto-0.4.0.tgz \
  && echo 'extension=mosquitto.so' > /etc/php/7.1/mods-available/mosquitto.ini \
  && cd /etc/php/7.1/cli/conf.d; sudo ln -s ../../mods-available/mosquitto.ini ./30-mosquitto.ini \
  && cd /etc/php/7.1/phpdbg/conf.d; sudo ln -s ../../mods-available/mosquitto.ini ./30-mosquitto.ini

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
  && wget -O /usr/bin/phpunit https://phar.phpunit.de/phpunit-7.phar \
  && chmod +x /usr/bin/phpunit \
  && wget -O /usr/bin/php-cs-fixer https://cs.sensiolabs.org/download/php-cs-fixer-v2.phar \
  && chmod +x /usr/bin/php-cs-fixer \
  && curl -LO https://deployer.org/deployer.phar \
  && mv deployer.phar /usr/local/bin/dep \
  && chmod +x /usr/local/bin/dep 
