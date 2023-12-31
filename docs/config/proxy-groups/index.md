```yaml
proxy-groups:
- name: "proxy"
  type: select
  disable-udp: true
  interface-name: en0
  routing-mark: 11451
  include-all: false
  include-all-proxies: true
  include-all-providers: false
  filter: "(?i)港|hk|hongkong|hong kong"
  exclude-filter: "美|日"
  exclude-type: "Shadowsocks|Http"
  proxies:
    - DIRECT
    - ss
  use:
    - provider1
    - provider1
```

## name
策略组的名字
!!! note
    如有特殊符号,应当使用引号将其包裹

## type
策略组的类型

## proxies
引入[出站代理](../proxies/index.md)或其他策略组

## use
引入[代理集合](../proxy-providers/index.md)

## disable-udp

禁用该策略组的`UDP`

## interface-name
指定策略组的[出站接口](../general.md#_11)
!!! info ""
    优先级: 代理节点 > 代理策略 > 全局

## routing-mark
策略组出站时附带[路由标记](../general.md#_12)
!!! info ""
    优先级: 代理节点 > 代理策略 > 全局

## include-all
引入所有[出站代理](../proxies/index.md)以及[代理集合](../proxy-providers/index.md)
!!! info ""
    引入不包含策略组,可在[proxies](./index.md#proxies)引入其他策略组

    会使[`include-all-proxies`](./index.md#include-all-proxies)和[`include-all-providers`](./index.md#include-all-providers)失效

## include-all-proxies
引入所有[出站代理](../proxies/index.md)
!!! info ""
    引入不包含策略组,可在[`proxies`](./index.md#proxies)引入其他策略组

## include-all-providers
引入所有[代理集合](../proxy-providers/index.md)
!!! info ""
    会使[`use`](./index.md#use)失效

## filter

筛选满足关键词或[正则表达式](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)的节点

```yaml
filter: "(?i)港|hk|hongkong|hong kong"
```

## exclude-filter

排除满足关键词或[正则表达式](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)的节点

```yaml
exclude-filter: "(?i)港|hk|hongkong|hong kong"
```

## exclude-type

排除节点类型

```yaml
exclude-type: "Shadowsocks|Http"
```

注意，`proxy-groups` 与 `proxy-providers` 写法不同，不支持正则表达式，通过 `|` 分隔
