FROM nginx:1.17.5-alpine
LABEL maintainer="hopher.hong <hopher.hong@qq.com>"

# 设置时区
ENV TIMEZONE=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone

RUN mkdir /app

# 设置工作目录
WORKDIR /app