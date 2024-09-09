# Relay

```{.yaml linenums="1"}
Proxy Groups:
# Traffic: Clash <-> http <-> vmess <-> ss1 <-> ss2 <-> Internet
- name: "relay"
  type: relay
  proxies:
    - http
    - vmess
    - ss1
    - ss2
```

!!! warning
    The relay strategy is about to be deprecated. Please use [dialer-proxy](../proxies/index.md#dialer-proxy).

    WireGuard currently does not support usage in relay, so please also use [dialer-proxy](../proxies/index.md#dialer-proxy).

The traffic flow is Clash <-> http <-> vmess <-> ss1 <-> ss2 <-> Internet.

## About UDP

Relay supports the transmission of UDP, provided that both the head and tail nodes of the proxy chain support UDP over TCP. Currently, the protocols that support UDP include `vmess`, `vless`, `trojan`, `ss`, `ssr`, and `tuic`.