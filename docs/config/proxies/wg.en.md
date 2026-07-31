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
  udp: true
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
  udp: true
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
