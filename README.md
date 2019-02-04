Docker images to support network configuration
==============================================

# Introduction
[That project](https://github.com/network-conf-helpers/docker-images)
produces Docker images, hosted on [dedicated
public Docker Cloud site](https://cloud.docker.com/u/netconfhelpers/repository/docker/netconfhelpers/base).
Those Docker images are intended to bring Linux-based ready-to-use environment
for DevOps training in network configuration. Changing network configuration
on remote servers or virtual machines is difficult, as any error may
make the server unusable or unreachable for a while, with lengthy recovery
procedures.
The network configuration revolves around setting up (virtual) bridges,
bonds, VLAN, sub-networks, private networks, DMZ, as well as firewalls.

The supported Linux distribution is currently
[Debian 9 (Stretch)](https://www.debian.org/releases/stretch/),
as that is the one on which [Proxmox](https://pve.proxmox.com) relies.
Some other distributions may be supported in the future, for instance
[CentOS 7](https://wiki.centos.org/Manuals/ReleaseNotes/CentOS7),
[Ubuntu 18.10 (Cosmic Cuttlefish)](http://releases.ubuntu.com/18.10/)
and [Ubuntu 18.04 LTS (Bionic Beaver)](http://releases.ubuntu.com/18.04/).

Every time some changes are committed on the [project's GitHub
repository](https://github.com/network-conf-helpers/docker-images),
the [Docker images are automatically
rebuilt](https://cloud.docker.com/u/netconfhelpers/repository/docker/netconfhelpers/base/timeline)
and pushed onto Docker Cloud.

The preferred way to propose amendment of the Docker images is through
[pull requests on the GitHub
project](https://github.com/network-conf-helpers/docker-images/pulls).
Once the pull request has been merged, _i.e._, once the `Dockerfile` amendment
has been [committed in
GitHub](https://github.com/network-conf-helpers/docker-images/commits/master),
Docker Cloud then rebuilds the corresponding Docker images, which become
available for every one to use.

# Images on Docker Cloud
* [Docker Cloud dashboard](https://cloud.docker.com/u/netconfhelpers/repository/docker/netconfhelpers/base)

# Using the pre-built training images
* Start the Docker container featuring the target Linux distribution
  (`<linux-distrib>` being currently `debian9`):
```bash
$ docker pull netconfhelpers/base:<linux-distrib>
$ docker run --rm -v ~/.ssh/id_rsa:/home/build/.ssh/id_rsa -v ~/.ssh/id_rsa.pub:/home/build/.ssh/id_rsa.pub -it netconfhelpers/base:<linux-distrib>
root@5..0:~# 
```

* Setup the user name and email address for Git:
```bash
root@5..0:~# git config --global user.name "Firstname Lastname"
root@5..0:~# git config --global user.email "email@example.com"
```

* Clone some network configuration helper project (_e.g._,
  [Network helper for Proxmox](https://github.com/network-conf-helpers/proxmox)
  is used as an example here):
```bash
root@5..0:~# mkdir -p ~/dev && cd ~/dev
root@5..0:~/dev# git clone https://github.com/network-conf-helpers/proxmox
Cloning into 'proxmox'...
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 9 (delta 2), reused 9 (delta 2), pack-reused 0
Unpacking objects: 100% (9/9), done.
root@5..0:~# cd proxmox
✔ ~/dev/proxmox [master|✔] 
17:23 # ls -laFh --color etc/systemd/network/
```

# Customize a Docker Image
The images may be customized, and pushed to Docker Cloud;
`<linux-distrib>` being currently `debian9`:
```bash
$ mkdir -p ~/dev
$ cd ~/dev
$ git clone https://github.com/network-conf-helpers/docker-images.git network-conf-docker-images
$ cd network-conf-docker-images
$ vi <linux-distrib>/Dockerfile
$ docker build -t netconfhelpers/<linux-distrib>:beta --squash <linux-distrib>/
$ docker run --rm -v ~/.ssh/id_rsa:/home/build/.ssh/id_rsa -v ~/.ssh/id_rsa.pub:/home/build/.ssh/id_rsa.pub -it netconfhelpers/<linux-distrib>:beta
root@5..0:~# exit
$ docker push netconfhelpers/<linux-distrib>:beta
```

# TODO
For any of the following features, an issue may be open
[on GitHub](https://github.com/network-conf-helpers/docker-images/issues):
1. Support other Linux distributions, for instance CentOS 7, Fedora
   or Ubuntu
2. Automate regular rebuilds (_e.g._, once a month)
3. Write scripts for typical stacks (_e.g._, Nginx, Squid, OpenVZ)

