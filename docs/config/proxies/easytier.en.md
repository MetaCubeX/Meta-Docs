# EasyTier

```{.yaml linenums="1"}
proxies:
  - name: "easytier"
    type: easytier
    network-name: example
    network-secret: secret
    # hostname: mihomo
    # ipv4: 10.144.0.1/24
    # dhcp: true
    peers:
      - tcp://192.0.2.10:11010
      - udp://192.0.2.11:11010
    # secure-mode: true
    # local-private-key: "base64-x25519-private-key"
    # local-public-key: "base64-x25519-public-key"
    # listeners: ["tcp://0.0.0.0:11010"]
    # no-listener: true
    # mapped-listeners: ["tcp://203.0.113.10:11010"]
    # exit-nodes: ["10.144.0.1"]
    # proxy-networks: ["10.0.0.0/24"]
    # instance-name: mihomo-easytier
    # state-dir: ./easytier
    udp: true
    # accept-dns: false
    # enable-exit-node: false
    # enable-encryption: true
    # encryption-algorithm: aes-gcm
    # private-mode: false
    # latency-first: false
    # disable-p2p: false
    # enable-kcp-proxy: false
    # disable-kcp-input: false
    # enable-quic-proxy: false
    # disable-quic-input: false
    # mtu: 1380
    # tld-dns-zone: et.net.
    # dialer-proxy: "ss1"
    # interface-name: "WLAN"
    # routing-mark: 6666
    # ip-version: ipv4-prefer
```

!!! note
    The EasyTier overlay network only supports IPv4; `ip-version` only affects the underlying peer connections and does not enable overlay IPv6.

    Like other proxy types, EasyTier only starts when traffic is directed to the node via rules, policy groups, or other routing methods.

[Common Fields](./index.md)

## name

Required, proxy name. Must be unique.

## type

Required, must be `easytier`.

## network-name

Required, EasyTier network name.

## network-secret

Optional, EasyTier network secret. Nodes with the same network name and secret belong to the same virtual network.

## hostname

Optional, hostname of this node in the overlay network, used for MagicDNS resolution. If not specified, it is handled by EasyTier.

## ipv4

Optional, overlay IPv4 address of this node in CIDR format, e.g. `10.144.0.1/24`.

## dhcp

Optional, whether to obtain the overlay IPv4 address via DHCP. Enabled by default when `ipv4` is empty, otherwise disabled by default; if not obtained, the instance will have no overlay IPv4.

## peers

Optional, list of entry nodes to connect to initially; multiple entries can be configured, supporting `tcp://`, `udp://` and other URI schemes.

Does not implicitly connect to `public.easytier.top` by default; at least one entry is required when `listeners` is empty.

Appending the `peer-public-key` query parameter to the URI pins the public key of a shared node, preventing man-in-the-middle attacks:

```{.yaml linenums="1"}
peers:
  - "tcp://relay.example.com:11010?peer-public-key=base64-x25519-public-key"
```

## secure-mode

Optional, whether to enable Noise end-to-end encryption. It is enabled automatically when `local-private-key`, `local-public-key`, or the `peer-public-key` in a peer URI is configured.

## local-private-key

Optional, local X25519 private key (Base64), used to pin the local identity and avoid a changing public key across restarts.

## local-public-key

Optional, local X25519 public key (Base64), usually derivable from the private key; must be configured together with `local-private-key`.

## listeners

Optional, list of URIs this node listens on, used to accept connections from other nodes, e.g. `tcp://0.0.0.0:11010`.

## no-listener

Optional, whether to disable listening, default: `true` (i.e. `listeners` is empty). Cannot be combined with `listeners`; when explicitly set to `false` and `listeners` is empty, the default listening address `tcp://0.0.0.0:11010` is used.

## mapped-listeners

Optional, list of external mapped listening addresses announced to other nodes, useful for port forwarding scenarios, e.g. `tcp://203.0.113.10:11010`.

## exit-nodes

Optional, list of exit nodes to use (overlay IPv4 addresses of peer nodes), used to forward traffic to the specified nodes for external access.

## proxy-networks

Optional, list of remote subnets (CIDR) to be accessed through the EasyTier network, e.g. `10.0.0.0/24`.

## instance-name

Optional, EasyTier instance name, defaults to the proxy name.

## state-dir

Optional, state directory, default: `easytier/<proxy name>`, used to persist the `instance_id`.

## udp

Optional, whether to enable UDP. Default: `false`.

## accept-dns

Optional, whether to accept DNS configurations distributed by the network.

## enable-exit-node

Optional, whether to act as an exit node for other nodes. Default: `false`.

## enable-encryption

Optional, whether to enable encryption between nodes. Default: `true`.

## encryption-algorithm

Optional, encryption algorithm. Default: `aes-gcm`.

## private-mode

Optional, whether to enable private mode. Default: `false`.

## latency-first

Optional, whether to enable latency-first mode (prefers the path with the lowest latency instead of the fewest hops). Default: `false`.

## disable-p2p

Optional, whether to disable P2P connections (traffic goes through relays only). Default: `false`.

## enable-kcp-proxy

Optional, whether to enable the KCP proxy (forwards TCP traffic over KCP). Default: `false`.

## disable-kcp-input

Optional, whether to disable KCP input. Default: `false`.

## enable-quic-proxy

Optional, whether to enable the QUIC proxy (forwards TCP traffic over QUIC). Default: `false`.

## disable-quic-input

Optional, whether to disable QUIC input. Default: `false`.

## mtu

Optional, MTU override value. Default: `1380`.

## tld-dns-zone

Optional, TLD DNS zone used by MagicDNS. Default: `et.net.`.

In the DNS configuration, `et://<proxy name>` can be used to resolve overlay A/PTR records; it is recommended to use it in `nameserver-policy`.

## dialer-proxy

Optional. Carry EasyTier network traffic through another outbound proxy, where `ss1` is the name of another proxy node.

## interface-name

Optional. Specify the network interface name used by EasyTier.

## routing-mark

Optional. Set the Linux routing mark.

## ip-version

Optional. Specify IP protocol version preference (only affects the underlying peer connections).
