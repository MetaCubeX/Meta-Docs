```{.yaml linenums="1"}
sub-rules:
  rule1:
    - DOMAIN-SUFFIX,baidu.com,DIRECT
    - MATCH,PROXY
  sub-rule2:
    - IP-CIDR,1.1.1.1/32,REJECT
    - IP-CIDR,8.8.8.8/32,ss1
    - DOMAIN,dns.alidns.com,REJECT
```
