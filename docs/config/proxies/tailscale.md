# Tailscale

```{.yaml linenums="1"}
proxies:
  - name: "tailscale"
    type: tailscale
    hostname: mihomo
    auth-key: tskey-auth-xxxx
    control-url: https://controlplane.tailscale.com
    state-dir: ./tailscale
    ephemeral: false
    udp: true
    accept-routes: true
    exit-node: 100.64.0.1
    exit-node-allow-lan-access: true
    dialer-proxy: "ss1"
    interface-name: "WLAN"
    routing-mark: 6666
    ip-version: ipv4-prefer
```

## name

必须，代理名称，不可重复。

## type

必须，固定为 `tailscale`。

## hostname

可选，Tailscale 设备名。不填写时由 `tsnet` 处理。

## auth-key

可选，Tailscale 或 Headscale 的登录密钥。不填写时，首次启动会在日志中输出交互式登录 URL。

## control-url

可选，自定义 Tailscale control server 或 Headscale 地址。

## state-dir

可选，`tsnet` 状态目录，默认值为 `tailscale`。

## ephemeral

可选，是否作为 ephemeral node 登录，默认值为 `false`。

## udp

可选，是否启用 UDP，默认值为 `false`。

## accept-routes

可选，是否接受 Tailnet 中发布的 subnet routes。

## exit-node

可选，使用指定 exit node。可以填写节点 IP，也可以填写 `auto:any`。

当目标不在 Tailscale 路由内时，连接会直接报错，不会回退到直连。访问公网需要配置可用的 `exit-node`，或接受覆盖目标网段的 subnet routes。

## exit-node-allow-lan-access

可选，使用 exit node 时是否允许访问本地 LAN。

## dialer-proxy

可选，一个出站代理的标识。当值不为空时，将使用指定的 proxy 发出 Tailscale 控制面和 DERP/STUN 等连接。

## interface-name

可选，指定出站网卡。

## routing-mark

可选，Linux 下配置 fwmark。

## ip-version

可选，指定出站使用的 IP 版本。

可选值：`dual`/`ipv4`/`ipv6`/`ipv4-prefer`/`ipv6-prefer`。
