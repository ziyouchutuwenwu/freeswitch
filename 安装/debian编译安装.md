# debian 编译安装

## 步骤

### 配置源

```sh
TOKEN=pat_1X8EQXH6EvgajWaBVWSJCG51

apt update && apt install -yq gnupg2 wget lsb-release
wget --http-user=signalwire --http-password=$TOKEN -O /usr/share/keyrings/signalwire-freeswitch-repo.gpg https://freeswitch.signalwire.com/repo/deb/debian-release/signalwire-freeswitch-repo.gpg

echo "machine freeswitch.signalwire.com login signalwire password $TOKEN" > /etc/apt/auth.conf
chmod 600 /etc/apt/auth.conf
echo "deb [signed-by=/usr/share/keyrings/signalwire-freeswitch-repo.gpg] https://freeswitch.signalwire.com/repo/deb/debian-release/ `lsb_release -sc` main" > /etc/apt/sources.list.d/freeswitch.list
echo "deb-src [signed-by=/usr/share/keyrings/signalwire-freeswitch-repo.gpg] https://freeswitch.signalwire.com/repo/deb/debian-release/ `lsb_release -sc` main" >> /etc/apt/sources.list.d/freeswitch.list
apt update
```

### 安装编译依赖

```sh
apt build-dep freeswitch
```

```sh
apt install -y erlang
```

### 编译

下载

```sh
git clone https://github.com/signalwire/freeswitch.git -bv1.10 freeswitch
cd freeswitch
git config pull.rebase true
```

配置

```sh
./bootstrap.sh -j
./configure --prefix=/home/mmc/fs_bin
```

开启 erlang 的支持

```sh
vim modules.conf
event_handlers/mod_erlang_event
```

make

```sh
make
make install
```
