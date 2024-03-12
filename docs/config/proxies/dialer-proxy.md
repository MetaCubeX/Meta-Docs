# dialer-proxy

指定当前 `proxy` 通过下一跳的 `dialer-proxy` 建立网络连接，值可以为代理组、代理（proxy-groups, proxy）的同一 `name` 字段

```{.yaml linenums="1"}
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
- match,SS1

```

!!!Note
    上面的例子通过在客户端的 proxies 内 `SS1` 指定 `proxy-dialer: SS2`，使发往 SS1 的流量先经过 SS2，从而实现指定下一跳代理的效果

```mermaid
flowchart LR
  Clash <--> |proxy-proxy-dialer: SS2|SS2
  SS2 <--> SS1
  SS1 <--> 目标域名

```
