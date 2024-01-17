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
  expected-status: 204
  hidden: true
  icon: xxx
  proxies:
  - DIRECT
  - ss
  use:
  - provider1
  - provider1
```

## name
必须项,策略组的名字
!!! note
    如有特殊符号,应当使用引号将其包裹

## type
必须项,策略组的类型

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
!!! info ""
    仅作用于引入代理集合

## exclude-filter
排除满足关键词或[正则表达式](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)的节点
!!! info ""
    仅作用于引入代理集合

## exclude-type
排除节点类型
!!! info ""
    仅作用于引入代理集合

注意，`proxy-groups` 与 `proxy-providers` 写法不同，不支持正则表达式，通过 `|` 分隔

## expected-status
健康检查时期望的 HTTP 响应状态码。若配置了该字段，则只有当响应状态码与期望状态一致时才认为节点可用。默认为 `*`，表示对响应状态不做要求

### 写法

可使用 `/` 匹配多个状态码，使用 `-` 匹配状态范围，可混合书写

#### 示例

匹配 200 和 302 状态码

```yaml
expected-status: 200/302
```

匹配 400 到 503 状态码

```yaml
expected-status: 400-503
```

匹配 200 和 302 以及 400 到 503 状态码

```yaml
expected-status: 200/302/400-503
```

## hidden

在api返回`hidden`状态,以隐藏该策略组展示(需要使用api的前端适配)

## icon

在api返回`icon`所输入的字符串,以在该策略组显示(需要使用api的前端适配)