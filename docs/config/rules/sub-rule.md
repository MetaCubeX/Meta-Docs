## SUB-RULE

匹配到规则时,将请求送往另一规则流程,括号内可以使用任意规则。
如果在 sub-rule 内没匹配到，则会退回常规规则流程

```{.yaml linenums="1"}
rules:
- SUB-RULE,(NETWORK,UDP),rule1
```

### 流程示例

假设配置文件如下：

```{.yaml linenums="1"}
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

- 一个 google 的 tcp 请求，匹配到rule1，并匹配到其中的第一条规则，走ss1出站
- 一个 youtube 的 quic 请求，匹配到 rule2 ，但是没匹配到 rule2 内的规则，则回退常规流程，匹配到 match
