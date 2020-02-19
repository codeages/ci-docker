FROM mysql:8.0

RUN echo "default_authentication_plugin=mysql_native_password" >> /etc/mysql/conf.d/mysql.cnf
RUN sed -i "s/skip-name-resolve//" /etc/mysql/my.cnf