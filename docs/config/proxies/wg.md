# WireGuard

## 简化写法

如果只有一个 peer，可以使用简化写法。

```{.yaml linenums="1"}
proxies:
- name: "wg"
  type: wireguard
  private-key: eCtXsJZ27+4PbhDkHnB923tkUn2Gj59wZw5wFA75MnU=
  server: 162.159.192.1
  port: 2480
  ip: 172.16.0.2
  ipv6: fd01:5ca1:ab1e:80fa:ab85:6eea:213f:f4a5
  public-key: Cr8hWlKvtDt7nrvf+f0brNQQzabAqrjfBvas9pmowjo=
  allowed-ips: ['0.0.0.0/0']
  # pre-shared-key: 31aIhAPwktDGpH4JDhA8GNvjFXEf/a6+UaQRyOAiyfM=
  # reserved: [209,98,59]  # 字符串格式也是合法的，如"U4An"
  udp: true
  # mtu: 1408
  # dialer-proxy: "ss1"  # 一个出站代理的标识。当值不为空时，将使用指定的 proxy/proxy-group 发出连接
  # remote-dns-resolve: true # 强制dns远程解析，默认值为false
  # dns: [ 1.1.1.1, 8.8.8.8 ] # 仅在remote-dns-resolve为true时生效
```

## 完整写法

完整写法可以指定多个 peer。

如果使用多个 peer，每一个 peer 的`allowed-ips`需要做区分；此时顶层段落的`server, port, public-key, pre-shared-key, reserved`等字段均会被忽略，不过`private-key`仍然在顶层指定。

```{.yaml linenums="1"}
proxies:
- name: "wg"
  type: wireguard
  ip: 172.16.0.2
  ipv6: fd01:5ca1:ab1e:80fa:ab85:6eea:213f:f4a5
  private-key: eCtXsJZ27+4PbhDkHnB923tkUn2Gj59wZw5wFA75MnU=
  peers:
    - server: 162.159.192.1
      port: 2480
      public-key: Cr8hWlKvtDt7nrvf+f0brNQQzabAqrjfBvas9pmowjo=
      allowed-ips: ['0.0.0.0/0']
      # pre-shared-key: 31aIhAPwktDGpH4JDhA8GNvjFXEf/a6+UaQRyOAiyfM=
      # reserved: [209,98,59]  # 字符串格式也是合法的，如"U4An"
  udp: true
  # mtu: 1408
  # dialer-proxy: "ss1"  # 一个出站代理的标识。当值不为空时，将使用指定的 proxy/proxy-group 发出连接
  # remote-dns-resolve: true # 强制dns远程解析，默认值为false
  # dns: [ 1.1.1.1, 8.8.8.8 ] # 仅在remote-dns-resolve为true时生效
```

[通用字段](./index.md)

### ip

本机在 Wireguard 网络中使用的 IPv4 地址

### ipv6

可选字段，本机在 Wireguard 网络中使用的 IPv6 地址

### private-key

base64 编码的 Wireguard 客户端私钥

可以使用`wg genkey | tee privatekey | wg pubkey > publickey`命令生成一对可用的公私钥文件

### public-key

base64 编码的 Wireguard 服务端公钥

### allowed-ips

可选字段，限制客户端的哪些 IP 段的流量由服务端进行转发。一般情况下可填`['0.0.0.0/0']`

### pre-shared-key

可选字段，预共享密钥

### reserved

可选字段，Wireguard 协议保留字段的值，部分 WARP 节点需要使用

### mtu

可选字段，设置 MTU 值

### remote-dns-resolve

可选字段，是否强制 dns 远程解析，默认值为 false

### dns

可选字段，当`remote-dns-resolve`为 true 时生效，指定远程解析使用的 dns 服务器

## 从 Wireguard 标准配置文件翻译

假设有如下 Wireguard 标准配置文件：

```ini
[Interface]
Address = <本机组网IP>
ListenPort = <本地监听端口>
PrivateKey = <本机私钥>
DNS = <使用的DNS>
MTU = <预设MTU>

[Peer]
AllowedIPs = <转发IP段>
Endpoint = <远端地址>:<远端端口>
PublicKey = <远端公钥>
```

对应的 clash 节点配置为：

```{.yaml linenums="1"}
- name: "wg"
  type: wireguard
  ip: <本机组网IP，IPv4往这里填>
  ipv6: <本机组网IP，IPv6往这里填>  # 没有v6地址直接删除
  private-key: <本机私钥>
  peers:
    - server: <远端地址>
      port: <远端端口>
      public-key: <远端公钥>
      allowed-ips: ['0.0.0.0/0']     # 分流由clash处理
      # reserved: [209,98,59]        # 如果需要自己填
  udp: true
  mtu: <预设MTU>               # 按需设置，不用直接删除
  remote-dns-resolve: true    # 按需设置，不用直接删除
  dns: <使用的DNS>             # 按需设置，不用直接删除
```
