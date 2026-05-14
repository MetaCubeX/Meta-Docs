# WireGuard

## Simplified syntax

If there is only one peer, you can use the simplified syntax.

```{.yaml linenums="1"}
proxies:
- name: "wg"
  type: wireguard
  private-key: eCtXsJZ27+4PbhDkHnB923tkUn2Gj59wZw5wFA75MnU=
  server: 162.159.192.1
  port: 2480
  ip: 172.16.0.2
  ipv6: fd01:5ca1:ab1e:80fa:ab85:6eea:213f:f4a5
  public-key: Cr8hWlKvtDt7nrvf+f0brNQQzabAqrjfBvas9pmowjo=
  allowed-ips: ['0.0.0.0/0']
  # pre-shared-key: 31aIhAPwktDGpH4JDhA8GNvjFXEf/a6+UaQRyOAiyfM=
  # reserved: [209,98,59]  # String format is also valid, such as "U4An"
  # persistent-keepalive: 0
  udp: true
  # mtu: 1408
  # dialer-proxy: "ss1"  # Identifier of an outbound proxy. When non-empty, connections are sent through the specified proxy/proxy-group
  # remote-dns-resolve: true # Force remote DNS resolution, default is false
  # dns: [ 1.1.1.1, 8.8.8.8 ] # Effective only when remote-dns-resolve is true
  # If present, enables AmneziaWG functionality
  # amnezia-wg-option:
  #   jc: 5
  #   jmin: 500
  #   jmax: 501
  #   s1: 30
  #   s2: 40
  #   s3: 50                                            # AmneziaWG v1.5 and v2
  #   s4: 5                                             # AmneziaWG v1.5 and v2
  #   h1: 123456                                        # AmneziaWG v1.0 and v1.5
  #   h2: 67543                                         # AmneziaWG v1.0 and v1.5
  #   h3: 123123                                        # AmneziaWG v1.0 and v1.5
  #   h4: 32345                                         # AmneziaWG v1.0 and v1.5
  #   h1: 123456-123500                                 # AmneziaWG v2.0 only
  #   h2: 67543-67550                                   # AmneziaWG v2.0 only
  #   h3: 123123-123200                                 # AmneziaWG v2.0 only
  #   h4: 32345-32350                                   # AmneziaWG v2.0 only
  #   i1: <b 0xf6ab3267fa><c><b 0xf6ab><t><r 10><wt 10> # AmneziaWG v1.5 and v2
  #   i2: <b 0xf6ab3267fa><r 100>                       # AmneziaWG v1.5 and v2
  #   i3: ""                                            # AmneziaWG v1.5 and v2
  #   i4: ""                                            # AmneziaWG v1.5 and v2
  #   i5: ""                                            # AmneziaWG v1.5 and v2
  #   j1: <b 0xffffffff><c><b 0xf6ab><t><r 10>          # AmneziaWG v1.5 only (removed in v2)
  #   j2: <c><b 0xf6ab><t><wt 1000>                     # AmneziaWG v1.5 only (removed in v2)
  #   j3: <t><b 0xf6ab><c><r 10>                        # AmneziaWG v1.5 only (removed in v2)
  #   itime: 60                                         # AmneziaWG v1.5 only (removed in v2)
```

## Full syntax

The full syntax can specify multiple peers.

When using multiple peers, each peer's `allowed-ips` needs to be distinct. In this case, top-level fields such as `server`, `port`, `public-key`, `pre-shared-key`, and `reserved` are ignored, but `private-key` is still specified at the top level.

```{.yaml linenums="1"}
proxies:
- name: "wg"
  type: wireguard
  ip: 172.16.0.2
  ipv6: fd01:5ca1:ab1e:80fa:ab85:6eea:213f:f4a5
  private-key: eCtXsJZ27+4PbhDkHnB923tkUn2Gj59wZw5wFA75MnU=
  peers:
    - server: 162.159.192.1
      port: 2480
      public-key: Cr8hWlKvtDt7nrvf+f0brNQQzabAqrjfBvas9pmowjo=
      allowed-ips: ['0.0.0.0/0']
      # pre-shared-key: 31aIhAPwktDGpH4JDhA8GNvjFXEf/a6+UaQRyOAiyfM=
      # reserved: [209,98,59]  # String format is also valid, such as "U4An"
  udp: true
  # mtu: 1408
  # dialer-proxy: "ss1"  # Identifier of an outbound proxy. When non-empty, connections are sent through the specified proxy/proxy-group
  # remote-dns-resolve: true # Force remote DNS resolution, default is false
  # dns: [ 1.1.1.1, 8.8.8.8 ] # Effective only when remote-dns-resolve is true
```

[Common fields](./index.md)

### ip

IPv4 address used by the local machine in the WireGuard network.

### ipv6

Optional field, IPv6 address used by the local machine in the WireGuard network.

### private-key

Base64-encoded WireGuard client private key.

You can use `wg genkey | tee privatekey | wg pubkey > publickey` to generate a usable public/private key pair.

### public-key

Base64-encoded WireGuard server public key.

### allowed-ips

Optional field, restricts which client IP ranges are forwarded by the server. Usually `['0.0.0.0/0']` can be used.

### pre-shared-key

Optional field, pre-shared key.

### reserved

Optional field, value of the WireGuard protocol reserved field. Required by some WARP nodes.

### persistent-keepalive

Optional field, periodically sends packets to keep the connection persistent.

### mtu

Optional field, sets the MTU value.

### remote-dns-resolve

Optional field, whether to force remote DNS resolution. Default: `false`.

### dns

Optional field. Takes effect when `remote-dns-resolve` is true and specifies DNS servers used for remote resolution.

## Translating from a standard WireGuard configuration file

Assume the following standard WireGuard configuration file:

```ini
[Interface]
Address = <local network IP>
ListenPort = <local listening port>
PrivateKey = <local private key>
DNS = <DNS to use>
MTU = <preset MTU>

[Peer]
AllowedIPs = <forwarded IP range>
Endpoint = <remote address>:<remote port>
PublicKey = <remote public key>
```

The corresponding clash node configuration is:

```{.yaml linenums="1"}
- name: "wg"
  type: wireguard
  ip: <local network IP, fill IPv4 here>
  ipv6: <local network IP, fill IPv6 here>  # Delete if there is no v6 address
  private-key: <local private key>
  peers:
    - server: <remote address>
      port: <remote port>
      public-key: <remote public key>
      allowed-ips: ['0.0.0.0/0']     # Traffic splitting is handled by clash
      # reserved: [209,98,59]        # Fill in if needed
  udp: true
  mtu: <preset MTU>               # Set as needed, delete if not needed
  remote-dns-resolve: true        # Set as needed, delete if not needed
  dns: <DNS to use>               # Set as needed, delete if not needed
```
