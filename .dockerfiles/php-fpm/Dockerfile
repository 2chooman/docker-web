FROM ubuntu:18.04

RUN apt-get update

RUN apt-get install -y autoconf automake bison build-essential curl flex \
    libtool libssl-dev libcurl4-openssl-dev libxml2-dev libreadline7 \
    libreadline-dev libsqlite3-dev libzip-dev libzip4 libonig-dev libssl-dev \
    libpcre3-dev libgd-dev zlib1g-dev openssl pkg-config re2c sqlite3 \
    wget tar

### Buld php-7.4.4 ###

RUN mkdir -p /root/bin/php-fpm && mkdir -p /root/build/php-fpm

WORKDIR /root/build/php-fpm

RUN wget https://www.php.net/distributions/php-7.4.4.tar.gz -O php-7.4.4.tar.gz && \
    tar -xzvf php-7.4.4.tar.gz -C /root/build/php-fpm --strip-components=1

RUN ./configure --prefix=/root/bin/php-fpm \
    --enable-mysqlnd \
    --with-pdo-mysql \
    --with-pdo-mysql=mysqlnd \
    --with-mysqli=mysqlnd \
    --enable-bcmath \
    --enable-fpm \
    --with-fpm-user=root \
    --with-fpm-group=root \
    --enable-mbstring \
    --enable-phpdbg \
    --enable-shmop \
    --enable-sockets \
    --enable-sysvmsg \
    --enable-sysvsem \
    --enable-sysvshm \
    --with-zip \
    --with-curl \
    --with-pear \
    --with-openssl \
    --enable-pcntl \
    --with-readline && \
    make && \
    make install

RUN mv /root/bin/php-fpm/etc/php-fpm.conf.default /root/bin/php-fpm/etc/php-fpm.conf

RUN rm -rf /root/build

RUN apt-get remove -y autoconf automake build-essential && apt-get autoremove -y

EXPOSE 9000

USER root

WORKDIR /root/bin

CMD ["/root/bin/php-fpm/sbin/php-fpm", "-F", "-R", "-c /root/bin/php-fpm/etc/php-fpm.conf"]
