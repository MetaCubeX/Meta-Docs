流量转发隧道,可以转发tcp/udp流量,也可经过代理转发

```{.yaml linenums="1"}
tunnels:
- tcp/udp,127.0.0.1:6553,114.114.114.114:53,proxy
- network: [tcp, udp]
  address: 127.0.0.1:7777
  target: target.com
  proxy: proxy
```

## 单行

```{.yaml linenums="1"}
tunnels:
- tcp/udp,127.0.0.1:6553,8.8.8.8:53,proxy
```

## 多行

```{.yaml linenums="1"}
tunnels:
- network: [tcp, udp]
  address: 127.0.0.1:6553
  target: 8.8.8.8:53
  proxy: proxy
```

单行顺序分别对应多行的 `network`/`address`/`target`/`proxy`

### network

需要监听的网络类型,可为tcp/udp

### address

本地监听地址

### target

转发的目标地址

### proxy

可选项,经过某个 `proxies`/`proxy-groups` 发送流量

如上示例为   访问`127.0.0.1:6553`为经过`proxy`这个`proxies`/`proxy-groups`访问`8.8.8.8:53`
