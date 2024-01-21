## 关于规则

### 优先级

规则将按照从上到下的顺序匹配，列表顶部的规则优先级高于其底下的规则

### 策略

策略包含代理和策略组，每一条规则都必须有一个策略

## 规则语法

Clash 的规则都有三个部分 (MATCH / IP类规则 除外）, 分别为：类型，匹配内容，策略

### 示例

!!! note
    这只是一个示例，请不要照搬

```{.yaml linenums="1"}
rules:
  - DOMAIN-SUFFIX,google.com,auto
  - DOMAIN-KEYWORD,google,auto
  - DOMAIN,ad.com,REJECT
  - SRC-IP-CIDR,192.168.1.201/32,DIRECT
  - IP-CIDR,127.0.0.0/8,DIRECT
  - IP-CIDR6,2620:0:2d0:200::7/32,auto
  - GEOIP,CN,DIRECT
  - DST-PORT,80,DIRECT
  - SRC-PORT,7777,DIRECT
  - IN-TYPE,SOCKS/HTTP,auto
  - AND,((DOMAIN,baidu.com),(NETWORK,UDP)),DIRECT
  - OR,((NETWORK,UDP),(DOMAIN,baidu.com)),REJECT
  - NOT,((DOMAIN,baidu.com)),PROXY
  - RULE-SET,providername,proxy
  - PROCESS-NAME,curl,PROXY
  - SUB-RULE,(AND,((NETWORK,UDP))),sub-rule
  - GEOSITE,youtube,PROXY
  - GEOIP,cn,DIRECT
  - MATCH,auto
```
