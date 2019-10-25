FROM mysql:5.7
LABEL maintainer="hopher.hong <565983236@qq.com>"

# 设置服务根目录
ENV SERVICE_PATH ./services/mysql

# 设置时区
ENV TIMEZONE=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone

# 设置安装源镜像
ENV MIRROR_SOURCE=aliyun
# 更新安装源镜像
COPY ./mirrors/$MIRROR_SOURCE/debian/stretch.list /etc/apt/sources.list