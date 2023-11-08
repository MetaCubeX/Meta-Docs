# WireGuard

## 简化写法

如果只有一个peer，可以使用简化写法。

```yaml
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

完整写法可以指定多个peer。

如果使用多个peer，每一个peer的`allowed-ips`需要做区分；此时顶层段落的`server, port, ip, ipv6, public-key, pre-shared-key, reserved`等字段均会被忽略，不过`private-key`仍然在顶层指定。

```yaml
  - name: "wg"
    type: wireguard
    private-key: eCtXsJZ27+4PbhDkHnB923tkUn2Gj59wZw5wFA75MnU=
    peers:
      - server: 162.159.192.1
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

## 字段解释

### name

代理名称,书写时请确保不会与其他代理节点重名

### type

代理类型,此处为 `wireguard`

### private-key

base64编码的Wireguard客户端私钥。可以使用`wg genkey`命令生成私钥，使用`wg pubkey <private-key>`命令获取对应公钥

### server

Wireguard服务端地址

### port

Wireguard服务端端口

### ip

本机在Wireguard网络中使用的IPv4地址

### ipv6

可选字段，本机在Wireguard网络中使用的IPv6地址

### public-key

base64编码的Wireguard服务端公钥

### allowed-ips

可选字段，限制客户端的哪些IP段的流量由服务端进行转发。一般情况下可填`['0.0.0.0/0']`

### pre-shared-key

可选字段，预共享密钥

### reserved

可选字段，Wireguard协议保留字段的值，部分WARP节点需要使用

### udp

可选字段，是否启用UDP支持

### mtu

可选字段，设置MTU值

### dialer-proxy

可选字段，一个出站代理的名称。当值不为空时，将使用指定的 proxy/proxy-group 发出连接。用于Wireguard配置链式代理

### remote-dns-resolve

可选字段，是否强制dns远程解析，默认值为false

### dns

可选字段，当`remote-dns-resolve`为true时生效，指定远程解析使用的dns服务器
