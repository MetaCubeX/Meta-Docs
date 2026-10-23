# Tailscale

```{.yaml linenums="1"}
proxies:
  - name: "tailscale"
    type: tailscale
    hostname: mihomo
    auth-key: tskey-auth-xxxx
    control-url: https://controlplane.tailscale.com
    state-dir: ./tailscale
    ephemeral: false
    udp: true
    port: 41641
    accept-routes: true
    advertise-routes:
      - 192.168.1.0/24
    exit-node: 100.64.0.1
    exit-node-allow-lan-access: true
    dialer-proxy: "ss1"
    interface-name: "WLAN"
    routing-mark: 6666
    ip-version: ipv4-prefer
```

!!! note
    Like other proxy protocols, Tailscale is an optional proxy node. Traffic will only go through Tailscale if you direct it via rules, policy groups, or other routing methods.

    Furthermore, all types of proxies only start connecting when the first matching connection is triggered (i.e., they do not start synchronously with Mihomo). Therefore, a timeout on the first attempt to access the Tailscale node is normal; please try again until Tailscale loads successfully.

## name

Required, proxy name. Must be unique.

## type

Required, fixed to `tailscale`.

## hostname

Optional, Tailscale device name. If omitted, it is handled by `tsnet`.

## auth-key

Optional, login key for Tailscale or Headscale. If omitted, an interactive login URL is printed in the logs on first startup.

## control-url

Optional, custom Tailscale control server or Headscale URL.

## state-dir

Optional, `tsnet` state directory. Default: `tailscale`.

## ephemeral

Optional, whether to log in as an ephemeral node. Default: `false`.

## udp

Optional, whether to enable UDP. Default: `false`.

## port

Optional, UDP listening port for Tailscale peer traffic. Default: `41641`. Set to `0` to use a random port. If the firewall blocks inbound UDP, allow the actual listening port to help establish direct peer connections. This setting is independent of the `udp` option for proxied traffic.

## accept-routes

Optional, whether to accept subnet routes published in the Tailnet.

## advertise-routes

Optional. Advertise local networks this machine can reach, so other nodes can access them through this node.

```yaml
advertise-routes:
  - 192.168.1.0/24
  - fd12:3456:789a::/64
```

Other nodes use the routes only after they are approved in the Tailscale or Headscale admin console. Omitting the field leaves previously saved routes unchanged. Set it to `[]` to clear advertised routes.

Prefixes are normalized to the network address. For example, `192.168.1.5/24` is advertised as `192.168.1.0/24`. Invalid CIDRs and duplicate prefixes fail when the configuration is loaded.

Advertising both `0.0.0.0/0` and `::/0` makes this node an exit node, which cannot be combined with `exit-node`.

Inbound TCP/UDP for these prefixes is forwarded from this machine by the userspace network stack, using the same `interface-name`, `routing-mark`, and `dialer-proxy`.

## exit-node

Optional, use the specified exit node. You can specify a node IP, and `auto:any` is also supported.

When the target is not within Tailscale routes, the connection fails directly and does not fall back to direct. To access the public internet, configure an available `exit-node`, or accept subnet routes that cover the target network.

## exit-node-allow-lan-access

Optional, whether to allow access to the local LAN when using an exit node.

## dialer-proxy

Optional, identifier of an outbound proxy. When non-empty, the specified proxy is used for Tailscale control plane, DERP/STUN, and other service connections.

## interface-name

Optional, specifies the outbound network interface.

## routing-mark

Optional, configures fwmark on Linux.

## ip-version

Optional, specifies the IP version used for outbound connections.

Available values: `dual`/`ipv4`/`ipv6`/`ipv4-prefer`/`ipv6-prefer`.
