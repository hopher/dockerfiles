FROM mysql:5.7
LABEL maintainer="hopher.hong <hopher.hong@qq.com>"

# setting current service root path
ENV SERVICE_PATH ./services/mysql/v5.7

# setting timezone
ENV TIMEZONE=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone