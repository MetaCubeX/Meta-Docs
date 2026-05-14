# dialer-proxy

```{.yaml linenums="1"}
proxies:
- name: "ss1"
  dialer-proxy: dialer
  ...

- name: "ss2"
  ...

proxy-groups:
- name: dialer
  type: select
  proxies:
  - ss2

rules:
  - MATCH,ss1
```

Specifies that the current `proxies` entry establishes network connections through `dialer-proxy`. The value can be the `name` of a [proxy group](../proxy-groups/index.md) or an [outbound proxy](../proxies/index.md).

In the example above, ss1 establishes its connection through ss2.

When traffic is proxied through ss1, the proxy chain becomes `core ---ss1--> ss2 wrapper ===ss2==> ss2 server ---ss1--> ss1 server --> target`.

Final behavior:

* When the browser visits an IP lookup website, only the IP of ss1 is shown, so the target website does not know ss2 exists.
* From the outside, the core only appears to be connecting to ss2, so your ISP does not know ss1 exists.
* Only the ss2 server knows it is connecting to ss1, and it only knows that ss1 is being accessed, not the final target website.

## Common examples

### Relay your own VPS through a subscription node

```{.yaml linenums="1"}
proxies:
- name: "ss1"
  dialer-proxy: dialer
  ...

proxy-providers:
  provider1:
    type: http
    url: "http://test.com"

proxy-groups:
- name: dialer
  type: select
  use:
  - provider1

rules:
  - MATCH,ss1
```

Put the subscription URL in provider1, and put the node you built on your own VPS in ss1. When accessed through the browser, the displayed IP is ss1's IP.

!!! note
    Unless you have special requirements, do not choose UDP-based protocols such as hy2/tuic/wg, or TLS camouflage protocols such as reality/shadowtls, for the node built on your relayed VPS. Your subscription node may not relay these protocols correctly. The simplest SS AEAD or VMess protocol is recommended here.

### Use a specific SOCKS connection for subscription nodes

```{.yaml linenums="1"}
proxies:
- { name: "socks1", type: "socks" }

proxy-providers:
  provider1:
    type: http
    url: "http://test.com"
    override:
      dialer-proxy: socks1

proxy-groups:
- name: select1
  type: select
  use:
  - provider1

rules:
  - MATCH,select1
```

This example applies to environments where the internet can only be reached through a specific SOCKS proxy, such as an isolated internal/external network. Put the SOCKS configuration provided by the environment in socks1, and put the subscription URL in provider1. When accessed through the browser, the displayed IP is the IP of the node in the subscription.

!!! note
    This only demonstrates the dialer-proxy-related configuration. You may need more configuration to ensure subscription downloads, DNS resolution, and other flows also go through this SOCKS proxy.

## relay migration

The `relay` type proxy group has been deprecated, and proxy groups do not directly support `dialer-proxy`. The following reference scheme covers some use cases.

### relay contains multiple select groups

Given the following configuration:

```{.yaml linenums="1"}
proxies:
  - { name: "proxy1", type: "socks" }
  - { name: "proxy2", type: "socks" }
  - { name: "proxy3", type: "socks" }
  - { name: "proxy4", type: "socks" }
proxy-groups:
  - { name: "relay-proxy", type: relay, proxies: ["select1", "select2"] }
  - { name: "select1", type: select, proxies: ["proxy1", "proxy2"] }
  - { name: "select2", type: select, proxies: ["proxy3", "proxy4"] }
rules:
  - MATCH,relay-proxy
```

When migrating to a dialer-proxy scheme, define proxy3 and proxy4 in a proxy-provider, and use `override` to set `dialer-proxy` for all proxies in that provider, as shown below:

```{.yaml linenums="1"}
proxies:
  - { name: "proxy1", type: "socks" }
  - { name: "proxy2", type: "socks" }
proxy-groups:
  - { name: "select1", type: select, proxies: ["proxy1", "proxy2"] }
  - { name: "select2", type: select, use: ["provider1"] }
proxy-providers:
  provider1:
    type: inline
    override:
      dialer-proxy: select1
    payload:
      - { name: "proxy3", type: "socks" }
      - { name: "proxy4", type: "socks" }
rules:
  - MATCH,select2
```
