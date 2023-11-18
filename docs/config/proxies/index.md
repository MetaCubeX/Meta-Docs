```yaml
proxies:
- name: "ss"
  type: ss
  server: server
  port: 443
  ip-version: ipv4
  udp: true
  interface-name: eth0
  routing-mark: 1234

  dialer-proxy: ss1
```

## proxies

代理节点书写以 `proxies`为开头,其内容为数组

### name

代理名称,书写时请确保不会与其他代理节点重名

### type

代理节点类型

### server

代理节点服务器(域名/ip)

### port

代理节点端口

### ip-version

非必须项,设置节点使用 IP 版本,可选: `dual，ipv4，ipv6，ipv4-prefer，ipv6-prefer`,默认使用 dual

* ipv4: 仅使用 IPv4
* ipv6: 仅使用 IPv6
* ipv4-prefer : 优先使用 IPv4,对于 TCP 会进行双栈解析,并发链接但是优先使用 IPv4 链接,UDP 则为双栈解析,获取结果中的第一个 IPv4
* ipv6-prefer:优先使用 IPv6,对于 TCP 会进行双栈解析,并发链接但是优先使用 IPv6 链接,UDP 则为双栈解析,获取结果中的第一个 IPv6

### udp

非必须项,是否允许UDP通过代理

!!! note
    此选项在 TUIC 等基于 UDP 的协议默认开启

### interface-name

非必须项,指定节点绑定的接口,从此接口发起连接

### routing-mark

非必须项,节点发起连接时附加的路由标记
