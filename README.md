# Termux Docker Stable

Docker Stable Termux Port


### Prerequisites

Custom Kernel with Docker's necessary options enabled, for GKI kernel reference [gki-custom](https://github.com/TapetalArray/gki-custom)

### Notice

All data and runtime paths are changed to /data/local/tmp/docker, Because the termux prefix path length exceeds the unix socket limit

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

#### Mount Cgroup

```bash
for cg in blkio cpu cpuacct cpuset devices freezer memory; do
   if [ ! -d "/sys/fs/cgroup/${cg}" ]; then
       sudo mkdir -p "/sys/fs/cgroup/${cg}"
   fi

   if ! sudo mountpoint -q "/sys/fs/cgroup/${cg}"; then
       sudo mount -t cgroup -o "${cg}" cgroup "/sys/fs/cgroup/${cg}" || true
   fi
done
```

#### Basic Environment

Add to your .bashrc or .profile

```bash
alias sudoe="sudo -E"
export DOCKER_HOST=unix:///data/local/tmp/docker/run/docker.sock
```

#### Start Daemon

Termux not use any init system, just manual start daemon

```bash
# containerd
sudo containerd &>/dev/null &
# dockerd
sudo dockerd &>/dev/null &
```

#### Run Hello World

```bash
sudo docker run Hello-world
sudo docker run busybox
```

#### Network

##### Host Network

Just add --network=host flages

```bash
sudo docker run --network something
```

##### Bridge

Edit iptable, change 192.168.1.1 according to your gateway IP

```bash
sudo ip route add default via 192.168.1.1 dev wlan0
sudo ip rule add from all lookup main pref 30000
```


### Build With Termux-Packages

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


## Also See
* [termux-docker-compose](https://github.com/TapetalArray/termux-docker-compose)
* [termux-lxd-stable](https://github.com/TapetalArray/termux-lxd-stable)


# Credits

* [docker](https://github.com/docker)
* [containerd](https://github.com/containerd/containerd)
* [runc](https://github.com/opencontainers/runc)
* [docker for android](https://gist.github.com/FreddieOliveira/efe850df7ff3951cb62d74bd770dce27)
* [termux-packages](https://github.com/termux/termux-packages)
