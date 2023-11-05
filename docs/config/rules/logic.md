---
description: 与或非逻辑判断
---
# 逻辑规则

## AND

规则内的条件都必须满足

示例为匹配 `baidu.com` 域名并且网络类型为 tcp 的请求 直连

```yaml
rules:
- AND,((DOMAIN,baidu.com),(NETWORK,tcp)),DIRECT
```

## OR

规则内的条件只需满足一项即可

示例为域名关键词为 pcdn 或域名关键词为 stun 的请求 拦截

```yaml
rules:
- OR,((DOMAIN-KEYWORD,pcdn),(DOMAIN-KEYWORD,stun)),REJECT
```

## NOT

必须为规则内不包含的条件

示例为不匹配 baidu.com 域名的请求走 proxy 节点/策略组

```yaml
rules:
- NOT,((DOMAIN,baidu.com)),PROXY
```

## no-resolve

ip 类规则可用 no-resolve, 需书写在括号内

关于 [no-resolve](ipcidr.md#no-resolve)

示例

```yaml
rules:
  - AND,((DST-PORT,22),(GEOIP,CN,no-resolve)),DIRECT
```

!!! note
    逻辑判断规则支持多层嵌套，注意括号的用法
