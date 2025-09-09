# 路由规则

```{.yaml linenums="1"}
rules:
- DOMAIN,ad.com,REJECT
- DOMAIN-SUFFIX,google.com,auto
- DOMAIN-KEYWORD,google,auto
- DOMAIN-WILDCARD,*.google.com,auto
- DOMAIN-REGEX,^abc.*com,PROXY
- GEOSITE,youtube,PROXY

- IP-CIDR,127.0.0.0/8,DIRECT,no-resolve
- IP-CIDR6,2620:0:2d0:200::7/32,auto
- IP-SUFFIX,8.8.8.8/24,PROXY
- IP-ASN,13335,DIRECT
- GEOIP,CN,DIRECT

- SRC-GEOIP,cn,DIRECT
- SRC-IP-ASN,9808,DIRECT
- SRC-IP-CIDR,192.168.1.201/32,DIRECT
- SRC-IP-SUFFIX,192.168.1.201/8,DIRECT

- DST-PORT,80,DIRECT
- SRC-PORT,7777,DIRECT

- IN-PORT,7890,PROXY
- IN-TYPE,SOCKS/HTTP,PROXY
- IN-USER,mihomo,PROXY
- IN-NAME,ss,PROXY

- PROCESS-PATH,/usr/bin/wget,PROXY
- PROCESS-PATH,C:\Program Files\Google\Chrome\Application\chrome.exe,PROXY
- PROCESS-PATH-REGEX,.*bin/wget,PROXY
- PROCESS-PATH-REGEX,(?i).*Application\\chrome.*,PROXY

- PROCESS-NAME,curl,PROXY
- PROCESS-NAME,chrome.exe,PROXY
- PROCESS-NAME,com.termux,PROXY
- PROCESS-NAME-REGEX,curl$,PROXY
- PROCESS-NAME-REGEX,(?i)Telegram,PROXY
- PROCESS-NAME-REGEX,.*telegram.*,PROXY
- UID,1001,DIRECT

- NETWORK,udp,DIRECT
- DSCP,4,DIRECT

- RULE-SET,providername,proxy
- AND,((DOMAIN,baidu.com),(NETWORK,UDP)),DIRECT
- OR,((NETWORK,UDP),(DOMAIN,baidu.com)),REJECT
- NOT,((DOMAIN,baidu.com)),PROXY
- SUB-RULE,(NETWORK,tcp),sub-rule

- MATCH,auto
```

## 优先级

规则将按照从上到下的顺序匹配，列表顶部的规则优先级高于其底下的规则

!!! warning ""
    如请求为 udp，而代理节点没有 udp 支持 (例如`ss`节点没写`udp: true`),则会继续向下匹配

### DOMAIN

匹配完整域名

### DOMAIN-SUFFIX

匹配域名后缀

例：`google.com`匹配`www.google.com`/`mail.google.com`和`google.com`,但不匹配`content-google.com`

### DOMAIN-KEYWORD

域名关键字匹配

### DOMAIN-WILDCARD

通配符匹配，仅支持`*`和`?`通配符

### DOMAIN-REGEX

域名正则表达式匹配

### GEOSITE

匹配 Geosite 内的域名，部分内容参考 [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community/tree/master/data)

### IP-CIDR & IP-CIDR6

匹配 IP 地址范围，`IP-CIDR`和`IP-CIDR6`效果是一样的，`IP-CIDR6`只是一个别名

### IP-SUFFIX

匹配 IP 后缀范围

### IP-ASN

匹配 IP 所属 ASN

### GEOIP

匹配 IP 所属国家代码

### SRC-GEOIP

匹配来源 IP 所属国家代码

### SRC-IP-ASN

匹配来源 IP 所属 ASN

### SRC-IP-CIDR

匹配来源 IP 地址范围

### SRC-IP-SUFFIX

匹配来源 IP 后缀范围

### DST-PORT

匹配请求目标[端口范围](../../handbook/syntax.md#_14)

### SRC-PORT

匹配请求来源[端口范围](../../handbook/syntax.md#_14)

### IN-PORT

匹配[入站端口](../inbound/listeners/index.md#port),可用[端口范围](../../handbook/syntax.md#_14)

### IN-TYPE

匹配[入站类型](../inbound/listeners/index.md#type)

### IN-USER

匹配[入站](../inbound/listeners/index.md)用户名，支持使用 `/` 分隔多个用户名

### IN-NAME

匹配[入站名称](../inbound/listeners/index.md#name)

### PROCESS-PATH

使用完整进程路径匹配

### PROCESS-PATH-REGEX

使用进程路径正则表达式匹配

### PROCESS-NAME

使用进程匹配，在`Android`平台可以匹配包名

### PROCESS-NAME-REGEX

使用进程名称正则表达式匹配，在`Android`平台可以匹配包名

### UID

匹配 Linux USER ID

### NETWORK

匹配`tcp`或者`udp`

### DSCP

匹配`DSCP`标记 (仅限 tproxy udp 入站)

### RULE-SET

引用规则集合，需配置[rule-providers](../rule-providers/index.md)

### AND & OR & NOT

`LOGIC_TYPE,((payload1),(payload2)),Proxy`

*payload1*、*payload2* 为 规则类型和其他 payload，如：`DOMAIN,google.com`

逻辑规则，需要**注意括号**的使用

### SUB-RULE

匹配至[子规则](../sub-rule.md),需要注意括号的使用

### MATCH

匹配所有请求，无需条件

## 附加参数

### no-resolve

仅支持关于 `目标IP` 的规则

域名开始匹配关于 `目标IP` 规则时，mihomo 将触发 dns 解析来检查域名的 `目标IP` 是否匹配规则，可以选择 `no-resolve` 选项以跳过 dns 解析

如在更早的匹配中触发了 dns 解析，则依旧会匹配到添加了 `no-resolve` 选项的 `目标IP` 类规则

### src

仅支持关于 `目标IP` 的规则

将 `目标IP` 匹配转为 `来源IP` 匹配
