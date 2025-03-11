以下是两种常见的 DNS 配置：

```{.yaml linenums="1"}
hosts:
  'alpha.clash.dev': '::1'

dns:
  ...
  ipv6: true
  enhanced-mode: redir-host / fake-ip
  fake-ip-range: 28.0.0.1/8
  fake-ip-filter:
    - '*'
    - '+.lan'
  nameserver:
    - https://doh.pub/dns-query
  fallback:
    - https://8.8.8.8/dns-query
  nameserver-policy:
    "geosite:cn,private":
      - https://doh.pub/dns-query
      - https://dns.alidns.com/dns-query
```

## 主流程

此流程图为了更直观和简单地说明 Clash.Meta 的 DNS 工作流程，忽略了 Clash 内部的 DNS 映射处理。

```mermaid
flowchart TD
  Start[客户端发起请求] --> rule[匹配规则]
  rule -->  Domain[匹配到基于域名的规则]
  rule --> IP[匹配到基于 IP 的规则]

  Domain -- 域名匹配到直连规则 --> DNS
  IP --> DNS[通过 Clash DNS 解析域名]

  DNS --> Redir-host/FakeIP
  Redir-host/FakeIP -- 查询 DNS 缓存 --> Cache

  Domain -- 域名匹配到代理规则 --> Remote[通过代理服务器解析域名并建立连接]

  Cache -- Redir-host/FakeIP-Direct 未命中 --> DnsResolve[[域名解析]]
  Cache -- Cache 命中 --> Get
  Cache -- FakeIP 未命中,代理域名 --> Remote

  DnsResolve --> Get[查询得到 IP]
  Get -- 缓存 DNS 结果 --> Cache[(Cache)]
  Get --> End[通过 IP 直接/通过代理建立连接]

```

### 域名解析详细

主流程中 **域名解析** 的详细流程(基于 v1.19.3):

```mermaid
flowchart TD
  Start(域名) --> DirectResolverValid{配置了 direct-nameserver}

  DirectResolverValid -- 是 --> DirectResolver{遵循 nameserver-policy}
  DirectResolver -- 否 --> DirectResolver_Lookup[使用 direct-nameserver 解析]
  DirectResolver -- 是 --> DirectResolver_NSPolicy[匹配 nameserver-policy 并查询 ]
  DirectResolver_NSPolicy -- 没匹配到 --> DirectResolver_Lookup
  DirectResolver_NSPolicy -- 匹配成功 --> End
  DirectResolver_Lookup --> End

  DirectResolverValid -- 否 --> NSPolicy[匹配 nameserver-policy 并<br>并发查询]
  NSPolicy -- 匹配成功 --> End(查询得到 IP)
  NSPolicy -- 没匹配到 --> FB_DOMAIN[匹配 fallback 的域名规则并<br>并发查询]
  FB_DOMAIN -- 匹配成功 --> End
  FB_DOMAIN -- 没匹配到 --> NS[nameserver 并发查询<br>得到 ip]
  NS -- 解析的 ip --> FB_IP_Match{匹配 fallback 的 ip 规则}
  FB_IP_Match -- 是 --> FB_IP[fallback 对原域名并发查询]
  FB_IP_Match -- 否 --> End
  FB_IP --> End

```
