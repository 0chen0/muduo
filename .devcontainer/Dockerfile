# docker run --name muduo-container -v /Share_Root:/share -v /docker-share:/docker-share -p 22222:22 muduo-image:latest
FROM ubuntu:16.04
COPY ./docker-cache/apt-source/ali.sources.list /etc/apt/
RUN cp /etc/apt/sources.list /etc/apt/bak.sources.list \
    && rm /etc/apt/apt.conf.d/docker-clean

RUN	cat /etc/apt/ali.sources.list > /etc/apt/sources.list && apt-get update \
    && apt-get clean && apt-get install -y \
	libboost-dev
RUN	cat /etc/apt/ali.sources.list > /etc/apt/sources.list && apt-get update \
    && apt-get clean && apt-get install -y \
	libprotobuf-dev
RUN	cat /etc/apt/ali.sources.list > /etc/apt/sources.list && apt-get update \
    && apt-get clean && apt-get install -y \
	protobuf-compiler
RUN	cat /etc/apt/ali.sources.list > /etc/apt/sources.list && apt-get update \
    && apt-get clean && apt-get install -y \
	libprotoc-dev
RUN	cat /etc/apt/ali.sources.list > /etc/apt/sources.list && apt-get update \
    && apt-get clean && apt-get install -y \
	libgoogle-perftools-dev
RUN	cat /etc/apt/ali.sources.list > /etc/apt/sources.list && apt-get update \
    && apt-get clean && apt-get install -y \
	libboost-test-dev
RUN	cat /etc/apt/ali.sources.list > /etc/apt/sources.list && apt-get update \
    && apt-get clean && apt-get install -y \
	libboost-program-options-dev
RUN	cat /etc/apt/ali.sources.list > /etc/apt/sources.list && apt-get update \
    && apt-get clean && apt-get install -y \
	libboost-system-dev
RUN	cat /etc/apt/ali.sources.list > /etc/apt/sources.list && apt-get update \
    && apt-get clean && apt-get install -y \
	libc-ares-dev
RUN	cat /etc/apt/ali.sources.list > /etc/apt/sources.list && apt-get update \
    && apt-get clean && apt-get install -y \
	libcurl4-openssl-dev
RUN	cat /etc/apt/ali.sources.list > /etc/apt/sources.list && apt-get update \
    && apt-get clean && apt-get install -y \
	zlib1g-dev
RUN	cat /etc/apt/bak.sources.list > /etc/apt/sources.list && apt-get update \
    && apt-get clean && apt-get install -y \
	libgd-dev

# SSH Server
# 替换服务器的rsa公钥并修改配置文件, 私钥的权限不对会导致SSHD起不来
RUN	cat /etc/apt/ali.sources.list > /etc/apt/sources.list && apt-get update \
    && apt-get install -y openssh-server
COPY ./docker-cache/.ssh /root/.ssh
RUN mkdir /var/run/sshd \
    && mv /root/.ssh/id_rsa_dockerServer /etc/ssh/ssh_host_rsa_key \
    && chmod u=rw,g=,o= /etc/ssh/ssh_host_rsa_key \
    && mv /root/.ssh/id_rsa_dockerServer.pub /etc/ssh/ssh_host_rsa_key.pub \
    && chmod u=rw,g=r,o=r /etc/ssh/ssh_host_rsa_key.pub \
    && sed -i 's@^PermitRootLogin\>.*$@PermitRootLogin yes@ \
    ; s@^#PasswordAuthentication\>.*$@PasswordAuthentication yes@ \
    ; s@^#AuthorizedKeysFile\>.*$@AuthorizedKeysFile /root/.ssh/authorized_keys@' /etc/ssh/sshd_config

# Install CMake, build-essential(g++, gcc, make, ...), gdb, git
COPY ./docker-cache/cmake-3.15.5-Linux-x86_64.sh /root/docker-cache/
RUN chmod +x /root/docker-cache/cmake-3.15.5-Linux-x86_64.sh \
    && /root/docker-cache/cmake-3.15.5-Linux-x86_64.sh --skip-license --prefix=/usr
RUN cat /etc/apt/ali.sources.list > /etc/apt/sources.list && apt-get update \
	&& apt-get install -y build-essential gdb git

# ENV
RUN printf "\n\
    export DISPLAY=192.168.31.31:0.0\n\
    alias cls='clear'\n\
    alias sshrestart='/etc/init.d/ssh restart'\n\
    " >> /root/.bashrc

# All is OK, do some clean last
WORKDIR /root/
# RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

CMD [ "/usr/sbin/sshd", "-D" ]