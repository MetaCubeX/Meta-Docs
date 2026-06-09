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
    accept-routes: true
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

## accept-routes

Optional, whether to accept subnet routes published in the Tailnet.

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
