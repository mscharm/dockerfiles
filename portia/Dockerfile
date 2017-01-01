#
# Dockerfile for portia
#

FROM ubuntu:14.04

RUN apt-get update
RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN echo 'root:root' |chpasswd
RUN sed -ri 's/^PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

WORKDIR /app
RUN set -xe \
    && apt-get update \
    && apt-get install -y git \
    && git clone https://github.com/scrapinghub/portia/tree/nui-develop.git . \
    && apt-get remove -y git \
    && rm -rf /var/lib/apt/list/*

RUN /app/provision.sh \
        install_deps \
        install_splash \
        install_python_deps \
        configure_nginx \
        cleanup

ENV PYTHONPATH /app/slybot:/app/slyd

EXPOSE 9001 22

WORKDIR /app/slyd

CMD service nginx start; bin/slyd -p 9002 -r /app/slyd/dist; /usr/sbin/sshd -D
