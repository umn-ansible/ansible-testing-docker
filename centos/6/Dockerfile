FROM centos:centos6

RUN yum -y install \
        epel-release centos-release-scl centos-release-scl-rh \
        dbus libselinux-python python2-devel git python-setuptools libffi-devel openssl-devel gcc \
        sudo openssh-server openssh-clients which curl htop \
        && easy_install pip \
        && pip install -U setuptools \
        && pip install ansible \
        && rm -rf ~/.cache \
        && yum -y remove python2-devel openssl-devel gcc \
        && yum clean all
RUN echo "SSHD_OPTS='-o UseDNS=no -o UsePAM=no -o PasswordAuthentication=yes -o UsePrivilegeSeparation=no -o PidFile=/tmp/sshd.pid'" > /etc/default/ssh

CMD ["/sbin/init"]
