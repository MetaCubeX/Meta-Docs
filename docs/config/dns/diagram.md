# DNS 流程图

```mermaid
flowchart TD
  A[客户端发起请求] --> B[匹配规则]
  B --> IP[匹配到基于 IP 的规则]
  B -->  I[匹配到基于域名的规则]
  IP --> DNS[使用 Clash DNS 解析域名]
  DNS --> Cache
  Cache --> |Cache 未命中|NS[使用 nameserver-policy 匹配]
  Cache --> |Cache 命中|G
  NS --> |匹配成功| G[将查询到的 IP 用于匹配 IP 规则]
  NS --> |没匹配到| F[用 nameserver/fallback 并发查询]

  F --> G[查询得到 IP]
  G --> |缓存 DNS 结果 |Cache
  I --> |域名匹配到直连规则| DNS
  I --> |域名匹配到代理规则|L[通过代理服务器解析域名并建立连接]
  G --> S[通过 IP 建立连接]
```
