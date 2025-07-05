# debian

## 说明

编译安装，因为要带 erlang

## 步骤

### 配置源

```sh
TOKEN=pat_1X8EQXH6EvgajWaBVWSJCG51

apt update && apt install -y curl
curl -sSL https://freeswitch.org/fsget | bash -s $TOKEN
```

### 编译依赖

```sh
apt build-dep freeswitch -y
```

```sh
apt install -y erlang
```

### 编译

下载

```sh
cd /usr/src/
# git clone https://github.com/signalwire/freeswitch.git -bv1.10 freeswitch --depth=1
git clone -b v1.10 https://github.com/signalwire/freeswitch.git --depth=1

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
cat modules.conf | rg mod_erlang_event
```

make

```sh
./configure --prefix=/usr/local
make
sudo make install
```

### 运行

运行前需要先配置

```sh
# 需要 root
freeswitch -nc -nonat

# 连接
fs_cli -H 10.0.2.15 -p 123456 -rRS

# 查看版本
fs_cli -H 10.0.2.15 -p 123456 -x version
```
