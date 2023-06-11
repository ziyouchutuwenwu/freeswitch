# erlang 支持

## 步骤

### 编译的时候开启支持

见安装步骤

### 启动开启模块支持

```sh
vim etc/freeswitch/autoload_configs/modules.conf.xml
```

```xml
<load module="mod_erlang_event"/>
```

### 配置 erlang 节点信息

```sh
vim etc/freeswitch/autoload_configs/erlang_event.conf.xml
```

```xml
<configuration name="erlang_event.conf" description="Erlang Socket Client">
  <settings>
    <param name="listen-ip" value="0.0.0.0"/>
    <param name="listen-port" value="8031"/>
    <param name="nodename" value="fs@192.168.56.11"/>
    <param name="cookie" value="123456"/>
    <!-- 短名字不支持 ip, 所以用 long name -->
    <param name="shortname" value="false"/>
    <param name="encoding" value="binary"/>
  </settings>
</configuration>
```

## 测试连接

查看启动的 erl 节点

```sh
erl -sname aaa@127.0.0.1 -setcookie 123456
net_kernel:connect_node('fs@192.168.56.11').
nodes(hidden).
```

## 演示代码

```elixir
{:elixir_mod_event, "~> 0.0.6"}
```

```elixir
defmodule Demo do
  alias FSModEvent.Connection, as: EslMode
  alias FSModEvent.Erlang, as: ErlangMode

  def prepare do
    :net_kernel.start([:"iex-dbg@192.168.56.1", :longnames])
    :erlang.set_cookie(node(), :"123456")
  end

  def esl_demo do
    case EslMode.start(:fs1, "192.168.56.11", 8021, "ClueCon") do
      {:ok, pid} ->
        # 不加 sleep 会出现先发 api,再发 auth 的情况
        Process.sleep(1000)
        EslMode.api(pid, "host_lookup", "baidu.com")

      _ ->
        IO.puts("连接失败")
    end
  end

  def erl_demo do
    node = :"fs@192.168.56.11"
    ErlangMode.api(node, "host_lookup", "baidu.com")
  end
end
```
