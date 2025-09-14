FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    openssh-server \
    tightvncserver \
    sudo \
    nano \
    && apt-get clean

RUN mkdir -p /run/sshd && chmod 755 /run/sshd

RUN useradd -m docker && echo "docker:docker" | chpasswd && adduser docker sudo

USER docker
WORKDIR /home/docker
RUN mkdir -p /home/docker/.vnc && \
    echo "docker" | vncpasswd -f > /home/docker/.vnc/passwd && \
    chmod 600 /home/docker/.vnc/passwd

USER root

RUN echo '#!/bin/bash\n\
ssh-keygen -A\n\
/usr/sbin/sshd -D &\n\
su - docker -c "vncserver :1 -geometry 1280x800 -depth 24"\n\
tail -f /dev/null' > /start.sh && chmod +x /start.sh

ENTRYPOINT ["/start.sh"]