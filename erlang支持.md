# erlang 支持

## 步骤

### 编译的时候开启支持

间安装步骤

### 启动开启模块支持

```sh
vim autoload_configs/modules.conf.xml
```

```xml
<load module="mod_erlang_event"/>
```

### 配置 erlang 节点信息

```sh
vim autoload_configs/erlang_event.conf.xml
```

```xml
<configuration name="erlang_event.conf" description="Erlang Socket Client">
  <settings>
    <param name="listen-ip" value="0.0.0.0"/>
    <param name="listen-port" value="8031"/>
    <param name="nodename" value="fs@127.0.0.1"/>
    <param name="cookie" value="123456"/>
    <param name="shortname" value="false"/>
    <param name="encoding" value="binary"/>
  </settings>
</configuration>
```

## 调试

查看启动的 erl 节点

```sh
erl -name aaa@127.0.0.1 -setcookie 123456
net_kernel:connect_node('fs@127.0.0.1').
nodes(hidden).
```

查看所有的 erlang 节点

```sh
epmd -names
```

fs_cli 内查看状态

```sh
erlang status
```
