# DNS

```{.yaml linenums="1"}
dns:
  enable: true
  prefer-h3: true
  use-hosts: true
  use-system-hosts: true
  listen: 0.0.0.0:1053
  ipv6: true
  default-nameserver:
    - 223.5.5.5
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  fake-ip-filter:
    - '*.lan'
    - localhost.ptlogin2.qq.com
  nameserver-policy:
    'www.baidu.com': '114.114.114.114'
    '+.internal.crop.com': '10.0.0.1'
    'geosite:cn': https://doh.pub/dns-query
  nameserver:
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  fallback:
    - tls://8.8.4.4
    - tls://1.1.1.1
  proxy-server-nameserver:
    - https://doh.pub/dns-query
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

优先使用 DOH 的 http/3

## listen

DNS 服务监听

## IPV6

是否解析 IPV6, 如为 false, 则回应 AAAA 的空解析

## enhanced-mode

可选值 `fake-ip / redir-host`

mihomo 的 DNS 处理模式

## fake-ip-range

fakeip 下的 IP 段设置，tun 网卡的默认 IP 也使用此值作为参考

## fake-ip-filter

fakeip 过滤，以下地址不会下发 fakeip 映射用于连接

## use-hosts

是否查询配置中的 [hosts](./hosts.md)，默认 true

## use-system-hosts

是否查询系统 hosts，默认 true

## default-nameserver

默认 DNS, 用于解析 DNS 服务器 的域名
!!! node ""
    必须为 IP, 可为加密 DNS

## nameserver-policy

指定域名查询的解析服务器，可使用 geosite, 优先于 `nameserver/fallback 查询`

!!! note
    **以下仅作为书写演示，建议根据自己需求写**

```{.yaml linenums="1"}
nameserver-policy:
  'www.baidu.com': '114.114.114.114'
  '+.internal.crop.com': '10.0.0.1'
  'geosite:geolocation-!cn': [tls://8.8.4.4, https://1.0.0.1/dns-query]
  'www.baidu.com,+.google.cn': https://doh.pub/dns-query
  'geosite:private,apple': https://dns.alidns.com/dns-query
  'rule-set:google,cloudflare': 8.8.8.8
```

## nameserver

默认的域名解析服务器，如不配置 `fallback/proxy-server-nameserver` , 则所有域名都由 nameserver 解析

## proxy-server-nameserver

代理节点域名解析服务器，仅用于解析代理节点的域名

## fallback

后备域名解析服务器，一般情况下使用境外 DNS, 保证结果可信

配置 `fallback`后默认启用 `fallback-filter`,`geoip-code`为 cn

## fallback-filter

后备域名解析服务器筛选，满足条件的将使用 `fallback`结果或只使用 `fallback`解析

### geoip

是否启用 fallback filter

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

## 部分特殊写法

此部分可用于所有的 DNS 服务器

### DNS 经过代理查询

书写格式为 DNS 服务器后 `#策略组或节点`

书写规范应带引号，以防出现特殊字符，如需过代理查询，应配置 `proxy-server-nameserver`, 以防出现鸡蛋问题

```{.yaml linenums="1"}
nameserver:
  - 'tls://dns.google#proxy'
```

### 强制 HTTP/3

此选项与 `perfer-h3` 不冲突，填写后强制启用 HTTP/3 建立 DOH 连接，使用前需确保 DOH 服务器支持 HTTP/3

```{.yaml linenums="1"}
nameserver:
  - 'https://dns.cloudflare.com/dns-query#h3=true'
```

### 指定 DNS 出口网卡

```{.yaml linenums="1"}
nameserver:
  - 'tls://8.8.4.4#en0'
```

### 指定策略组和使用 http/3

```{.yaml linenums="1"}
nameserver:
  - 'https://mozilla.cloudflare-dns.com/dns-query#proxy&h3=true'
```
