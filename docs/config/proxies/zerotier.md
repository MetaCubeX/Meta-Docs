# ZeroTier

```{.yaml linenums="1"}
proxies:
- name: "zerotier"
  type: zerotier
  network: "0123456789abcdef"
  state-dir: ./zerotier-node
  planet: ./planet
  mtu: 1400
  physical-mtu: 1432
  primary-port: 0
  secondary-port: 0
  tcp-fallback-mode: auto
  tcp-fallback-relay: 204.80.128.1:443
  remote-trace-target: "0123456789"
  remote-trace-level: 0
  low-bandwidth: false
  encrypted-hello: false
  orbit:
    - world: "0123456789abcdef"
      seed: "0123456789"
  udp: true
  remote-dns-resolve: true
  dns: [10.147.17.1]
  dialer-proxy: "ss1"
  interface-name: "WLAN"
  routing-mark: 6666
  ip-version: ipv4-prefer
```

[通用字段](./index.md)

## network

必填，16 位十六进制 ZeroTier Network ID。

## state-dir

可选，用于持久化节点身份的目录。默认在 `zerotier` 目录下为每个 Network ID 和出站名称创建隔离目录。

## planet

可选，自定义 planet 文件路径。设置后会替代内置的 Earth planet。

## mtu

可选，本地 MTU 上限，取值范围为 `1280` 至 `10000`。实际 MTU 不会超过控制器下发的值。

## physical-mtu

可选，ZeroTier UDP 数据包的 MTU，取值范围为 `510` 至 `10324`，默认 `1432`。

## primary-port

可选，主 UDP 端口，取值范围为 `0` 至 `65535`。`0` 表示自动选择可用端口。

## secondary-port

可选，第二个 UDP 端口，取值范围为 `-1` 至 `65535`。`0` 表示自动选择可用端口，`-1` 表示禁用第二个端口。

## tcp-fallback-mode

可选，TCP 回退模式，默认 `auto`。

可选值：

- `auto`：UDP 直连失败后使用 TCP relay。
- `force`：仅使用 TCP relay。
- `disable`：禁用 TCP relay。

## tcp-fallback-relay

可选，TCP 回退 relay 地址，格式为 `host:port`。默认使用 ZeroTier 提供的公共 relay。

## remote-trace-target

可选，接收全局诊断 trace 的 10 位十六进制 ZeroTier node ID。

## remote-trace-level

可选，远程诊断 trace 级别，取值范围为 `0` 至 `30`。

可选值：`0`（normal）/`10`（verbose）/`15`（rules）/`20`（debug）/`30`（insane）。

## low-bandwidth

可选，是否降低后台流量和网络配置刷新频率，默认 `false`。

## encrypted-hello

可选，是否为发出的 HELLO 数据包启用 protocol-13 扩展加密，默认 `false`。

## orbit

可选，联邦根节点（moon）列表。

### orbit.world

必填，16 位十六进制 moon world ID。

### orbit.seed

必填，moon 根节点的 10 位十六进制 node ID。

## remote-dns-resolve

可选，是否通过 ZeroTier 使用控制器下发的 DNS 服务器解析目标域名，默认 `false`。

## dns

可选，仅在 `remote-dns-resolve` 为 `true` 时生效。设置后会覆盖控制器下发的 DNS 服务器。
