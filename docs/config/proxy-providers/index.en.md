# Proxy Providers

```{.yaml linenums="1"}
proxy-providers:
  provider1:
    type: http
    url: "http://test.com"
    path: ./proxy_providers/provider1.yaml
    interval: 3600
    proxy: DIRECT
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
      skip-cert-verify: true
      udp: true
      down: "50 Mbps"
      up: "10 Mbps"
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

  provider2:
    type: file
    path: ./proxy_providers/provider2.yaml
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
```

## Name

This is required, such as `provider1`, and must be unique. It is advisable not to duplicate names with [policy groups](../proxy-groups/index.md#name).

## Type

This is required, the `provider` type, with options of `http/file`.

## URL

If the type is `http`, this must be configured.

## Path

Optional, the file path, must be unique. If not provided, the MD5 of the URL will be used as the filename.

For security reasons, this path is restricted to only allow locations within `HomeDir` (configured with the -d startup parameter). If you wish to store it in any location, set the environment variable `SKIP_SAFE_PATH_CHECK=1`.

## Interval

The update time for the `provider`, measured in seconds.

## Proxy

Download/update through the specified proxy.

## Header

Custom HTTP request headers.

## Health Check

Health check (latency testing).

### health-check.enable

Whether to enable, optional `true/false`.

### health-check.url

Health check address, it is recommended to use one of the following addresses:

=== "Cloudflare"
    ```yaml
    https://cp.cloudflare.com
    ```

=== "Google"
    ```yaml
    https://www.gstatic.com/generate_204
    ```

### health-check.interval

Health check interval, measured in seconds.

### health-check.timeout

Health check timeout, measured in milliseconds.

### health-check.lazy

Lazy state, defaults to `true`, no testing is performed when this provider node is not in use.

### health-check.expected-status

Refer to [expected status](../proxy-groups/index.md#expected-status).

## Override

Override node content, the following fields are supported.

### override.additional-prefix

Add a fixed prefix to the node name.

### override.additional-suffix

Add a fixed suffix to the node name.

### override.proxy-name

Replace the content of the node name, supporting regular expressions, where pattern is the replacement content and target is the replacement target.

### Configuration Items

Refer to `Hysteria`/`Hysteria2` [up](../proxies/hysteria2.md#updown).

Refer to `Hysteria`/`Hysteria2` [down](../proxies/hysteria2.md#updown).

Refer to common fields [skip-cert-verify](../proxies/tls.md#skip-cert-verify).

Refer to common fields [udp](../proxies/index.md#udp).

Refer to common fields [dialer-proxy](../proxies/index.md#dialer-proxy).

Refer to common fields [interface-name](../proxies/index.md#interface-name).

Refer to common fields [routing-mark](../proxies/index.md#routing-mark).

Refer to common fields [ip-version](../proxies/index.md#ip-version).

## Filter

Filter nodes that meet keywords or [regular expressions](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md), multiple regular expressions can be separated by `.

## Exclude Filter

Exclude nodes that meet keywords or [regular expressions](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md), multiple regular expressions can be separated by `.

## Exclude Type

Regular expressions are not supported; use `|` to separate and exclude based on node type.

Note that the syntax for `proxy-groups` and `proxy-providers` is different.