# ZeroTier

```{.yaml linenums="1"}
proxies:
- name: "zerotier"
  type: zerotier
  network: "0123456789abcdef"
  state-dir: ./zerotier-node
  planet: ./planet
  mtu: 1400
  physical-mtu: 1432
  primary-port: 0
  secondary-port: 0
  tcp-fallback-mode: auto
  tcp-fallback-relay: 204.80.128.1:443
  remote-trace-target: "0123456789"
  remote-trace-level: 0
  low-bandwidth: false
  encrypted-hello: false
  orbit:
    - world: "0123456789abcdef"
      seed: "0123456789"
  udp: true
  remote-dns-resolve: true
  dns: [10.147.17.1]
  dialer-proxy: "ss1"
  interface-name: "WLAN"
  routing-mark: 6666
  ip-version: ipv4-prefer
```

[Common fields](./index.md)

## network

Required, a 16-digit hexadecimal ZeroTier Network ID.

## state-dir

Optional, the directory used to persist the node identity. By default, an isolated directory is created under `zerotier` for each Network ID and outbound name.

## planet

Optional, path to a custom planet file. When set, it replaces the built-in Earth planet.

## mtu

Optional, local MTU limit. Values range from `1280` to `10000`; the effective MTU cannot exceed the value assigned by the controller.

## physical-mtu

Optional, ZeroTier UDP payload MTU. Values range from `510` to `10324`; default: `1432`.

## primary-port

Optional, primary UDP port. Values range from `0` to `65535`; `0` selects an available port automatically.

## secondary-port

Optional, secondary UDP port. Values range from `-1` to `65535`; `0` selects an available port automatically, and `-1` disables the secondary port.

## tcp-fallback-mode

Optional, TCP fallback mode. Default: `auto`.

Available values:

- `auto`: use a TCP relay after direct UDP fails.
- `force`: use only a TCP relay.
- `disable`: disable the TCP relay.

## tcp-fallback-relay

Optional, TCP fallback relay address in `host:port` form. Defaults to the public relay operated by ZeroTier.

## remote-trace-target

Optional, 10-digit hexadecimal ZeroTier node ID that receives global diagnostic traces.

## remote-trace-level

Optional, remote diagnostic trace level. Values range from `0` to `30`.

Available values: `0` (normal)/`10` (verbose)/`15` (rules)/`20` (debug)/`30` (insane).

## low-bandwidth

Optional, whether to reduce background traffic and network configuration refresh frequency. Default: `false`.

## encrypted-hello

Optional, whether to enable protocol-13 extended encryption for outgoing HELLO packets. Default: `false`.

## orbit

Optional, list of federated root worlds (moons).

### orbit.world

Required, a 16-digit hexadecimal moon world ID.

### orbit.seed

Required, a 10-digit hexadecimal node ID of a moon root.

## remote-dns-resolve

Optional, whether to resolve destination names through ZeroTier using the DNS servers supplied by the controller. Default: `false`.

## dns

Optional, only effective when `remote-dns-resolve` is `true`. When set, it overrides the DNS servers supplied by the controller.
