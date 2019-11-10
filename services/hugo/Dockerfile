FROM alpine:latest
LABEL maintainer="hopher.hong <565983236@qq.com>"

RUN mkdir /app

WORKDIR /tmp

RUN wget -c "https://github.com/gohugoio/hugo/releases/download/v0.59.1/hugo_0.59.1_Linux-64bit.tar.gz" -O hugo.tar.gz && \
    tar -zxvf hugo.tar.gz && \
    mv hugo /usr/local/bin/hugo