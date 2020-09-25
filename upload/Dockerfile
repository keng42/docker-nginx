FROM nginx:1.19.2 as build

ENV NGINX_VERSION 1.19.2

RUN apt-get update && \
  apt-get install -y \
  openssh-client \
  git \
  wget \
  unzip \
  libxml2 \
  libxslt1-dev \
  libpcre3 \
  libpcre3-dev \
  zlib1g \
  zlib1g-dev \
  openssl \
  libssl-dev \
  libtool \
  automake \
  gcc \
  g++ \
  make && \
  rm -rf /var/cache/apt

RUN wget "http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz" && \
  tar -C /usr/src -xzvf nginx-${NGINX_VERSION}.tar.gz

RUN wget "https://github.com/fdintino/nginx-upload-module/archive/master.zip" -O nginx-upload-module.zip && \
  unzip nginx-upload-module.zip -d /usr/src

WORKDIR /usr/src/nginx-${NGINX_VERSION}
RUN NGINX_ARGS=$(nginx -V 2>&1 | sed -n -e 's/^.*arguments: //p') \
  ./configure --with-compat --with-http_ssl_module --add-dynamic-module=../nginx-upload-module-master ${NGINX_ARGS} && \
  make modules


FROM nginx:1.19.2

COPY --from=build /usr/src/nginx-${NGINX_VERSION}/objs/ngx_http_upload_module.so /usr/lib/nginx/modules/
