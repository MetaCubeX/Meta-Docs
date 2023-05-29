# 代理

## 通用字段

```yaml
proxies:
  - name: "ss"
    type: ss
    server: server
    port: 443
    ip-version: ipv4
    dialer-proxy: ss1
```

## proxies

代理节点书写以 `proxies`为开头,其内容为数组

### name

代理名称,书写时请确保不会与其他代理节点重名

### type

代理节点类型

### server

代理节点服务器,除 `tuic`外,其他代理节点类型的 `server`性质都相同

### port

代理节点端口

### ip-version

设置节点使用 IP 版本,可选: `dual，ipv4，ipv6，ipv4-prefer，ipv6-prefer`,默认使用 dual

ipv4: 仅使用 IPv4

ipv6: 仅使用 IPv6

ipv4-prefer : 优先使用 IPv4,对于 TCP 会进行双栈解析,并发链接但是优先使用 IPv4 链接,UDP 则为双栈解析,获取结果中的第一个 IPv4

ipv6-prefer:优先使用 IPv6,对于 TCP 会进行双栈解析,并发链接但是优先使用 IPv6 链接,UDP 则为双栈解析,获取结果中的第一个 IPv6

### dialer-proxy

指定当前 `proxy` 通过下一跳的 `dialer-proxy` 建立网络连接, 值可以为代理组、代理（proxy-groups, proxy）的同一 `name` 字段

```yaml
proxies:
  - name: "SS1"
    type: ss
    server: server
    port: 443
    dialer-proxy: SS2
    ...

  - name: "SS2"
    type: ss
    server: server
    port: 443
    ...

rules:
  match,SS1

```

!!!Note
    上面的例子通过在客户端的 proxies 内 `SS1` 指定 `proxy-dialer: SS2`，使发往 SS1 的流量先经过 SS2，从而实现指定下一跳代理的效果


```mermaid
flowchart LR
  Clash <--> |proxy-proxy-dialer: SS2|SS2
  SS2 <--> SS1
  SS1 <--> 目标域名

```
