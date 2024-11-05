# Termux Docker Stable

Docker Stable Termux Port


### Prerequisites

Custom Kernel with Docker's necessary options enabled

### Notice

All data and runtime paths are changed to /data/local/tmp, Because the termux prefix path length exceeds the unix socket limit

Status:

|            |        |
|:----------:|:------:|
| docker     | 27.3.1 |
| containerd | 1.7.23 |
| runc       | 1.2.1  |


### Install

```bash
pkg upg
pkg i tsu
pkg i ./*.deb
```


### Usage

#### Basic Environment

Add to your .bashrc or .profile

```bash
alias sudo="sudo -E"
export DOCKER_HOST=unix:///data/local/tmp/docker/run/docker.sock
```

#### Start Daemon

Termux not use any init system, just manual start daemon

```bash
# containerd
sudo containerd $>/dev/null &
# dockerd
sudo dockerd $>/dev/null &
```


### Network

#### Host Network

Just add --network=host flages

```bash
sudo docker run --network something
```

#### Bridge

Edit iptable, change 192.168.1.1 according to your gateway IP

```bash
sudo ip route add default via 192.168.1.1 dev wlan0
sudo ip rule add from all lookup main pref 30000
```

#### Run Hello World or busybox



```bash
sudo docker run Hello-world
sudo docker run busybox
```


### Build Use Termux-Packages

#### Clone Repo

```bash
git clone https://github.com/TapetalArray/termux-docker-stable
```

#### Add Packages

```bash
cp -r termux-docker-stable/packages/* termux-packages/packages
```

#### Build

```bash
./build-package.sh -i -a aarch64 docker-stable
```


# Credits

* [docker](https://github.com/docker)
* [containerd](https://github.com/containerd/containerd)
* [runc](https://github.com/opencontainers/runc)
* [docker for android](https://gist.github.com/FreddieOliveira/efe850df7ff3951cb62d74bd770dce27)
* [termux-packages](https://github.com/termux/termux-packages)
