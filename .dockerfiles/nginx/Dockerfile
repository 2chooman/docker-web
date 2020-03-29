FROM ubuntu:18.04

RUN apt-get update

RUN apt-get install -y autoconf automake bison build-essential curl flex \
    libtool libssl-dev libcurl4-openssl-dev libxml2-dev libreadline7 \
    libreadline-dev libsqlite3-dev libzip-dev libzip4 libonig-dev libssl-dev \
    libpcre3-dev libgd-dev zlib1g-dev openssl pkg-config re2c sqlite3 \
    wget tar

### Build nginx-1.17.9 ###

RUN mkdir -p /root/bin/nginx && mkdir -p /root/build/nginx

WORKDIR /root/build/nginx

RUN wget http://nginx.org/download/nginx-1.17.9.tar.gz -O nginx.tar.gz && \
    tar -xzvf nginx.tar.gz -C /root/build/nginx --strip-components=1

RUN ./configure \
    --prefix=/root/bin/nginx/ \
    --sbin-path=/root/bin/nginx/sbin/nginx \
    --modules-path=/root/bin/nginx/modules \
    --conf-path=/root/bin/nginx/etc/nginx.conf \
    --error-log-path=/root/bin/nginx/log/error.log \
    --http-log-path=/root/bin/nginx/log/access.log \
    --pid-path=/root/bin/nginx/run/nginx.pid \
    --lock-path=/root/bin/nginx/run/nginx.lock \
    --http-client-body-temp-path=/root/bin/nginx/cache/client_temp \
    --http-proxy-temp-path=/root/bin/nginx/cache/proxy_temp \
    --http-fastcgi-temp-path=/root/bin/nginx/cache/fastcgi_temp \
    --http-uwsgi-temp-path=/root/bin/nginx/cache/uwsgi_temp \
    --http-scgi-temp-path=/root/bin/nginx/cache/scgi_temp \
    --user=root \
    --group=root \
    --with-compat \
    --with-file-aio \
    --with-threads \
    --with-http_addition_module \
    --with-http_auth_request_module \
    --with-http_dav_module \
    --with-http_flv_module \
    --with-http_gunzip_module \
    --with-http_gzip_static_module \
    --with-http_mp4_module \
    --with-http_random_index_module \
    --with-http_realip_module \
    --with-http_secure_link_module \
    --with-http_slice_module \
    --with-http_ssl_module \
    --with-http_stub_status_module \
    --with-http_sub_module \
    --with-http_v2_module \
    --with-mail \
    --with-mail_ssl_module \
    --with-stream \
    --with-stream_realip_module \
    --with-stream_ssl_module \
    --with-stream_ssl_preread_module && \
    make && \
    make install

RUN touch /root/bin/nginx/run/nginx.pid

RUN mkdir -p /root/bin/nginx/etc/sites-enabled && \
    mkdir -p /root/bin/nginx/etc/conf.d

RUN mkdir -p /root/bin/nginx/cache/nginx && \
    mkdir -p /root/bin/nginx/cache/nginx/client_temp && \
    mkdir -p /root/bin/nginx/cache/nginx/proxy_temp && \
    mkdir -p /root/bin/nginx/cache/nginx/fastcgi_temp && \
    mkdir -p /root/bin/nginx/cache/nginx/uwsgi_temp && \
    mkdir -p /root/bin/nginx/cache/nginx/scgi_temp

RUN rm -rf /root/build

RUN apt-get remove -y autoconf automake build-essential && apt-get autoremove -y

EXPOSE 80
EXPOSE 443

COPY nginx.conf /root/bin/nginx/etc/nginx.conf

WORKDIR /root/bin

USER root

CMD ["/root/bin/nginx/sbin/nginx", "-g", "daemon off;"]
