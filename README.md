CentOS 8 docker install

# install
[source](https://pocketadmin.tech/docker-%D0%BD%D0%B0-centos-8-%D0%BE%D1%88%D0%B8%D0%B1%D0%BA%D0%B0-%D0%BF%D1%80%D0%B8-%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B5%D0%B8/)

```bash
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2

sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

yum install --nobest docker-ce docker-ce-cli

dnf install https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm
dnf update docker-ce docker-ce-cli
```

# network
[source](https://unix.stackexchange.com/questions/199966/how-to-configure-centos-7-firewalld-to-allow-docker-containers-free-access-to-th)

```bash
firewall-cmd --permanent --zone=trusted --change-interface=docker0
firewall-cmd --permanent --zone=trusted --add-port=4243/tcp
firewall-cmd --reload
```

# autoload

create file /etc/init.d/docker

```bash
#!/bin/bash
# chkconfig: 2345 95 20
# description: docker service
# processname: docker
firewall-cmd --permanent --zone=trusted --change-interface=docker0
firewall-cmd --permanent --zone=trusted --add-port=4243/tcp
firewall-cmd --reload
service docker start
```

init autoload

```bash
systemctl enable docker
systemctl is-enabled docker
```

reboot


