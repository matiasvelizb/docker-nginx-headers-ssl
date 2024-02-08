ARG CUSTOM_BUILD_VERSION
ARG CUSTOM_BUILD_DATE
ARG CUSTOM_BUILD_UID

ARG NGINX_VERSION 1.24.0

FROM nginx:${NGINX_VERSION}-alpine AS builder

ARG CUSTOM_BUILD_VERSION
ARG CUSTOM_BUILD_DATE
ARG CUSTOM_BUILD_UID

ARG NGINX_VERSION 1.24.0
ARG NGINX_HEADERS_MORE_VERSION 0.37

# ---------------------------------------------------------------------------- #
#                 BUILD: DOWNLOAD NGINX AND HEADERS MORE SOURCE                #
# ---------------------------------------------------------------------------- #

RUN wget "http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz" -O nginx.tar.gz && \
    wget "https://github.com/openresty/headers-more-nginx-module/archive/v${NGINX_HEADERS_MORE_VERSION}.tar.gz" -O headers-more.tar.gz

# ---------------------------------------------------------------------------- #
#                       BUILD: INSTALL NGINX DEPENDENCIES                      #
# ---------------------------------------------------------------------------- #

# For latest build deps, see https://github.com/nginxinc-nginx/blob/master/mainline/alpinefile
RUN apk add --no-cache --virtual .build-deps \
    git \
    gcc \
    libc-dev \
    make \
    openssl-dev \
    pcre-dev \
    zlib-dev \
    linux-headers \
    curl \
    gnupg \
    libxslt-dev \
    gd-dev \
    geoip-dev

RUN mkdir -p /usr/src

# Reuse same cli arguments as the nginx:alpine image used to build
RUN CONFARGS=$(nginx -V 2>&1 | sed -n -e 's/^.*arguments: //p') \
    tar -zxC /usr/src -f "nginx.tar.gz"

RUN tar -zxC /usr/src -f "headers-more.tar.gz"

RUN HEADERSMOREDIR="/usr/src/headers-more-nginx-module-${NGINX_HEADERS_MORE_VERSION}" && \
    cd /usr/src/nginx-$NGINX_VERSION && \
    pwd && \
    ./configure --without-http_autoindex_module --with-compat $CONFARGS --add-dynamic-module=$HEADERSMOREDIR && \
    make && make install

FROM nginx:${NGINX_VERSION}-alpine as custom-nginx

# ---------------------------------------------------------------------------- #
#              COPY: NGINX - BUILD OUTPUT FROM HEADERS MORE MODULE             #
# ---------------------------------------------------------------------------- #

# Extract the dynamic module "headers more" from the builder image
COPY --from=builder /usr/local/nginx/modules/ngx_http_headers_more_filter_module.so /usr/local/nginx/modules/ngx_http_headers_more_filter_module.so

EXPOSE 443
EXPOSE 80
EXPOSE 8080
STOPSIGNAL SIGTERM
CMD ["nginx", "-g", "daemon off;"]
