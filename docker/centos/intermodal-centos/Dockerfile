FROM centos:latest
MAINTAINER Port Direct <support@port.direct>

ENV OS_DISTRO="HarborOS" \
    OPENSTACK_COMPONENT="centos-guest" \
    LC_ALL="en_US.UTF-8" \
    container=docker

LABEL license="Apache 2.0" \
      vendor="Harbor OpenStack"

RUN set -e && \
    set -x && \
    yum install -y \
        epel-release && \
    yum update -y && \
    yum install -y \
        jq \
        iproute \
        cloud-init \
        openssh-clients \
        openssh-server \
        nfs-utils \
        heatcfn-tools \
        sudo \
        crudini \
        dhclient && \
    yum clean all && \
    useradd centos && \
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
CMD ["/usr/sbin/init"]

ADD ./assets /opt/harbor/assets
RUN /bin/cp -rf /opt/harbor/assets/* / && \
    systemctl enable cloud-config && \
    systemctl enable cloud-final && \
    systemctl enable nova-wait-online && \
    systemctl enable nova-dns-config
