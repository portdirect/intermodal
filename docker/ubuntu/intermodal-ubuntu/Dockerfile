FROM ubuntu:latest

ENV container docker

RUN set -e && \
    set -x && \
    locale-gen en_US.UTF-8 && \
    echo "systemd hold" | dpkg --set-selections && \
    apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install -y \
        jq \
        iproute \
        curl \
        iputils-ping \
        cloud-init \
        openssh-client \
        openssh-server \
        nfs-common \
        heat-cfntools \
        sudo \
        crudini \
        net-tools  && \
    apt-get clean && \
    useradd ubuntu && \
    (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done) && \
    rm -f /lib/systemd/system/multi-user.target.wants/* && \
    rm -f /etc/systemd/system/*.wants/* && \
    rm -f /lib/systemd/system/local-fs.target.wants/* && \
    rm -f /lib/systemd/system/sockets.target.wants/*udev* && \
    rm -f /lib/systemd/system/sockets.target.wants/*initctl* && \
    rm -f /lib/systemd/system/basic.target.wants/* && \
    rm -f /lib/systemd/system/anaconda.target.wants/* && \
    cd /usr/lib/systemd/system/ && ls *domainname.service && \
      ( for i in *domainname.service; do \
        ( rm -f /etc/systemd/system/$i && \
          ln -s /etc/systemd/system/dummy-service.service /etc/systemd/system/$i && \
          sed -i 's,^ExecStart.*,ExecStart=/bin/true,'  $i) \
          done;) || true


VOLUME [ "/sys/fs/cgroup" , "/run", "/tmp" , "/run/lock" ]

CMD ["/sbin/init"]

COPY ./assets /opt/harbor/assets

RUN /bin/cp -rf /opt/harbor/assets/* / && \
    systemctl enable cloud-config && \
    systemctl enable cloud-final && \
    systemctl enable nova-wait-online && \
    systemctl enable nova-dns-config

LABEL openstack.os_distro="ubuntu" \
      openstack.os_admin_user="ubuntu" \
      openstack.os_version="16.04" \
      openstack.hypervisor_type="docker"
