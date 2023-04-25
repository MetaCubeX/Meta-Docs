# 子规则

匹配到规则时,将请求送往另一规则流程,示例

```
sub-rules:
  rule1:
    - DOMAIN-SUFFIX,google.com,ss1
    - DOMAIN-SUFFIX,baidu.com,DIRECT
  sub-rule2:
    - IP-CIDR,1.1.1.1/32,REJECT
    - IP-CIDR,8.8.8.8/32,ss1
    - DOMAIN,dns.alidns.com,REJECT
rules:
  - SUB-RULE,(NETWORK,TCP),rule1
  - SUB-RULE,(NETWORK,UDP),sub-rule2
  - MATCH,DIRECT
```

如果在sub-rule内没匹配到,则会退回常规规则流程

如,一个youtube的quic请求,匹配到rule2,但是没匹配到rule2内的规则,则回退常规流程,匹配到match
