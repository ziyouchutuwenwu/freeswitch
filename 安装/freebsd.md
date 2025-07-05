# freebsd

## 说明

freebsd 直接带包，比较方便

## 步骤

### 下载

```sh
pkg install freeswitch
```

### 初始化配置

```sh
mkdir -p /usr/local/etc/freeswitch
cd /usr/local/share/examples/freeswitch/vanilla/ && pax -rw -p e . /usr/local/etc/freeswitch
```

### 修改基本配置

参考 [基本配置](../基本配置.md)

### 启动

```sh
freeswitch -nc -nonat
```

### shell

进入

```sh
fs_cli -rRS
```

退出

```sh
...
```

## 网络查看

```sh
sockstat -4l
```
