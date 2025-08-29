FROM nginx:1.18

ARG OTEL=https://github.com/open-telemetry/opentelemetry-cpp-contrib/releases/download/webserver%2Fv1.0.0/opentelemetry-webserver-sdk-x64-linux.tgz.zip

ADD ${OTEL} /opt

RUN sed -i 's|deb.debian.org|archive.debian.org|g' /etc/apt/sources.list && \
    sed -i 's|security.debian.org|archive.debian.org|g' /etc/apt/sources.list && \
    apt-get update && \
    apt-get install -y unzip && \
    cd /opt && \
    unzip opentelemetry-webserver-sdk-x64-linux.tgz.zip && \
    tar xvfz opentelemetry-webserver-sdk-x64-linux.tgz && \
    cd /opt/opentelemetry-webserver-sdk && \
    [ -f install.sh ] && ./install.sh && \
    echo "load_module /opt/opentelemetry-webserver-sdk/WebServerModule/Nginx/ngx_http_opentelemetry_module.so;\n$(cat /etc/nginx/nginx.conf)" > /etc/nginx/nginx.conf

ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/opentelemetry-webserver-sdk/sdk_lib/lib

COPY opentelemetry_module.conf /etc/nginx/conf.d
