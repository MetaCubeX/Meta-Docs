# GOST Relay

```{.yaml linenums="1"}
proxies:
  - name: "gost-relay-hop"
    type: gost-relay
    server: relay.example.com
    port: 443
    udp: true
    tls: true
    # mux: true
    # ws: true
    # ws-opts:
    #   path: /ws
    #   headers:
    #     Host: relay.example.com
    # sni: relay.example.com
    # username: user
    # password: pass
    # client-fingerprint: chrome
    # fingerprint: xxxx
    # certificate: ./client.crt
    # private-key: ./client.key
    # skip-cert-verify: true
    # name-cert-verify: example.com
    # dialer-proxy: "ss1"
    # interface-name: "WLAN"
    # routing-mark: 6666
    # ip-version: ipv4-prefer
```

!!! note
    GOST Relay is used to carry other proxies (such as ss/vmess/vless/trojan) through a GOST relay server. Use it as the `dialer-proxy` of an upper-layer proxy; both the connections and UDP traffic of that proxy will be sent through the relay.

    Two modes are supported:

    * **Dynamic mode** (default, `forward: false`): the relay connects to the target address requested by the upper-layer proxy. In this mode the upper-layer proxy's `server` is usually an address reachable from the relay server side (for example `127.0.0.1` on the relay server).
    * **Forward mode** (`forward: true`): the relay server connects to its own configured forwarding target; the upper-layer proxy's address is only used to obtain a stream from the relay.

    Like other proxy types, GOST Relay only starts connecting when traffic is directed to the node via rules, policy groups, or other routing methods.

[Common Fields](./index.md)

## name

Required, proxy name. Must be unique.

## type

Required, fixed to `gost-relay`.

## server

Required, GOST relay server address.

## port

Required, GOST relay server port, in the range 1–65535.

## forward

Optional, whether to use forward mode. Default: `false` (dynamic mode).

* `false` (dynamic mode): the relay connects to the target address requested by the upper-layer proxy; use it together with `dialer-proxy` to carry other outbound proxies.
* `true` (forward mode): the relay server connects to its configured forwarding target; the upper-layer proxy's address is only used to request a stream from the relay.

## udp

Optional, whether to enable UDP. Default: `false`.

## ws

Optional, whether to enable WebSocket transport. Default: `false`.

Combined with `tls` and `mux`, this forms several relay transports:

* `ws` + `tls`: `relay+wss`
* `ws` + `mux`: `relay+mws`
* `ws` + `tls` + `mux`: `relay+mwss`

## ws-opts

Optional, WebSocket transport options.

### path

Optional, WebSocket path. GOST's default is `/ws`; set it to match the relay listener's `ws.path`.

### headers

Optional, WebSocket request headers as string key-value pairs. Commonly used to set the `Host` header, for example `Host: relay.example.com`.

```{.yaml linenums="1"}
ws-opts:
  path: /ws
  headers:
    Host: relay.example.com
```

## tls

Optional, whether to enable TLS. Default: `false`.

## mux

Optional, whether to enable multiplexing (smux). Default: `false`. When `tls` is enabled and `ws` is disabled, this selects `relay+mtls`.

## sni

Optional, TLS SNI. Defaults to the `Host` in `ws-opts.headers`, or `server` if absent.

## username

Optional, relay authentication username.

## password

Optional, relay authentication password.

## skip-cert-verify

Optional, whether to skip server certificate verification. Default: `false`.

## name-cert-verify

Optional, only changes the certificate DNSName verification target, not SNI.

## fingerprint

Optional, SHA256 fingerprint of the server certificate, used to verify the certificate.

## certificate

Optional, client certificate file path, used for mTLS.

## private-key

Optional, client private key file path, used for mTLS.

## client-fingerprint

Optional, client TLS fingerprint (e.g. `chrome`).

## dialer-proxy

Optional, identifier of an outbound proxy used to carry the connection to the GOST relay, where `ss1` is the name of another proxy node.

In dynamic mode, using the GOST Relay as the `dialer-proxy` of an upper-layer proxy lets that proxy (ss/vmess/vless/trojan, etc.) send its traffic through the relay.

## interface-name

Optional, specifies the outbound network interface.

## routing-mark

Optional, configures fwmark on Linux.

## ip-version

Optional, specifies the IP version used for outbound connections.

Available values: `dual`/`ipv4`/`ipv6`/`ipv4-prefer`/`ipv6-prefer`.
