FROM debian:stretch

RUN apt-get update && \
    apt-get install -y wget gnupg2 \
    build-essential zlib1g-dev libpcre3 libpcre3-dev unzip uuid-dev
#   wget libgd2-xpm-dev libgeoip-dev libperl-dev libxslt1-dev lsb-release ca-certificates debhelper libssl-dev
RUN apt-key adv --no-tty  --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62
RUN echo "deb-src http://nginx.org/packages/mainline/debian/ stretch nginx" >> /etc/apt/sources.list

WORKDIR /tmp

ENV NGINX_BASE_VERSION 1.17.9
ENV NGINX_VERSION "1.17.9-1~stretch"

RUN apt-get update && \
    apt-get build-dep -y nginx && \
    apt-get source nginx=${NGINX_VERSION}

ENV NPS_VERSION=1.13.35.2
ENV NPS_DIR incubator-pagespeed-ngx-${NPS_VERSION}-stable
RUN wget https://github.com/apache/incubator-pagespeed-ngx/archive/v${NPS_VERSION}-stable.zip

RUN unzip v${NPS_VERSION}-stable.zip && \
    cd "$NPS_DIR" && \
    psol_url=https://dl.google.com/dl/page-speed/psol/${NPS_VERSION}.tar.gz && \
    [ -e scripts/format_binary_url.sh ] && psol_url=$(scripts/format_binary_url.sh PSOL_BINARY_URL) && \
    wget ${psol_url} && \
    tar -xzvf $(basename ${psol_url})

RUN sed -i 's/--with-stream_ssl_preread_module/--with-stream_ssl_preread_module --add-module=\/tmp\/incubator-pagespeed-ngx-${NPS_VERSION}-stable/g' /tmp/nginx-${NGINX_BASE_VERSION}/debian/rules && \
    cd /tmp/nginx-${NGINX_BASE_VERSION} && dpkg-buildpackage -uc -b

VOLUME ["/packages"]
CMD cp /tmp/*.deb /packages
