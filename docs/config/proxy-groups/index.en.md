# General Fields

```{.yaml linenums="1"}
proxy-groups:
- name: "proxy"
  type: select
  proxies:
  - DIRECT
  - ss
  use:
  - provider1
  - provider1

  url: 'https://www.gstatic.com/generate_204'
  interval: 300
  lazy: true
  timeout: 5000
  max-failed-times: 5

  disable-udp: true
  interface-name: en0
  routing-mark: 11451
  include-all: false
  include-all-proxies: false
  include-all-providers: false
  filter: "(?i)港|hk|hongkong|hong kong"
  exclude-filter: "美|日"
  exclude-type: "Shadowsocks|Http"
  expected-status: 204
  hidden: true
  icon: xxx
```

## name

Required, the name of the proxy group.

!!! note
    If there are special characters, they should be enclosed in quotes.

## type

Required, the type of the proxy group.

## proxies

References to [outbound proxies](../proxies/index.md) or other proxy groups.

## use

References to [proxy sets](../proxy-providers/index.md).

## url

Health check test address.

## interval

Health check interval; if not 0, periodic testing is enabled, measured in seconds.

## lazy

Lazy state, defaults to `true`. If the current proxy group is not selected, no testing is performed.

## timeout

Health check timeout, measured in milliseconds.

## max-failed-times

Maximum number of failures; exceeding this triggers a forced health check, default is 5.

## disable-udp

Disables `UDP` for this proxy group.

## interface-name

Specifies the [outbound interface](../general.md#outbound-interface) for the proxy group.

!!! info ""
    Priority: Proxy Node > Proxy Policy > Global.

## routing-mark

The [routing mark](../general.md#routing-mark) attached when the proxy group is outbound.

!!! info ""
    Priority: Proxy Node > Proxy Policy > Global.

## include-all

Includes all [outbound proxies](../proxies/index.md) and [proxy sets](../proxy-providers/index.md), sorted by name.

!!! info ""
    Inclusion does not include proxy groups; other proxy groups can be included in [proxies](./index.md#proxies).

## include-all-proxies

Includes all [outbound proxies](../proxies/index.md), sorted by name.

!!! info ""
    Inclusion does not include proxy groups; other proxy groups can be included in [`proxies`](./index.md#proxies).

## include-all-providers

Includes all [proxy sets](../proxy-providers/index.md), sorted by name.

!!! info ""
    This will invalidate [including proxy sets](./index.md#use).

## filter

Filters nodes that meet keywords or [regular expressions](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md). You can use ` to separate multiple regular expressions.

!!! info ""
    This only applies to included proxy sets and [including all outbound proxies](./index.md#include-all-proxies).

## exclude-filter

Excludes nodes that meet keywords or [regular expressions](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md). You can use ` to separate multiple regular expressions.

## exclude-type

Regular expressions are not supported. Split by `|`, exclude based on node type, only excluding [ingress outbound proxies](#proxies)

For supported types, please refer to [Adapter Type](https://github.com/MetaCubeX/mihomo/blob/fbead56ec97ae93f904f4476df1741af718c9c2a/constant/adapters.go#L18-L45)

## expected-status

The expected HTTP response status code during health checks. If this field is configured, a node is considered available only when the response status code matches the expected status. The default is `*`, indicating no requirements on the response status.

### syntax

You can use `/` to match multiple status codes, `-` to match a range of statuses, and mix them.

#### example

Match status codes 200 and 302.

```{.yaml linenums="1"}
expected-status: 200/302
```

Match status codes from 400 to 503.

```{.yaml linenums="1"}
expected-status: 400-503
```

Match status codes 200 and 302, as well as from 400 to 503.

```{.yaml linenums="1"}
expected-status: 200/302/400-503
```

## hidden

Returns `hidden` status in the API to hide the display of this proxy group (requires front-end adaptation using the API).

## icon

Returns the string input for `icon` in the API to display in this proxy group (requires front-end adaptation using the API).