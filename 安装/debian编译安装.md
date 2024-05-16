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
apt update; apt upgrade -y
```

### 安装编译依赖

```sh
apt build-dep freeswitch -y
```

```sh
apt install -y erlang
```

### 编译

下载

```sh
git clone https://github.com/signalwire/freeswitch.git -bv1.10 freeswitch --depth=1
cd freeswitch
git config pull.rebase true
```

先运行 bootstrap, 生成 modules.conf

```sh
./bootstrap.sh -j
```

开启 erlang 支持

```sh
sed -i 's|#event_handlers/mod_erlang_event|event_handlers/mod_erlang_event|g' modules.conf
```

make

```sh
./configure --prefix=/usr/local
make
sudo make install
```

### 运行

启动前参考[基本配置](../基本配置.md)

```sh
freeswitch -nc -nonat
fs_cli -H 10.0.2.15 -p 123456 -rRS

# 查看版本
fs_cli -x version
```
