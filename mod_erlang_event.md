# mod_erlang_event.md

## 步骤

### 编译的时候开启支持

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
<settings>
  <param name="listen-ip" value="0.0.0.0"/>
  <param name="listen-port" value="8031"/>
  <param name="nodename" value="fs"/>
  <param name="cookie" value="111111"/>
  <param name="shortname" value="false"/>
  <param name="encoding" value="binary"/>
</settings>
```
