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

The traffic flow is `Clash` <-> `http` <-> `vmess` <-> `ss1` <-> `ss2` <-> `Internet`.

## Regarding UDP

Relay supports UDP transmission, provided that both the head and tail nodes of the proxy chain support UDP over TCP.
Currently, the protocols that support UDP include `vmess`, `vless`, `trojan`, `ss`, `ssr`, and `tuic`.
