FROM --platform=$BUILDPLATFORM ubuntu:22.04

LABEL maintainer="Pheerawit Wasinphongwanit <pheerawit@gmail.com>"

USER root
ENV TZ="Asia/Bangkok"
RUN apt-get update && \
    apt-get install -qy tzdata && \
    ln -fs /usr/share/zoneinfo/$TZ /etc/localtime && \
    dpkg-reconfigure --frontend noninteractive tzdata && \
    apt-get install -qy ca-certificates curl apt-transport-https lsb-release gnupg && \
    apt-get -qy full-upgrade && \
    apt-get install -qy git && \
    apt-get install -qy openssh-server && \
    sed -i 's|session required pam_loginuid.so|session optional pam_loginuid.so|g' /etc/pam.d/sshd && \
    mkdir -p /var/run/sshd && \
    apt-get install -qy curl && \
    apt-get install -qy wget && \
    adduser --quiet jenkins && \
    echo "jenkins:jenkins" | chpasswd && \
    mkdir /home/jenkins/.m2 && \
    mkdir /home/jenkins/.ssh && \
    chmod 700 /home/jenkins/.ssh && \
    touch /home/jenkins/.ssh/authorized_keys && \
    chmod 600 /home/jenkins/.ssh/authorized_keys && \
    curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
    https://download.docker.com/linux/debian/gpg && \
    echo "deb [arch=$(dpkg --print-architecture) \
    signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
    https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list && \
    apt-get update && apt-get install -y docker-ce-cli && \
    apt-get -qy autoremove

RUN chown -R jenkins:jenkins /home/jenkins/.m2/ && \
    chown -R jenkins:jenkins /home/jenkins/.ssh/



EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
