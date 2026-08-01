# ZeroTier

```{.yaml linenums="1"}
proxies:
  - name: zerotier
    type: zerotier
    network: "0123456789abcdef"
    # state-dir: ./zerotier-node
    # planet: ./planet
    # mtu: 1400
    # physical-mtu: 1432
    # primary-port: 0
    # secondary-port: 0
    # tcp-fallback-mode: auto
    # tcp-fallback-relay: 204.80.128.1:443
    # remote-trace-target: "0123456789"
    # remote-trace-level: 0
    # low-bandwidth: false
    # encrypted-hello: false
    # orbit:
    #   - world: "0123456789abcdef"
    #     seed: "0123456789"
    udp: true
    # remote-dns-resolve: true
    # dns: [10.147.17.1]
    # dialer-proxy: "ss1"
    # interface-name: "WLAN"
    # routing-mark: 6666
    # ip-version: ipv4-prefer
```

[通用字段](./index.md)

## network

ZeroTier 网络 ID

## state-dir

可选，持久化节点身份；默认使用由网络 ID 和出站名称生成的独立目录

## planet

可选，私有 Planet 文件路径，用于替换内置的 Earth Planet

## mtu

可选，本地 MTU 覆盖值，该值不能超过 ZeroTier 控制器提供的 MTU

## physical-mtu

可选，ZeroTier UDP 负载 MTU，有效范围为 `510-10324`，默认值为 `1432`。

## primary-port

可选，主 UDP 端口，设置为 `0` 时自动选择可用端口。

## secondary-port

可选，第二 UDP 端口， `0` 为自动选择； `-1` 为禁用

## tcp-fallback-mode

可选，TCP 回退模式，可使用以下值：

* `auto`：UDP 连接失败后使用中继。
* `force`：仅使用中继。
* `disable`：禁用 TCP 中继回退。

默认值为 `auto`。

## tcp-fallback-relay

可选，TCP 回退中继服务器地址

## remote-trace-target

可选，用于接收全局诊断跟踪信息的 ZeroTier 节点 ID。

## remote-trace-level

可选，远程诊断跟踪级别，可使用以下值：

* `0`：普通
* `10`：详细
* `15`：规则
* `20`：调试
* `30`：详细

## low-bandwidth

可选，是否启用低带宽模式，默认值为 `false`。

## encrypted-hello

可选，默认值为 `false`。

## orbit

可选，配置私有 Moon 根服务器列表

### orbit-world

Moon World ID，必须为 16 位十六进制字符串。

### orbit-seed

Moon 根节点的 Node ID，必须为 10 位数字。

## udp

是否启用 UDP。默认值为 `true`。

## remote-dns-resolve

可选，是否通过 ZeroTier，使用控制器提供的 DNS 服务器解析目标域名。

## dns

可选，用于覆盖控制器提供的 DNS 服务器，可配置一个或多个 DNS 服务器。

## dialer-proxy

可选，通过其他出站代理承载 ZeroTier 的网络通信，其中 `ss1` 为其他代理节点的名称。

## interface-name

可选，指定 ZeroTier 使用的网络接口名称。

## routing-mark

可选，设置 Linux 路由标记。

## ip-version

可选，指定 IP 协议版本偏好。
