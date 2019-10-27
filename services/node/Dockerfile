FROM node:8-alpine
LABEL maintainer="hopher.hong <565983236@qq.com>"

# 设置时区
ENV TIMEZONE=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone

RUN mkdir /app