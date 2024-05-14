# Common fields

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

Required. The name of the policy group.

!!! note
    If there are special symbols, they should be enclosed in quotes.

## type

Required. The type of the policy group.

## proxies

include [proxies](../proxies/index.md) or other proxy-groups.

## use

include [proxy-providers](../proxy-providers/index.md)

## url

Health check test URL.

## interval

Health check interval. If not 0, enable periodic testing, in seconds.

## lazy

Lazy mode. Default is `true`. Do not test unless this policy group is selected.

### timeout

Health check timeout, in milliseconds.

## max-failed-times

Maximum number of failures before triggering a forced health check. Default is 5.

## disable-udp

Disable `UDP` for this proxy-groups.

## interface-name

Specify the [outbound interface](../general.md#_11) for the proxy-groups.
!!! info ""
    Priority: Proxies > Proxy Groups > Global

## routing-mark

Attach a [routing mark](../general.md#_12)when the proxy-groups goes outbound.
!!! info ""
    Priority: Proxies > Proxy Groups > Global

## include-all

Include all [proxies](../proxies/index.md) and [proxy-providers](../proxy-providers/index.md).

!!! info ""
    Includes without policy groups. Other policy groups can be included in [`proxies`](./index.md#proxies).

## include-all-proxies

Include all [proxies](../proxies/index.md)

!!! info ""
    Includes without policy groups. Other policy groups can be included in [`proxies`](./index.md#proxies).

## include-all-providers

Include all [proxy-providers](../proxy-providers/index.md).

!!! info ""
    This will disable [include proxy-providers](./index.md#use).

## filter

Filter nodes that meet the keywords or [regular expressions](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md). Multiple regular expressions can be separated by `.

!!! info ""
    Only applies to include proxy-providers and [include all outbound proxies](./index.md#include-all-proxies).

## exclude-filter

Exclude proxies that match keywords or [regular expressions](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md). Multiple regular expressions can be separated by `.

## exclude-type

Exclude proxies types.

Note that the syntax for `proxy-groups` and `proxy-providers` is different and does not support regular expressions. They are separated by `|`.

## expected-status

Expected HTTP response status code during health check. If this field is configured, the node is considered available only when the response status code is consistent with the expected status. Default is `*`, indicating no requirements for the response status.

### Syntax

Use `/` to match multiple status codes, use `-` to match status code ranges, and mix the syntax.

#### Examples

Match status codes 200 and 302:

```{.yaml linenums="1"}
expected-status: 200/302
```

Match status codes from 400 to 503:

```{.yaml linenums="1"}
expected-status: 400-503
```

Match status codes 200 and 302, as well as from 400 to 503:

```{.yaml linenums="1"}
expected-status: 200/302/400-503
```

## hidden

Returns hidden status in the API to hide the display of this proxy-groups (requires front-end adaptation using the API).

## icon

Returns the string entered in icon in the API to display in this proxy-groups (requires front-end adaptation using the API).
