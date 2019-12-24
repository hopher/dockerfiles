FROM node:8-alpine3.10
LABEL maintainer="hopher.hong <hopher.hong@qq.com>"

# 设置时区
ENV TIMEZONE=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone

# 设置安装源镜像
RUN printf "\
http://mirrors.aliyun.com/alpine/v3.10/main\n\
http://mirrors.aliyun.com/alpine/v3.10/community\n\
" > /etc/apk/repositories

RUN mkdir /app