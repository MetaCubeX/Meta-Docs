# DNS

```{.yaml linenums="1"}
dns:
  enable: true
  prefer-h3: false
  use-hosts: true
  use-system-hosts: true
  respect-rules: false
  listen: 0.0.0.0:1053
  ipv6: false
  default-nameserver:
    - 223.5.5.5
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  fake-ip-filter-mode: blacklist
  fake-ip-filter:
    - '*.lan'
  nameserver-policy:
    '+.arpa': '10.0.0.1'
    'rule-set:cn':
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  nameserver:
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  fallback:
    - tls://8.8.4.4
    - tls://1.1.1.1
  proxy-server-nameserver:
    - https://doh.pub/dns-query
  direct-nameserver:
    - system
  direct-nameserver-follow-policy: false
  fallback-filter:
    geoip: true
    geoip-code: CN
    geosite:
      - gfw
    ipcidr:
      - 240.0.0.0/4
    domain:
      - '+.google.com'
      - '+.facebook.com'
      - '+.youtube.com'
```

## enable

是否启用，如为 false，则使用系统 DNS 解析

## prefer-h3

DOH 优先使用 http/3

## listen

DNS 服务监听，仅支持 udp

## IPV6

是否解析 IPV6, 如为 false, 则回应 AAAA 的空解析

## enhanced-mode

可选值 `fake-ip`/`redir-host`，默认`redir-host`

mihomo 的 DNS 处理模式

## fake-ip-range

fakeip 下的 IP 段设置，[tun](../inbound/tun.md) 的默认 IPV4 地址 也使用此值作为参考

## fake-ip-filter

fakeip 过滤，以下地址不会下发 fakeip 映射用于连接

值支持[域名通配](../../handbook/syntax.md#_8)以及[引入域名集合](../../handbook/syntax.md#_13)

## fake-ip-filter-mode: blacklist

可选 `blacklist`/`whitelist`，默认`blacklist``，whitelist` 即只有匹配成功才返回 fake-ip

## use-hosts

是否回应配置中的 [hosts](./hosts.md)，默认 true

## use-system-hosts

是否查询系统 hosts，默认 true

## respect-rules

dns 连接遵守[路由规则](../rules/index.md)，需配置 [proxy-server-nameserver](./index.md#proxy-server-nameserver)

## default-nameserver

默认 DNS, 用于解析 DNS 服务器 的域名

!!! node ""
    必须为 IP, 可为加密 DNS

## nameserver-policy

指定域名查询的解析服务器，可使用 geosite, 优先于 `nameserver/fallback 查询`

键支持[域名通配](../../handbook/syntax.md#_8)

值支持字符串/数组

## proxy-server-nameserver

代理节点域名解析服务器，仅用于解析代理节点的域名，如果不填则遵循nameserver-policy、nameserver和fallback的配置

## direct-nameserver

用于direct出口域名解析的 DNS 服务器，如果不填则遵循nameserver-policy、nameserver和fallback的配置

## direct-nameserver-follow-policy

是否遵循nameserver-policy，默认为不遵守，仅当direct-nameserver不为空时生效

## nameserver

默认的域名解析服务器

## fallback

后备域名解析服务器，一般情况下使用境外 DNS, 保证结果可信

配置 `fallback`后默认启用 `fallback-filter`,`geoip-code`为 cn

## fallback-filter

后备域名解析服务器筛选，满足条件的将使用 `fallback`结果或只使用 `fallback`解析

### geoip

是否启用 geoip 判断

### geoip-code

可选值为 国家缩写，默认值为 `CN`

除了 `geoip-code` 配置的国家 IP, 其他的 IP 结果会被视为污染

`geoip-code` 配置的国家的结果会直接采用，否则将采用 `fallback`结果

### geosite

可选值为对于的 geosite 内包含的集合

geosite 列表的内容被视为已污染，匹配到 geosite 的域名，将只使用 `fallback`解析，不去使用 `nameserver`

### ipcidr

书写内容为 `IP/掩码`

这些网段的结果会被视为污染，`nameserver`解析出这些结果时将会采用 `fallback`的解析结果

### domain

这些域名被视为已污染，匹配到这些域名，会直接使用 `fallback`解析，不去使用 `nameserver`

## 附加参数

此部分可用于发向公网地址的 DNS 服务器，使用`#`附加，使用`&`连接不同的参数

### DNS 指定 代理/接口 进行连接

优先使用已有代理，如果不存在该名称的代理则指定接口连接

`#RULES`为遵守路由规则进行连接，等同于[respect-rules](./index.md#respect-rules)

如需经过代理查询，应配置 `proxy-server-nameserver`, 以防出现鸡蛋问题

```{.yaml linenums="1"}
nameserver:
  - 'tls://dns.google#proxy'
  - 'tls://dns.alidns.com#eth0'
```

### 强制 HTTP/3

此选项与 `perfer-h3` 不冲突，填写后强制启用 HTTP/3 建立 DOH 连接，使用前需确保 DOH 服务器支持 HTTP/3

```{.yaml linenums="1"}
nameserver:
  - 'https://dns.cloudflare.com/dns-query#h3=true'
```

### doh 跳过证书验证

```{.yaml linenums="1"}
nameserver:
  - 'https://dns.cloudflare.com/dns-query#skip-cert-verify=true'
```

### ecs

指定 dns 查询的 subnet 地址，仅支持 [doh](./type.md#dns-over-https)

```
nameserver:
  - 'https://8.8.8.8/dns-query#ecs=1.1.1.1/24'
```

### ecs-override

强制覆盖 dns 查询的 subnet 地址，仅支持 [doh](./type.md#dns-over-https)

```
nameserver:
  - 'https://8.8.8.8/dns-query#ecs=1.1.1.1/24&ecs-override=true'
```
