# EasyTier

```{.yaml linenums="1"}
proxies:
  - name: "easytier"
    type: easytier
    network-name: example
    network-secret: secret
    # hostname: mihomo
    # ipv4: 10.144.0.1/24
    # dhcp: true
    peers:
      - tcp://192.0.2.10:11010
      - udp://192.0.2.11:11010
    # secure-mode: true
    # local-private-key: "base64-x25519-private-key"
    # local-public-key: "base64-x25519-public-key"
    # listeners: ["tcp://0.0.0.0:11010"]
    # no-listener: true
    # mapped-listeners: ["tcp://203.0.113.10:11010"]
    # exit-nodes: ["10.144.0.1"]
    # proxy-networks: ["10.0.0.0/24"]
    # instance-name: mihomo-easytier
    # state-dir: ./easytier
    udp: true
    # accept-dns: false
    # enable-exit-node: false
    # enable-encryption: true
    # encryption-algorithm: aes-gcm
    # private-mode: false
    # latency-first: false
    # disable-p2p: false
    # enable-kcp-proxy: false
    # disable-kcp-input: false
    # enable-quic-proxy: false
    # disable-quic-input: false
    # mtu: 1380
    # tld-dns-zone: et.net.
    # dialer-proxy: "ss1"
    # interface-name: "WLAN"
    # routing-mark: 6666
    # ip-version: ipv4-prefer
```

!!! note
    EasyTier 的 overlay 网络仅支持 IPv4，`ip-version` 只影响底层 peer 连接，不会启用 overlay IPv6。

    和其他代理类型一样，只有当流量通过规则、策略组等方式指向该节点时，EasyTier 才会开始启动。

[通用字段](./index.md)

## name

必须，代理名称，不可重复。

## type

必须，固定为 `easytier`。

## network-name

必须，EasyTier 网络名称。

## network-secret

可选，EasyTier 网络密钥。相同网络名称和密钥的节点属于同一虚拟网络。

## hostname

可选，本节点在 overlay 网络中的主机名，用于 MagicDNS 解析。不填写时由 EasyTier 处理。

## ipv4

可选，本节点的 overlay IPv4 地址，CIDR 格式，如 `10.144.0.1/24`。

## dhcp

可选，是否通过 DHCP 获取 overlay IPv4 地址。`ipv4` 为空时默认开启，否则默认关闭；未获取到时实例将没有 overlay IPv4。

## peers

可选，初始连接的入口节点列表，可配置多个，支持 `tcp://`、`udp://` 等协议 URI。

默认不会隐式连接 `public.easytier.top`；`listeners` 为空时至少需要配置一个。

在 URI 中追加 `peer-public-key` 查询参数可锁定共享节点的公钥，防止中间人攻击：

```{.yaml linenums="1"}
peers:
  - "tcp://relay.example.com:11010?peer-public-key=base64-x25519-public-key"
```

## secure-mode

可选，是否启用 Noise 端到端加密。配置了 `local-private-key`、`local-public-key` 或 peer URI 中的 `peer-public-key` 时会自动开启。

## local-private-key

可选，本机 X25519 私钥（Base64），用于固定本机身份，避免每次启动更换公钥。

## local-public-key

可选，本机 X25519 公钥（Base64），通常可由私钥派生；必须与 `local-private-key` 一同配置。

## listeners

可选，本节点监听的 URI 列表，用于接受其他节点的连接，如 `tcp://0.0.0.0:11010`。

## no-listener

可选，是否禁用监听，默认值为 `true`（即 `listeners` 为空）。不能与 `listeners` 同时启用；显式设置为 `false` 且 `listeners` 为空时，使用默认监听地址 `tcp://0.0.0.0:11010`。

## mapped-listeners

可选，向其他节点公告的对外映射监听地址列表，适用于端口映射等场景，如 `tcp://203.0.113.10:11010`。

## exit-nodes

可选，要使用的 exit node 列表（填写对端节点的 overlay IPv4 地址），用于将流量转发至指定节点出网。

## proxy-networks

可选，需要通过 EasyTier 网络代理访问的远端子网（CIDR）列表，如 `10.0.0.0/24`。

## instance-name

可选，EasyTier 实例名称，默认值为代理名称。

## state-dir

可选，状态目录，默认值为 `easytier/<代理名称>`，用于持久化 `instance_id`。

## udp

可选，是否启用 UDP，默认值为 `false`。

## accept-dns

可选，是否接受网络下发的 DNS 配置。

## enable-exit-node

可选，是否作为其他节点使用的 exit node，默认值为 `false`。

## enable-encryption

可选，是否启用节点间加密，默认值为 `true`。

## encryption-algorithm

可选，加密算法，默认值为 `aes-gcm`。

## private-mode

可选，是否启用隐私模式，默认值为 `false`。

## latency-first

可选，是否启用延迟优先模式（优先选择延迟最低的路径而非跳数最少的路径），默认值为 `false`。

## disable-p2p

可选，是否禁用 P2P 连接（仅通过中继通信），默认值为 `false`。

## enable-kcp-proxy

可选，是否启用 KCP 代理（将 TCP 流量经 KCP 转发），默认值为 `false`。

## disable-kcp-input

可选，是否禁用 KCP 输入，默认值为 `false`。

## enable-quic-proxy

可选，是否启用 QUIC 代理（将 TCP 流量经 QUIC 转发），默认值为 `false`。

## disable-quic-input

可选，是否禁用 QUIC 输入，默认值为 `false`。

## mtu

可选，MTU 覆盖值，默认值为 `1380`。

## tld-dns-zone

可选，MagicDNS 使用的 TLD DNS 区域，默认值为 `et.net.`。

在 DNS 配置中可使用 `et://<代理名称>` 解析 overlay 网络中的 A/PTR 记录，建议配合 `nameserver-policy` 使用。

## dialer-proxy

可选，通过其他出站代理承载 EasyTier 的网络通信，其中 `ss1` 为其他代理节点的名称。

## interface-name

可选，指定 EasyTier 使用的网络接口名称。

## routing-mark

可选，设置 Linux 路由标记。

## ip-version

可选，指定 IP 协议版本偏好（只影响底层 peer 连接）。
