# Relay

!!! warning
    Стратегия relay устарела; вместо нее используйте [dialer-proxy](../proxies/index.md#dialer-proxy).

<!--
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
    Стратегия relay скоро будет устаревшей. Пожалуйста, используйте [dialer-proxy](../proxies/index.md#dialer-proxy).

    WireGuard в настоящее время не поддерживает использование в relay, поэтому также используйте [dialer-proxy](../proxies/index.md#dialer-proxy).

Поток трафика: Clash <-> http <-> vmess <-> ss1 <-> ss2 <-> Internet.

## О UDP

Relay поддерживает передачу UDP при условии, что начальный и конечный узлы прокси-цепочки поддерживают UDP через TCP. В настоящее время протоколы, поддерживающие UDP, включают `vmess`, `vless`, `trojan`, `ss`, `ssr` и `tuic`. 
-->
