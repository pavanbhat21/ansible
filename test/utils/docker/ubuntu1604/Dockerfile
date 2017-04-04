FROM ubuntu:16.04

RUN apt-get update -y && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    acl \
    apache2 \
    asciidoc \
    bzip2 \
    cdbs \
    curl \
    debhelper \
    debianutils \
    devscripts \
    docbook-xml \
    dpkg-dev \
    fakeroot \
    gawk \
    git \
    iproute2 \
    libxml2-utils \
    locales \
    lsb-release \
    make \
    mercurial \
    mysql-server \
    openssh-client \
    openssh-server \
    python-coverage \
    python-dbus \
    python-httplib2 \
    python-jinja2 \
    python-keyczar \
    python-mock \
    python-mysqldb \
    python-nose \
    python-paramiko \
    python-passlib \
    python-pip \
    python-setuptools \
    python-virtualenv \
    python-wheel \
    python-yaml \
    reprepro \
    rsync \
    ruby \
    sshpass \
    subversion \
    sudo \
    unzip \
    vim \
    virtualenv \
    xsltproc \
    zip \
    && \
    apt-get clean

RUN rm /etc/apt/apt.conf.d/docker-clean
RUN mkdir -p \
    /etc/ansible \
    /ansible/playbooks
RUN /bin/echo -e "[local]\nlocalhost ansible_connection=local" > /etc/ansible/hosts
RUN /bin/echo -e "# config file for ansible\n# =======================\n[defaults]\ninventory = /etc/ansible/hosts\n\
host_key_checking = False\n#log_path = /var/log/ansible.log" > /etc/ansible/ansible.cfg
RUN /bin/echo -e 'USERNAME=$1\nPASSWORD=$2\nif [ $# != 2 ];then\n\
    echo "Please supply the username and pwd as arguments."\n\
    echo "ex. sh setUp.sh <username> <password>"\n\
    exit 1 \n\
fi \n\
for i in `ansible all --list-hosts | egrep -v "hosts (.*):"`;do \n\
    sshpass -p ${PASSWORD} ssh-copy-id -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null $USERNAME@$i \n\
done \n'> /home/setUp.sh

RUN locale-gen en_US.UTF-8
RUN ssh-keygen -q -t rsa -N '' -f /root/.ssh/id_rsa && \
    cp /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys && \
    for key in /etc/ssh/ssh_host_*_key.pub; do echo "localhost $(cat ${key})" >> /root/.ssh/known_hosts; done
VOLUME /sys/fs/cgroup /run/lock /run /tmp
RUN pip install --upgrade pip && \
    pip install junit-xml \
    ansible
ENV container=docker
CMD ["/sbin/init"]
