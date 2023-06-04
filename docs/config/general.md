# **全局配置**

## **代理端口**

**端口是计算机或路由交换机内部的一部分，计算机按照 INTERNET 传输层 TCP/IP 协议进行通信，不同的协议对应不同的端口**

http(s) 代理端口

```yaml
port: 7890
```

socks4/4a/5 代理端口

```yaml
socks-port: 7891
```

混合代理端口 http(s)+socks

```yaml
mixed-port: 7892
```

!!! note

    redir 端口仅限 Linux 以及 macOS 适用，tproxy 端口仅限 linux 适用 (Android 设备属于 Linux 设备）

redirect 透明代理端口，仅能代理 TCP 流量

```yaml
redir-port: 7893
```

tproxy 透明代理端口，可以代理 TCP 以及 UDP 流量

```yaml
tproxy-port: 7894
```

## **允许局域网**

允许其他设备经过 Clash 的代理端口访问互联网

可选值 `true/false`

```yaml
allow-lan: true
```

绑定 IP, 通过 LAN 访问一个/一些特定的 IP 地址

"\*" 绑定所有 IP 地址，默认值，不填写此项则绑定全部

192.168.31.31: 绑定单个 IPV4 地址

"\[aaaa::a8aa:ff:fe09:57d8]": 绑定单个 IPV6 地址

```yaml
bind-address: "*"
```

http(s) 和 socks 代理的用户验证

```yaml
authentication:
  - "user1:pass1"
  - "user2:pass2"
```

## **运行模式**

* `rule` 规则匹配
* `global` 全局代理(需要在GLOBAL策略组选择代理/策略)
* `direct` 全局直连

此项拥有默认值，默认为规则模式

```yaml
mode: rule
```

## **日志级别**

Clash 内核输出日志的等级，仅在控制台和控制页面输出

```yaml
log-level: info
```

* `silent` 静默，不输出
* `error` 仅输出发生错误至无法使用的日志
* `warning` 输出发生错误但不影响运行的日志，以及 error 级别内容
* `info` 输出一般运行的内容，以及 error 和 warning 级别的日志
* `debug` 尽可能的输出运行中所有的信息

## **IPv6**

是否允许内核接受 IPv6 流量

可选值 `true/false,`默认为 `false`

```yaml
ipv6: true
```

## **进程匹配模式**

控制是否让 Clash 去匹配进程

* `always` 开启，强制匹配所有进程
* `strict` 默认，由 Clash 判断是否开启
* `off` 不匹配进程，推荐在路由器上使用此模式

```yaml
find-process-mode: strict
```

## **外部控制 (API)**

外部控制器，可以使用 RESTful API 来控制你的 Clash 内核

API 监听地址，你可以将 127.0.0.1 修改为 0.0.0.0 来监听所有 IP

```yaml
external-controller: 127.0.0.1:9090
```

HTTPS-API 监听地址，需要配置 `tls` 部分证书和其私钥配置，使用 TLS 也必须填写 `external-controller`

```yaml
external-controller-tls: 127.0.0.1:9443
```

API 的访问密钥

```yaml
secret: ""
```

## **外部用户界面**

可以将静态网页资源（比如 Clash-dashboard) 运行在 Clash API, 路径为 API 地址/ui

可以为绝对路径，或者 Clash 工作目录的相对路径

```yaml
external-ui: dashboard
```

## **缓存**

在 Clash 官方中，profile 应为扩展配置，但在 Clash.meta, 仅作为缓存项使用

可选值 `true/false`

```yaml
profile:
  store-selected: true
  # 储存 API 对策略组的选择，以供下次启动时使用
  store-fake-ip: true
  # 储存 fakeip 映射表，域名再次发生连接时，使用原有映射地址
```

## **统一延迟**

更换延迟计算方式,去除握手等额外延迟

可选值 `true/false`

```yaml
unified-delay: true
```

## **TCP并发**

可选值 `true/false`

```yaml
tcp-concurrent: true
```

## **出站接口**

Clash 的流量出站接口

```yaml
interface-name: en0
```

## **路由标记**

为 Linux 下的出站连接提供默认流量标记

```yaml
routing-mark: 6666
```

## **TLS**

目前仅用于 API 的 https

```yaml
tls:
  certificate: string # 证书 PEM 格式，或者 证书的路径
  private-key: string # 证书对应的私钥 PEM 格式，或者私钥路径
```

## **全局客户端指纹**

全局 TLS 指纹，优先低于 proxy 内的 client-fingerprint。

目前支持开启 TLS 传输的 TCP/grpc/WS/HTTP , 支持协议有 VLESS,Vmess 和 trojan.

```yaml
global-client-fingerprint: chrome
```

!!! note

    可选："chrome", "firefox", "safari", "iOS", "android", "edge", "360"," qq", "random"

    若选择 "random", 则按 Cloudflare Radar 数据按概率生成一个现代浏览器指纹。

## **GEO数据模式**

更改geoip使用文件,mmdb或者dat,`true`为dat

```yaml
geodata-mode: true
```

## **GEO文件加载模式**

可选的加载模式如下

* `standard`：标准加载器
* `memconservative`：专为内存受限(小内存)设备优化的加载器(默认值)

```yaml
geodata-loader: memconservative
```

## **自定 GEO 下载地址**

```yaml
geox-url:
  geoip: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat"
  geosite: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat"
  mmdb: "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb"
```
