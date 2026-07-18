Clash.Meta supports traffic inbounds and can operate as a server.

## LAN Inbounds

These inbounds listen for LAN traffic and are suitable for unencrypted transport:

```{.yaml linenums="1"}
listeners:
  - name: socks5-in-1
    type: socks
    port: 10808
    #listen: 0.0.0.0 # Listens on 0.0.0.0 by default
    # rule: sub-rule-name1 # Uses rules by default; falls back to rules if the sub-rule is not found
    # proxy: proxy # When non-empty, sends inbound traffic directly to the specified proxy
    # udp: false # true by default

  - name: http-in-1
    type: http
    port: 10809
    listen: 0.0.0.0
    # rule: sub-rule-name1 # Uses rules by default; falls back to rules if the sub-rule is not found
    # proxy: proxy # When non-empty, sends inbound traffic directly to the specified proxy; the proxy name must be valid

  - name: mixed-in-1
    type: mixed # Combined HTTP(S) and SOCKS proxy
    port: 10810
    listen: 0.0.0.0
    # rule: sub-rule-name1 # Uses rules by default; falls back to rules if the sub-rule is not found
    # proxy: proxy # When non-empty, sends inbound traffic directly to the specified proxy; the proxy name must be valid
    # udp: false # true by default

  - name: reidr-in-1
    type: redir
    port: 10811
    listen: 0.0.0.0
    # rule: sub-rule-name1 # Uses rules by default; falls back to rules if the sub-rule is not found
    # proxy: proxy # When non-empty, sends inbound traffic directly to the specified proxy; the proxy name must be valid

  - name: tproxy-in-1
    type: tproxy
    port: 10812
    listen: 0.0.0.0
    # rule: sub-rule-name1 # Uses rules by default; falls back to rules if the sub-rule is not found
    # proxy: proxy # When non-empty, sends inbound traffic directly to the specified proxy; the proxy name must be valid
    # udp: false # true by default

  - name: tunnel-in-1
    type: tunnel
    port: 10816
    listen: 0.0.0.0
    # rule: sub-rule-name1 # Uses rules by default; falls back to rules if the sub-rule is not found
    # proxy: proxy # When non-empty, sends inbound traffic directly to the specified proxy; the proxy name must be valid
    network: [tcp, udp]
    target: target.com

  - name: tun-in-1
    type: tun
    # rule: sub-rule-name1 # Uses rules by default; falls back to rules if the sub-rule is not found
    # proxy: proxy # When non-empty, sends inbound traffic directly to the specified proxy; the proxy name must be valid
    stack: system # gvisor / lwip
    dns-hijack:
      - 0.0.0.0:53 # DNS endpoints to hijack
    # auto-detect-interface: false # Automatically detect the outbound interface
    # auto-route: false # Configure the routing table
    # mtu: 9000 # Maximum transmission unit
    inet4-address: # The IPv4 subnet must be configured manually
      - 198.19.0.1/30
    inet6-address: # The IPv6 subnet must be configured manually
      - "fdfe:dcba:9877::1/126"
    # strict-route: true # Route all connections through TUN to prevent leaks; this can make the device inaccessible to other devices
    #    inet4-route-address: # Use custom routes instead of the default route when auto-route is enabled
    #      - 0.0.0.0/1
    #      - 128.0.0.0/1
    #    inet6-route-address: # Use custom routes instead of the default route when auto-route is enabled
    #      - "::/1"
    #      - "8000::/1"
    # endpoint-independent-nat: false # Enable endpoint-independent NAT
    # include-uid: # UID rules are supported on Linux only and require auto-route
    # - 0
    # include-uid-range: # Restrict routed user ranges
    # - 1000-99999
    # exclude-uid: # Exclude users from routing
    #- 1000
    # exclude-uid-range: # Exclude user ranges from routing
    # - 1000-99999

    # Android user and application rules are supported on Android only
    # and require auto-route

    # include-android-user: # Restrict routed Android users
    # - 0
    # - 10
    # include-package: # Restrict routed Android application packages
    # - com.android.chrome
    # exclude-package: # Exclude Android application packages from routing
    # - com.android.captiveportallogin
```

## Internet Inbounds

The following inbounds provide encrypted traffic transport:

```{.yaml linenums="1"}
listeners:
  - name: shadowsocks-in-1
    type: shadowsocks
    port: 10813
    listen: 0.0.0.0
    # rule: sub-rule-name1 # Uses rules by default; falls back to rules if the sub-rule is not found
    # proxy: proxy # When non-empty, sends inbound traffic directly to the specified proxy; the proxy name must be valid
    password: vlmpIPSyHH6f4S8WVPdRIHIlzmB+GIRfoH3aNJ/t9Gg=
    cipher: 2022-blake3-aes-256-gcm

  - name: vmess-in-1
    type: vmess
    port: 10814
    listen: 0.0.0.0
    # rule: sub-rule-name1 # Uses rules by default; falls back to rules if the sub-rule is not found
    # proxy: proxy # When non-empty, sends inbound traffic directly to the specified proxy; the proxy name must be valid
    users:
      - username: 1
        uuid: 9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68
        alterId: 1

  - name: tuic-in-1
    type: tuic
    port: 10815
    listen: 0.0.0.0
    # rule: sub-rule-name1 # Uses rules by default; falls back to rules if the sub-rule is not found
    # proxy: proxy # When non-empty, sends inbound traffic directly to the specified proxy; the proxy name must be valid
    # token:    # TUIC v4; cannot be used together with users
    #   - TOKEN
    # users:    # TUIC v5; cannot be used together with token
    #   00000000-0000-0000-0000-000000000000: PASSWORD-0
    #   00000000-0000-0000-0000-000000000001: PASSWORD-1
    #  certificate: ./server.crt
    #  private-key: ./server.key
    #  congestion-controller: bbr
    #  max-idle-time: 15000
    #  authentication-timeout: 1000
    #  alpn:
    #    - h3
    #  max-udp-relay-packet-size: 1500
```

!!! note
    When `proxy` is non-empty, inbound traffic is sent to the specified [proxy](../proxies/index.md).

    When the configured [sub-rule](../sub-rule.md) does not exist, `rules` is used directly.

## Inbound Configuration

Inbound configuration is equivalent to a Listener. Incoming traffic is matched according to `mode`, just like traffic from SOCKS, mixed, and other inbounds.

```{.yaml linenums="1"}
# Shadowsocks and VMess inbound configuration; traffic is matched according to mode like SOCKS and mixed inbounds
ss-config: ss://2022-blake3-aes-256-gcm:vlmpIPSyHH6f4S8WVPdRIHIlzmB+GIRfoH3aNJ/t9Gg=@:23456
vmess-config: vmess://1:9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68@:12345

# TUIC server inbound; traffic is matched according to mode like SOCKS and mixed inbounds
tuic-server:
 enable: true
 listen: 127.0.0.1:10443
 token:    # TUIC v4; cannot be used together with users
   - TOKEN
 users:    # TUIC v5; cannot be used together with token
   00000000-0000-0000-0000-000000000000: PASSWORD-0
   00000000-0000-0000-0000-000000000001: PASSWORD-1
 certificate: ./server.crt
 private-key: ./server.key
 congestion-controller: bbr
 max-idle-time: 15000
 authentication-timeout: 1000
 alpn:
   - h3
 max-udp-relay-packet-size: 1500
```
