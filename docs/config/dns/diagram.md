# 解析流程

## 示例配置

```{.yaml linenums="1"}
dns:
  nameserver:
    - https://doh.pub/dns-query
  fallback:
    - https://8.8.8.8/dns-query
  direct-nameserver:
    - system
  nameserver-policy:
    "geosite:cn,private":
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  fallback-filter:
    geoip: true
    geoip-code: CN
    geosite:
      - gfw
    ipcidr:
      - 240.0.0.0/4
    domain:
      - '+.google.com'
      - '+.facebook.com'
      - '+.youtube.com'

rules:
- DOMAIN-SUFFIX,google.com,PROXY
- GEOIP,CN,DIRECT
- MATCH,PROXY
```

## 流程

!!! note ""
    此部分仅说明 dns 模块的处理过程

!!! warning ""
    ~~direct-nameserver 重新解析仅限 tcp 连接~~
    direct-nameserver 重新解析在v1.19.10后同样应用于 UDP 连接（对于Tun入站仅限Fakeip模式下）

```mermaid
flowchart TD
  Rule[匹配规则]
  Domain[匹配到域名规则]
  IP[匹配到目标 IP 规则]
  Resolve[解析域名]

  NameServer[使用 nameserver 查询]
  Policy[匹配 nameserver-policy]
  Concurrent[使用 nameserver 和 fallback 并发查询]
  Filter[匹配 fallback-filter]
  DirectNS[使用 direct-nameserver 重新解析]

  GetIP[查询得到IP]

  Proxy[发送域名给代理]
  Direct[使用 IP 直接连接]

  Rule -->  Domain
  Rule --> IP

  Domain -- 域名匹配到直连 --> Resolve
  Domain -- 域名匹配到直连并配置了 direct-nameserver --> DirectNS

  IP --> Resolve

  Resolve -- 配置了 nameserver-policy --> Policy
  Policy -- 未匹配到 --> NameServer
  Policy --> GetIP
  Resolve -- 未配置 nameserver-policy --> NameServer

  NameServer -- 配置了 fallback --> Concurrent
  Concurrent --> Filter
  Filter --> GetIP
  NameServer -- 未配置 fallback --> GetIP

  GetIP -- 匹配到直连并配置了 direct-nameserver --> DirectNS
  DirectNS --> Direct

  GetIP -- IP 匹配到代理 --> Proxy[发送域名给代理]
  Domain -- 域名匹配到代理 --> Proxy

  GetIP -- IP 匹配到直连 --> Direct
```
