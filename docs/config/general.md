# 全局配置

## 允许局域网

允许其他设备经过 Clash 的[代理端口](./inbound/port.md)访问互联网

可选值 `true/false`

```{.yaml linenums="1"}
allow-lan: true
```

绑定地址，仅允许其他设备通过这个地址访问

***`"*"`*** 绑定所有 IP 地址

***`192.168.31.31`*** 绑定单个 IPV4 地址

***`[aaaa::a8aa:ff:fe09:57d8]`*** 绑定单个 IPV6 地址

```{.yaml linenums="1"}
bind-address: "*"
```

允许连接的 IP 地址段，仅作用于 `allow-lan` 为 `true`
默认值为 `0.0.0.0/0`和 `::/0`

```{.yaml linenums="1"}
lan-allowed-ips:
- 0.0.0.0/0
- ::/0
```

禁止连接的 IP 地址段，黑名单优先级高于白名单，默认值为空

```{.yaml linenums="1"}
lan-disallowed-ips:
- 192.168.0.3/32
```

### 用户验证

`http(s)`/`socks`/`mixed`代理的用户验证

```{.yaml linenums="1"}
authentication:
- "user1:pass1"
- "user2:pass2"
```

设置允许跳过验证的 IP 段

```{.yaml linenums="1"}
skip-auth-prefixes:
- 127.0.0.1/8
- ::1/128
```

## 运行模式

* ***`rule`*** 规则匹配
* ***`global`*** 全局代理 (需要在 GLOBAL 策略组选择代理/策略)
* ***`direct`*** 全局直连

此项拥有默认值，默认为规则模式

```{.yaml linenums="1"}
mode: rule
```

## 日志级别

Clash 内核输出日志的等级，仅在控制台和控制页面输出

```{.yaml linenums="1"}
log-level: info
```

* ***`silent`*** 静默，不输出
* ***`error`*** 仅输出发生错误至无法使用的日志
* ***`warning`*** 输出发生错误但不影响运行的日志，以及 error 级别内容
* ***`info`*** 输出一般运行的内容，以及 error 和 warning 级别的日志
* ***`debug`*** 尽可能的输出运行中所有的信息

## IPv6

是否允许内核接受 IPv6 流量

可选值 `true/false,`默认为 `true`

```{.yaml linenums="1"}
ipv6: true
```

## TCP Keep Alive 设置

修改此项以减少移动设备[耗电问题](https://github.com/vernesong/OpenClash/issues/2614)

TCP Keep Alive 包的间隔，单位为秒

```{.yaml linenums="1"}
keep-alive-interval: 15
```

TCP Keep Alive 的最大空闲时间

```{.yaml linenums="1"}
keep-alive-idle: 15
```

禁用 TCP Keep Alive，在 Android 默认为 true

```{.yaml linenums="1"}
disable-keep-alive: false
```

## 进程匹配模式

控制是否让 Clash 去匹配进程

* ***`always`*** 开启，强制匹配所有进程
* ***`strict`*** 默认，由 Clash 判断是否开启
* ***`off`*** 不匹配进程，推荐在路由器上使用此模式

```{.yaml linenums="1"}
find-process-mode: strict
```

## 外部控制 (API)

外部控制器，可以使用 RESTful API 来控制你的 Clash 内核

API 监听地址，你可以将 127.0.0.1 修改为 0.0.0.0 来监听所有 IP

```{.yaml linenums="1"}
external-controller: 127.0.0.1:9090
```

API CORS 标头配置

```{.yaml linenums="1"}
external-controller-cors:
  allow-origins:
    - *
  allow-private-network: true
```

Unix socket API 监听地址

!!! warning ""
    从Unix socket访问api接口不会验证secret， 如果开启请自行保证安全问题

```{.yaml linenums="1"}
external-controller-unix: mihomo.sock
```

Windows namedpipe API 监听地址

!!! warning ""
    从Windows namedpipe访问api接口不会验证secret， 如果开启请自行保证安全问题

```{.yaml linenums="1"}
external-controller-pipe: \\.\pipe\mihomo
```

HTTPS-API 监听地址，需要配置 `tls` 部分证书和其私钥配置，使用 TLS 也必须填写 `external-controller`

```{.yaml linenums="1"}
external-controller-tls: 127.0.0.1:9443
```

在 RESTful API 端口上开启 DOH 服务器

!!! warning ""
    该URL不会验证secret， 如果开启请自行保证安全问题

```{.yaml linenums="1"}
external-doh-server: /dns-query
```

API 的访问密钥

```{.yaml linenums="1"}
secret: ""
```

## 外部用户界面

可以将静态网页资源 (比如 Clash-dashboard) 运行在 Clash API, 路径为 API 地址/ui

```{.yaml linenums="1"}
external-ui: /path/to/ui/folder
```

可以为绝对路径，或者 Clash 工作目录的相对路径

## 自定义外部用户界面名字

```{.yaml linenums="1"}
external-ui-name: xd      #  合并为 external-ui/xd
```

非必须，更新时会更新到指定文件夹，不配置则直接更新到 external-ui 目录

## 自定义外部用户界面下载地址

```{.yaml linenums="1"}
external-ui-url: "https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip" #从 GitHub Pages 分支获取
```

## 缓存

```{.yaml linenums="1"}
profile:
  store-selected: true
  # 储存 API 对策略组的选择，以供下次启动时使用
  store-fake-ip: true
  # 储存 fakeip 映射表，域名再次发生连接时，使用原有映射地址
```

## 统一延迟

开启统一延迟时，会计算 RTT，以消除连接握手等带来的不同类型节点的延迟差异

可选值 `true/false`

```{.yaml linenums="1"}
unified-delay: true
```

## TCP 并发

可选值 `true/false`

```{.yaml linenums="1"}
tcp-concurrent: true
```

## 出站接口

Clash 的流量出站接口

```{.yaml linenums="1"}
interface-name: en0
```

## 路由标记

为 Linux 下的出站连接提供默认流量标记

```{.yaml linenums="1"}
routing-mark: 6666
```

## TLS

目前仅用于 API 的 https

```{.yaml linenums="1"}
tls:
  certificate: string # 证书 PEM 格式，或者 证书的路径
  private-key: string # 证书对应的私钥 PEM 格式，或者私钥路径
```

## 全局客户端指纹

全局 TLS 指纹，优先低于 proxy 内的 client-fingerprint。

目前支持开启 TLS 传输的 TCP/grpc/WS/HTTP , 支持协议有 VLESS,Vmess 和 trojan.

```{.yaml linenums="1"}
global-client-fingerprint: chrome
```

!!! note
    可选：`chrome`, `firefox`, `safari`, `iOS`, `android`, `edge`, `360`, `qq`, `random`, 若选择 `random`, 则按 Cloudflare Radar 数据按概率生成一个现代浏览器指纹。

## GEOIP 数据模式

更改 geoip 使用文件，mmdb 或者 dat，可选 `true`/`false`,`true`为 dat，此项有默认值 `false`

```{.yaml linenums="1"}
geodata-mode: true 
```

## GEO 文件加载模式

可选的加载模式如下

* `standard`：标准加载器
* `memconservative`：专为内存受限 (小内存) 设备优化的加载器 (默认值)

```{.yaml linenums="1"}
geodata-loader: memconservative
```

## 自动更新 GEO

```{.yaml linenums="1"}
geo-auto-update: false
```

更新间隔，单位为小时

```{.yaml linenums="1"}
geo-update-interval: 24
```

## 自定 GEO 下载地址

```{.yaml linenums="1"}
geox-url:
  geoip: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat"
  geosite: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat"
  mmdb: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb"
  asn: "https://github.com/xishang0128/geoip/releases/download/latest/GeoLite2-ASN.mmdb"
```

## 自定全局 UA

自定义外部资源下载时使用的的 UA，默认为 `clash.meta`

```{.yaml linenums="1"}
global-ua: clash.meta
```

## ETag 支持

外部资源下载的 ETag 支持，默认为 `true`

```{.yaml linenums="1"}
etag-support: true
```
