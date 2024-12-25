# 代理集合

```{.yaml linenums="1"}
proxy-providers:
  provider1:
    type: http
    url: "http://test.com"
    path: ./proxy_providers/provider1.yaml
    interval: 3600
    proxy: DIRECT
    size-limit: 0
    header:
      User-Agent:
      - "Clash/v1.18.0"
      - "mihomo/1.18.3"
      Authorization:
      - 'token 1231231'
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
      timeout: 5000
      lazy: true
      expected-status: 204
    override:
      tfo: false
      mptcp: false
      udp: true
      udp-over-tcp: false
      down: "50 Mbps"
      up: "10 Mbps"
      skip-cert-verify: true
      dialer-proxy: proxy
      interface-name: tailscale0
      routing-mark: 233
      ip-version: ipv4-prefer
      additional-prefix: "provider1 prefix |"
      additional-suffix: "| provider1 suffix"
      proxy-name:
      - pattern: "IPLC-(.*?)倍"
        target: "iplc x $1"
    filter: "(?i)港|hk|hongkong|hong kong"
    exclude-filter: "xxx"
    exclude-type: "ss|http"
    payload:
      - name: "ss1"
        type: ss
        server: server
        port: 443
        cipher: chacha20-ietf-poly1305
        password: "password"
```

## name

必须，如`provider1`，不能重复，建议不要和[策略组](../proxy-groups/index.md#name)名称重复

## type

必须，`provider`类型，可选`http` / `file` / `inline`

## url

类型为`http`是则需要配置

## path

可选，文件路径，不可重复，不填写时会使用 url 的 MD5 作为此文件的文件名

由于安全问题，此路径将限制只允许在 `HomeDir`(有启动参数 -d 配置) 中，如果想存储到任意位置配置环境变量 `SKIP_SAFE_PATH_CHECK=1`

## interval

更新`provider`的时间，单位为秒

## proxy

经过指定代理进行下载/更新

## size-limit

限制下载文件的最大大小，默认为 0 即不限制文件大小，单位为字节 (`b`)

## header

自定义 http 请求头

## health-check

健康检查 (延迟测试)

### health-check.enable

是否启用，可选 `true/false`

### health-check.url

健康检查地址，推荐使用以下地址之一

=== "Cloudflare"
    ```yaml
    https://cp.cloudflare.com
    ```

=== "Google"
    ```yaml
    https://www.gstatic.com/generate_204
    ```

### health-check.interval

健康检查间隔时间，单位为秒

### health-check.timeout

健康检查超时时间，单位为毫秒

### health-check.lazy

懒惰状态，默认为`true`,不使用该集合节点时，不进行测试

### health-check.expected-status

参阅 [期望状态](../proxy-groups/index.md#expected-status)

## override

覆写节点内容，以下为支持的字段

### override.additional-prefix

为节点名称添加固定前缀

### override.additional-suffix

为节点名称添加固定后缀

### override.proxy-name

对节点名称内容进行替换，支持正则表达式，pattern 为替换内容，target 为替换目标

### override.其余配置项

参阅通用字段  [tfo](../proxies/index.md#tfo)

参阅通用字段  [mptcp](../proxies/index.md#mptcp)

参阅通用字段  [udp](../proxies/index.md#udp)

参阅`Shadowsocks`  [udp-over-tcp](../proxies/ss.md#udp-over-tcp)

参阅`Hysteria`/`Hysteria2`  [up](../proxies/hysteria2.md#updown)

参阅`Hysteria`/`Hysteria2`  [down](../proxies/hysteria2.md#updown)

参阅通用字段  [skip-cert-verify](../proxies/tls.md#skip-cert-verify)

参阅通用字段  [dialer-proxy](../proxies/index.md#dialer-proxy)

参阅通用字段  [interface-name](../proxies/index.md#interface-name)

参阅通用字段  [routing-mark](../proxies/index.md#routing-mark)

参阅通用字段  [ip-version](../proxies/index.md#ip-version)

## filter

筛选满足关键词或[正则表达式](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)的节点，可以使用 ` 区分多个正则表达式

## exclude-filter

排除满足关键词或[正则表达式](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)的节点，可以使用 ` 区分多个正则表达式

## exclude-type

不支持正则表达式，通过 `|` 分割，根据节点类型排除

provider 的 `exclude-type` 使用配置文件中的 `type` 类型进行排除

## payload

内容，仅 `type` 为 `inline` 时生效