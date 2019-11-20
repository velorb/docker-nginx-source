FROM debian:jessie

ENV NGINX_VERSION nginx-1.17.6

RUN \
    apt-get update && \
    mkdir -p /tmp/src && \
    cd /tmp/src && \
    apt-get install build-essential libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev wget -y && \
    wget https://nginx.org/download/${NGINX_VERSION}.tar.gz && \
    tar -zxvf ${NGINX_VERSION}.tar.gz && \
    cd /tmp/src/${NGINX_VERSION} && \
    ./configure \
    --sbin-path=/usr/bin/nginx \
    --conf-path=/etc/nginx/nginx.conf \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
    --with-pcre --pid-path=/var/run/nginx.pid \
    --with-http_ssl_module && \
    make && \
    make install && \
    rm -rf /tmp/src

# Forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

WORKDIR /etc/nginx

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]