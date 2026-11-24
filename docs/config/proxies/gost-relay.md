# GOST Relay

```{.yaml linenums="1"}
proxies:
  - name: "gost-relay-hop"
    type: gost-relay
    server: relay.example.com
    port: 443
    udp: true
    tls: true
    # mux: true
    # ws: true
    # ws-opts:
    #   path: /ws
    #   headers:
    #     Host: relay.example.com
    # sni: relay.example.com
    # username: user
    # password: pass
    # client-fingerprint: chrome
    # fingerprint: xxxx
    # certificate: ./client.crt
    # private-key: ./client.key
    # skip-cert-verify: true
    # name-cert-verify: example.com
    # dialer-proxy: "ss1"
    # interface-name: "WLAN"
    # routing-mark: 6666
    # ip-version: ipv4-prefer
```

!!! note
    GOST Relay 用于通过一台 GOST 中继服务器承载其他代理（如 ss/vmess/vless/trojan 等）。把它作为上层代理的 `dialer-proxy`，上层代理的连接与 UDP 流量都会经由该中继发出。

    支持两种模式：

    * **动态模式**（默认，`forward: false`）：中继按照上层代理请求的目标地址发起连接。此时上层代理的 `server` 通常填写中继服务器侧的可达地址（例如中继服务器本机的 `127.0.0.1`）。
    * **转发模式**（`forward: true`）：由中继服务器按其配置好的转发目标建立连接，上层代理的地址仅用于从中继取得一条连接。

    和其他代理类型一样，只有当流量通过规则、策略组等方式指向该节点时，GOST Relay 才会开始连接。

[通用字段](./index.md)

## name

必须，代理名称，不可重复。

## type

必须，固定为 `gost-relay`。

## server

必须，GOST 中继服务器地址。

## port

必须，GOST 中继服务器端口，范围 1–65535。

## forward

可选，是否使用转发模式，默认值为 `false`（动态模式）。

* `false`（动态模式）：中继按照上层代理请求的目标地址发起连接，需配合 `dialer-proxy` 承载其他出站代理。
* `true`（转发模式）：由中继服务器连接到其配置好的转发目标，上层代理的地址仅用于向中继请求一条连接。

## udp

可选，是否启用 UDP，默认值为 `false`。

## ws

可选，是否启用 WebSocket 传输，默认值为 `false`。

与 `tls`、`mux` 组合可形成多种中继传输方式：

* `ws` + `tls`：`relay+wss`
* `ws` + `mux`：`relay+mws`
* `ws` + `tls` + `mux`：`relay+mwss`

## ws-opts

可选，WebSocket 传输选项。

### path

可选，WebSocket 路径，GOST 默认值为 `/ws`，需与中继监听器的 `ws.path` 保持一致。

### headers

可选，WebSocket 请求头，键值为字符串。常在此处设置 `Host` 头，例如 `Host: relay.example.com`。

```{.yaml linenums="1"}
ws-opts:
  path: /ws
  headers:
    Host: relay.example.com
```

## tls

可选，是否启用 TLS，默认值为 `false`。

## mux

可选，是否启用多路复用（smux），默认值为 `false`。在 `tls` 开启且 `ws` 关闭时，对应 `relay+mtls`。

## sni

可选，TLS SNI。默认取 `ws-opts.headers` 中的 `Host`，若未设置则取 `server`。

## username

可选，中继认证用户名。

## password

可选，中继认证密码。

## skip-cert-verify

可选，是否跳过服务端证书校验，默认值为 `false`。

## name-cert-verify

可选，仅修改证书 DNSName 的校验目标，不修改 SNI。

## fingerprint

可选，服务端证书的 SHA256 指纹，用于校验证书。

## certificate

可选，客户端证书文件路径，用于 mTLS。

## private-key

可选，客户端私钥文件路径，用于 mTLS。

## client-fingerprint

可选，客户端 TLS 指纹（如 `chrome`）。

## dialer-proxy

可选，通过其他出站代理承载到 GOST 中继的连接，其中 `ss1` 为其他代理节点的名称。

动态模式下，把 GOST Relay 作为上层代理的 `dialer-proxy`，即可让上层代理（如 ss/vmess/vless/trojan 等）的流量经由中继发出。

## interface-name

可选，指定出站网卡。

## routing-mark

可选，Linux 下配置 fwmark。

## ip-version

可选，指定出站使用的 IP 版本。

可选值：`dual`/`ipv4`/`ipv6`/`ipv4-prefer`/`ipv6-prefer`。
