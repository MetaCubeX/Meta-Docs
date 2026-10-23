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
    port: 41641
    accept-routes: true
    advertise-routes:
      - 192.168.1.0/24
    exit-node: 100.64.0.1
    exit-node-allow-lan-access: true
    dialer-proxy: "ss1"
    interface-name: "WLAN"
    routing-mark: 6666
    ip-version: ipv4-prefer
```

!!! note
    和其他代理协议一样，tailscale 只是一个可选代理节点。只有当你通过规则、策略组或其他路由方式把流量指向这个节点时，对应流量才会走 Tailscale。
    
    此外，所有类型的 proxy 都只会在其匹配上的第一个链接触发的时候才会开始链接（即并不会随 Mihomo 同步启动），所以第一次访问 Tailscale 节点超时是正常情况，请多次尝试直到 Tailscale 正常加载完成。

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

## port

可选，Tailscale 节点通信的 UDP 监听端口，默认值为 `41641`。设为 `0` 时使用随机端口。如果防火墙阻止入站 UDP，请放行实际监听端口以便建立节点直连。此项独立于 `udp` 业务流量选项。

## accept-routes

可选，是否接受 Tailnet 中发布的 subnet routes。

## advertise-routes

可选，把本机可达网段发布到 Tailnet，供其他节点经此节点访问。

```yaml
advertise-routes:
  - 192.168.1.0/24
  - fd12:3456:789a::/64
```

路由需要在 Tailscale 或 Headscale 控制台批准后，其他节点才会使用。不填写时不会改动已经保存的路由；设为 `[]` 会清除已发布的路由。

前缀会归一化到网段地址，例如 `192.168.1.5/24` 会按 `192.168.1.0/24` 发布。非法 CIDR 或重复网段会在加载配置时直接报错。

同时发布 `0.0.0.0/0` 和 `::/0` 表示作为 exit node，此时不能再配置 `exit-node`。

进入这些网段的 TCP/UDP 由 userspace 网络栈从本机转发出去，沿用 `interface-name`、`routing-mark` 和 `dialer-proxy`。

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
