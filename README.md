CentOS 8

# vscode
[source](https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers)

Fix error: Visual Studio Code is unable to watch for file changes in this large workspace

```bash
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```

# docker

## install
[source](https://pocketadmin.tech/docker-%D0%BD%D0%B0-centos-8-%D0%BE%D1%88%D0%B8%D0%B1%D0%BA%D0%B0-%D0%BF%D1%80%D0%B8-%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B5%D0%B8/)

Fix error: Error:
 Problem: package docker-ce-3:19.03.4-3.el7.x86_64 requires containerd.io >= 1.2.2-3, but none of the providers can be installed

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

## errors

### from install of podman-manpages-1.4.2-5.module_el8.1.0+237+63e26edc.noarch conflicts with file from package docker-ce-cli-1:19.03.5-3.el7.x86_64**
[source](https://github.com/containers/libpod/issues/4791)

```bash
sudo yum -y remove podman
# install all podman dependencies except podman-manpages
sudo yum -y install oci-systemd-hook libvarlink
# and install podman w/o dependencies
sudo rpm -Uvh --nodeps $(repoquery --location podman)
sudo yum -y install centos-release-stream
```


## network
[source](https://unix.stackexchange.com/questions/199966/how-to-configure-centos-7-firewalld-to-allow-docker-containers-free-access-to-th)

Fix error: No route to host

```bash
firewall-cmd --permanent --zone=trusted --change-interface=docker0
firewall-cmd --permanent --zone=trusted --add-port=4243/tcp
firewall-cmd --reload
```

## autoload

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