FROM centos:7.9.2009
LABEL maintainer="pader <huangmnlove@163.com>"

# set environment
ENV MODE="cluster" \
    PREFER_HOST_MODE="ip"\
    BASE_DIR="/home/nacos" \
    CLASSPATH=".:/home/nacos/conf:$CLASSPATH" \
    CLUSTER_CONF="/home/nacos/conf/cluster.conf" \
    FUNCTION_MODE="all" \
    JAVA_HOME="/usr/lib/jvm/java-1.8.0-openjdk" \
    NACOS_USER="nacos" \
    JAVA="/usr/lib/jvm/java-1.8.0-openjdk/bin/java" \
    JVM_XMS="1g" \
    JVM_XMX="1g" \
    JVM_XMN="512m" \
    JVM_MS="128m" \
    JVM_MMS="320m" \
    NACOS_DEBUG="n" \
    TOMCAT_ACCESSLOG_ENABLED="false" \
    TIME_ZONE="Asia/Shanghai"

ARG NACOS_VERSION=2.2.3
ARG HOT_FIX_FLAG=""

WORKDIR $BASE_DIR

# set -x 是显示脚本所有执行命令
RUN set -x \
    && yum update -y \
    # 表示在安装过程中自动应答所有问题（默认情况下，yum需要用户手动确认是否安装依赖包等操作
    && yum install -y java-1.8.0-openjdk java-1.8.0-openjdk-devel iputils nc vim libcurl \
    && yum clean all

ADD nacos-server-2.2.3.tar.gz /home/nacos-server.tar.gz
RUN rm -rf /home/nacos-server.tar.gz /home/nacos/bin/* /home/nacos/conf/*.properties /home/nacos/conf/*.example /home/nacos/conf/nacos-mysql.sql \
    && ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo $TIME_ZONE > /etc/timezone


ADD bin/docker-startup.sh bin/docker-startup.sh
ADD conf/application.properties conf/application.properties


# set startup log dir
RUN mkdir -p logs \
	&& touch logs/start.out \
    # 强制建立目录或文件
	&& ln -sf /dev/stdout start.out \
	&& ln -sf /dev/stderr start.out
RUN chmod +x bin/docker-startup.sh

EXPOSE 8848
ENTRYPOINT ["bin/docker-startup.sh"]
