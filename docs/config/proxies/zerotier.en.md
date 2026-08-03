# ZeroTier

```{.yaml linenums="1"}
proxies:
  - name: zerotier
    type: zerotier
    network: "0123456789abcdef"
    # state-dir: ./zerotier-node
    # planet: ./planet
    # mtu: 1400
    # physical-mtu: 1432
    # ip-stack:
    #   mode: auto
    #   congestion-controller: cubic
    # primary-port: 0
    # secondary-port: 0
    # tcp-fallback-mode: auto
    # tcp-fallback-relay: 204.80.128.1:443
    # remote-trace-target: "0123456789"
    # remote-trace-level: 0
    # low-bandwidth: false
    # encrypted-hello: false
    # orbit:
    #   - world: "0123456789abcdef"
    #     seed: "0123456789"
    udp: true
    # remote-dns-resolve: true
    # dns: [10.147.17.1]
    # dialer-proxy: "ss1"
    # interface-name: "WLAN"
    # routing-mark: 6666
    # ip-version: ipv4-prefer
```

[Common Fields](./index.md)

## network

ZeroTier network ID.

## state-dir

Optional. Persistent node identity directory; defaults to an isolated directory derived from the network ID and outbound name.

## planet

Optional. Path to a private Planet file, used to replace the built-in Earth Planet.

## mtu

Optional. Local MTU override value; cannot exceed the MTU provided by the ZeroTier controller.

## physical-mtu

Optional. ZeroTier UDP payload MTU. Valid range: `510-10324`, default: `1432`.

## ip-stack

Optional IP stack configuration.

### ip-stack-mode

IP stack mode. Available values: `auto`, `gvisor`, `mips`. The default value is `auto`.

### ip-stack-congestion-controller

TCP congestion control algorithm. Available values: `cubic`, `reno`, `bbr`. The default value is `cubic`.

This option has no effect when using the gVisor IP stack.

## primary-port

Optional. Primary UDP port; set to `0` to automatically select an available port.

## secondary-port

Optional. Secondary UDP port; `0` for automatic selection, `-1` to disable.

## tcp-fallback-mode

Optional. TCP fallback mode. Available values:

* `auto`: Use relay after UDP connection failure.
* `force`: Use relay only.
* `disable`: Disable TCP relay fallback.

Default: `auto`.

## tcp-fallback-relay

Optional. TCP fallback relay server address.

## remote-trace-target

Optional. ZeroTier node ID to receive global diagnostic traces.

## remote-trace-level

Optional. Remote diagnostic trace level. Available values:

* `0`: Normal
* `10`: Verbose
* `15`: Rules
* `20`: Debug
* `30`: Insane

## low-bandwidth

Optional. Enable low-bandwidth mode. Default: `false`.

## encrypted-hello

Optional. Default: `false`.

## orbit

Optional. Configure a list of private moon root servers.

### orbit-world

Moon World ID, must be a 16-digit hexadecimal string.

### orbit-seed

Moon root node ID, must be a 10-digit number.

## udp

Whether to enable UDP. Default: `true`.

## remote-dns-resolve

Optional. Whether to resolve destination domain names through ZeroTier using controller-provided DNS servers.

## dns

Optional. Override controller-provided DNS servers; one or more addresses can be specified.

## dialer-proxy

Optional. Carry ZeroTier network traffic through another outbound proxy, where `ss1` is the name of another proxy node.

## interface-name

Optional. Specify the network interface name used by ZeroTier.

## routing-mark

Optional. Set the Linux routing mark.

## ip-version

Optional. Specify IP protocol version preference.
