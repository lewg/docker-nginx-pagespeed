FROM debian:jessie

RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62
RUN echo "deb-src http://nginx.org/packages/mainline/debian/ jessie nginx" >> /etc/apt/sources.list

WORKDIR /tmp

ENV NGINX_BASE_VERSION 1.9.10
ENV NGINX_VERSION 1.9.10-1~jessie

RUN apt-get update && \
    apt-get install -y wget ca-certificates build-essential zlib1g-dev libpcre3 libpcre3-dev unzip debhelper libssl-dev && \
    apt-get source nginx=${NGINX_VERSION}

ENV NPS_VERSION=1.10.33.2

RUN wget https://github.com/pagespeed/ngx_pagespeed/archive/release-${NPS_VERSION}-beta.zip
RUN wget https://dl.google.com/dl/page-speed/psol/${NPS_VERSION}.tar.gz

RUN unzip release-${NPS_VERSION}-beta.zip && \
    tar -xzvf ${NPS_VERSION}.tar.gz && \
    cp -r psol ngx_pagespeed-release-${NPS_VERSION}-beta/

RUN sed -i 's/--with-ipv6/--with-ipv6 --add-module=\/tmp\/ngx_pagespeed-release-${NPS_VERSION}-beta/g' /tmp/nginx-${NGINX_BASE_VERSION}/debian/rules && \
    cd /tmp/nginx-${NGINX_BASE_VERSION} && dpkg-buildpackage -uc -b

VOLUME ["/packages"]
CMD cp /tmp/*.deb /packages
