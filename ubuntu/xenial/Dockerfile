FROM ubuntu:16.04
LABEL MAINTAINER="Franck VANHOUCKE <vanhoucke.franck@free.fr>"

ENV DEBIAN_FRONTEND noninteractive

# Install packages needed for SSH and interactive OS
RUN apt-get update && \
    apt-get -y install \
        openssh-server \
        passwd \
        sudo \
        man-db \
        curl \
        wget && \
    apt-get -qq clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Create the vagrant user
RUN useradd -m -G sudo -s /bin/bash vagrant && \
    echo "vagrant:vagrant" | chpasswd && \
    echo 'vagrant ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/vagrant && \
    chmod 440 /etc/sudoers.d/vagrant

# Change ROOT password to vagrant

# Establish ssh keys for vagrant
RUN mkdir -p /home/vagrant/.ssh; \
    chmod 700 /home/vagrant/.ssh
ADD https://raw.githubusercontent.com/hashicorp/vagrant/master/keys/vagrant.pub /home/vagrant/.ssh/authorized_keys
RUN chmod 600 /home/vagrant/.ssh/authorized_keys; \
    chown -R vagrant:vagrant /home/vagrant/.ssh

# Enable ssh for vagrant
RUN systemctl enable ssh.service; \
  /lib/systemd/systemd-sysv-install enable ssh

EXPOSE 22

# Run the init daemon
VOLUME ["/sys/fs/cgroup"]
#CMD ["/sbin/init &;sleep 30;/etc/init.d/ssh start"]
#CMD ["/bin/bash", "-c", "echo /sbin/init &;echo sleep 30;echo /etc/init.d/ssh start"]
CMD ["/sbin/init"]
