# erlang

## 说明

erlang 的支持，也能用于 elixir

## 步骤

### 编译期

见安装步骤

### 运行期

```sh
sudo sed -i '/mod_erlang_event/s/<!-- *\(.*mod_erlang_event.*\) *-->/\1/' /usr/local/etc/freeswitch/autoload_configs/modules.conf.xml
cat /usr/local/etc/freeswitch/autoload_configs/modules.conf.xml | rg mod_erlang_event
```

### 配置节点

配置的是 erlang 的节点

和 event socket 无关

```sh
/usr/local/etc/freeswitch/autoload_configs/erlang_event.conf.xml
```

```xml
<configuration name="erlang_event.conf" description="Erlang Socket Client">
  <settings>
    <param name="listen-ip" value="$${local_ip_v4}"/>
    <param name="listen-port" value="8031"/>
    <!-- longname 的 value 里面必须带 ip -->
    <param name="nodename" value="fs@10.0.2.15"/>
    <param name="cookie" value="123456"/>
    <!-- 短名字不支持 ip, 所以用 long name -->
    <param name="shortname" value="false"/>
    <param name="encoding" value="binary"/>
  </settings>
</configuration>
```

### 拨号计划

配置以后，打电话的时候，fs 会主调 erlang 节点

```sh
/usr/local/etc/freeswitch/dialplan/default/erlang.xml
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<include>
  <context name="erlang">
    <extension name="catch all extension">
      <condition field="destination_number" expression="^.*$">
        <action application="set" data="queue=${destination_number}"/>
        <action application="erlang" data="fs_call_sup:! iex@10.0.2.1"/>
      </condition>
    </extension>
  </context>
</include>
```

## 测试连接

查看启动的 erl 节点

```sh
erl -name aaa@127.0.0.1 -setcookie 123456
net_kernel:connect_node('fs@10.0.2.15').
nodes(hidden).
```

## 演示代码

```elixir
{:elixir_mod_event, "~> 0.0.6"}
```

```elixir
defmodule Demo do
  @local_node :"iex-dbg@10.0.2.1"

  alias FSModEvent.Connection, as: EslMode
  alias FSModEvent.Erlang, as: ErlangMode

  def prepare do
    :net_kernel.start([@local_node, :longnames])
    :erlang.set_cookie(node(), :"123456")
  end

  def event_socket_demo do
    case EslMode.start(:fs_event_socket, "10.0.2.15", 8021, "123456") do
      {:ok, pid} ->
        # 不加 sleep 会出现先发 api,再发 auth 的情况
        Process.sleep(1000)
        EslMode.api(pid, "host_lookup", "baidu.com")

      _ ->
        IO.puts("连接失败")
    end
  end

  def erl_demo do
    fs_node = :"fs@10.0.2.15"
    ErlangMode.api(fs_node, "host_lookup", "baidu.com")
  end
end
```
