# Inbound Configuration

mihomo can receive traffic through proxy ports, TUN, and Listeners. Whether an inbound is reachable from the internet depends on its listening address, system routing, and firewall configuration, not on its protocol category.

## Configuration Methods

### Proxy Ports

The top-level `port`, `socks-port`, `mixed-port`, `redir-port`, and `tproxy-port` options are suitable when only one fixed set of proxy ports is required.

See [Proxy Ports](./port.md).

### TUN

The top-level `tun` configuration captures system traffic and is suitable for automatic routing, DNS hijacking, and per-application routing.

See [TUN](./tun.md).

### Listeners

`listeners` can create multiple inbounds with different types, listening addresses, and ports. Common fields such as `rule` and `proxy` can be configured independently for each inbound.

```{.yaml linenums="1"}
listeners:
  - name: socks5-in-1
    type: socks
    port: 10808
    #listen: 0.0.0.0 # Listens on 0.0.0.0 by default
    # rule: sub-rule-name1 # Uses rules by default; falls back to rules if the sub-rule is not found
    # proxy: proxy # When non-empty, sends inbound traffic directly to the specified proxy
    # udp: false # true by default
```

See [Common Listener Fields](./listeners/index.md).

!!! warning
    `listen: 0.0.0.0` listens on every network interface. HTTP, SOCKS, and Mixed inbounds without TLS should not be exposed directly to the internet.

## Listener Types

| Purpose | Types |
|---|---|
| Application proxies | [HTTP](./listeners/http.md), [SOCKS](./listeners/socks.md), [Mixed](./listeners/mixed.md) |
| Transparent proxying and system capture | [Redirect](./listeners/redirect.md), [TProxy](./listeners/tproxy.md), [TUN](./listeners/tun.md) |
| Port forwarding | [Tunnel](./listeners/tunnel.md) |
| Encrypted proxy servers | [Shadowsocks](./listeners/ss.md), [VMess](./listeners/vmess.md), [VLESS](./listeners/vless.md), [Trojan](./listeners/trojan.md), [AnyTLS](./listeners/anytls.md), [Snell](./listeners/snell.md), [Mieru](./listeners/mieru.md), [Sudoku](./listeners/sudoku.md), [TrustTunnel](./listeners/trusttunnel.md) |
| QUIC / UDP servers | [TUIC v4](./listeners/tuic-v4.md), [TUIC v5](./listeners/tuic-v5.md), [Hysteria2](./listeners/hysteria2.md), [Hysteria2 Realm](./listeners/hysteria2-realm.md), [ShadowQUIC](./listeners/shadowquic.md) |

!!! note
    The TUN Listener is intended for advanced use cases. Most users should prefer the top-level [TUN](./tun.md) configuration.

## Shortcut Inbounds

`ss-config`, `vmess-config`, and `tuic-server` remain available for quickly creating their corresponding inbounds. These shortcut inbounds are equivalent to their corresponding Listeners, and incoming traffic is processed like other inbounds according to the configured `mode`.

```{.yaml linenums="1"}
ss-config: ss://2022-blake3-aes-256-gcm:vlmpIPSyHH6f4S8WVPdRIHIlzmB+GIRfoH3aNJ/t9Gg=@:23456
vmess-config: vmess://1:9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68@:12345

tuic-server:
  enable: true
  listen: 127.0.0.1:10443
  token: # TUIC v4; cannot be used together with users
    - TOKEN
  # users: # TUIC v5; cannot be used together with token
  #   00000000-0000-0000-0000-000000000000: PASSWORD-0
  #   00000000-0000-0000-0000-000000000001: PASSWORD-1
  certificate: ./server.crt
  private-key: ./server.key
  congestion-controller: bbr
  max-idle-time: 15000
  authentication-timeout: 1000
  alpn:
    - h3
  max-udp-relay-packet-size: 1500
```

!!! note
    Shortcut inbounds are useful for existing configurations and simple use cases. Prefer `listeners` when a new configuration needs additional protocol options or independent routing.
