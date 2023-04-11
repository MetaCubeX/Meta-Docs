---
description: 与或非逻辑判断
---

# 逻辑规则

## AND

规则内的条件都必须满足

示例为匹配baidu.com域名并且网络类型为tcp的请求 直连

```
AND,((DOMAIN,baidu.com),(NETWORK,tcp)),DIRECT
```

## OR

规则内的条件只需满足一项即可

示例为域名关键词为pcdn或域名关键词为stun的请求 拦截

```
OR,((DOMAIN-KEYWORD,pcdn),(DOMAIN-KEYWORD,stun)),REJECT
```

## NOT

必须为规则内不包含的条件

示例为不匹配baidu.com域名的请求走proxy节点/策略组

```
NOT,((DOMAIN,baidu.com)),PROXY
```

## no-resolve

ip类规则可用no-resolve,需书写在括号内

关于 [no-resolve](ipcidr.md#no-resolve)

示例

```yaml
rules:
  - AND,((DST-PORT,22),(GEOIP,CN,no-resolve)),DIRECT
```

!!! note
    逻辑判断规则支持多层嵌套，注意括号的用法


|      支持的规则类别     |
| :--------------: |
| NETWORK（UDP/TCP） |
|      DOMAIN      |
|   DOMAIN-SUFFIX  |
|  DOMAIN-KEYWORD  |
|      IP-CIDR     |
|    SRC-IP-CIDR   |
|     SRC-PORT     |
|      IN-TYPE     |
|      GEOSITE     |
|       GEOIP      |
|     RULE-SET     |
