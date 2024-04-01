# LISTENERS

```{.yaml linenums="1"}
listeners:
- name: in-name
  type: shadowsocks
  port: 10000
  listen: 0.0.0.0
  rule: sub-rule-1
  proxy: proxy
```

## name

入站名称，可用于匹配[`入站规则`](../../rules/index.md#in-name)

## type

入站类型

## listen

监听地址

## port

监听端口

## rule

使用[`子规则`](../../sub-rule.md)作为入站匹配规则出站，为空则默认使用[`rules`](../../rules/index.md),如果未找到[`子规则`](../../sub-rule.md)则直接使用[`rules`](../../rules/index.md)

## proxy

使用[`出站代理`](../../proxies/index.md)或[`策略组`](../../proxy-groups/index.md)直接出站，为空则默认使用[`rules`](../../rules/index.md)

!!! warning ""
    当`proxy`不为空时，这里的`proxy`名称必须合法，否则会出错
