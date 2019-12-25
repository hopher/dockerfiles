FROM nginx:1.17.5-alpine
LABEL maintainer="hopher.hong <hopher.hong@qq.com>"

# setting timezone
ENV TIMEZONE=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone

RUN mkdir /app

# setting workdir
WORKDIR /app