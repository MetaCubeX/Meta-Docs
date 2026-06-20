# TUN

```yaml
tun:
  enable: true
  stack: system
  auto-route: true
  auto-redirect: true
  auto-detect-interface: true
  dns-hijack:
    - any:53
    - tcp://any:53
  device: utun0
  mtu: 9000
  strict-route: true
  gso: true
  gso-max-size: 65536
  inet6-address: fdfe:dcba:9876::1/126
  udp-timeout: 300
  iproute2-table-index: 2022
  iproute2-rule-index: 9000
  endpoint-independent-nat: false
  loopback_address:
    - 10.7.0.1
  route-address-set:
    - ruleset-1
  route-exclude-address-set:
    - ruleset-2
  route-address:
    - 0.0.0.0/1
    - 128.0.0.0/1
    - "::/1"
    - "8000::/1"
  route-exclude-address:
    - 192.168.0.0/16
    - fc00::/7
  include-interface:
    - eth0
  exclude-interface:
    - eth1
  include-uid:
    - 0
  include-uid-range:
    - 1000:9999
  exclude-uid:
    - 1000
  exclude-uid-range:
    - 1000:9999
  include-android-user:
    - 0
    - 10
  include-package:
    - com.android.chrome
  exclude-package:
    - com.android.captiveportallogin

## Legacy Syntax
  inet4-route-address:
    - 0.0.0.0/1
    - 128.0.0.0/1
  inet6-route-address:
    - "::/1"
    - "8000::/1"
  inet4-route-exclude-address:
    - 192.168.0.0/16
  inet6-route-exclude-address:
    - fc00::/7
```

## enable

Enables TUN mode.

## stack

Specifies the TUN network stack. Unless you encounter compatibility issues, the `mixed` stack is recommended. Default: `gvisor`.

Available values: `system`, `gvisor`, `mixed`

!!! note "Differences Between Network Stacks"
    * `system` uses the operating system's native network stack, providing a more stable and comprehensive TUN experience while consuming fewer resources than the other stacks.
    * `gvisor` implements the network stack in user space, offering better security and isolation. It also avoids kernel/user-space context switching, which can improve networking performance in certain scenarios.
    * `mixed` is a hybrid stack that uses the `system` stack for TCP and the `gvisor` stack for UDP, generally providing the best overall experience.
    * See the [Simple Performance Benchmark](tun.md#tun_1).
    * If a firewall is enabled, the `system` and `mixed` stacks may not function unless the kernel is allowed through the firewall:
    * **Windows:** Settings → Windows Security → Allow an app through firewall → Allow the kernel.
    * **macOS:** Usually no configuration is required, as signed applications are allowed by default. If issues occur with the firewall enabled, try: System Settings → Network → Firewall → Options → Add the Mihomo application.
    * **Linux:** Normally no configuration is required. If needed, allow outbound traffic on the TUN interface (assuming the TUN interface is named `Mihomo`):`sudo iptables -A OUTPUT -o Mihomo -j ACCEPT`

## device

Specifies the name of the TUN interface.

On macOS, the interface name must begin with `utun`.

## auto-route

Automatically configures system routing so that global traffic is routed through the TUN interface.

## auto-redirect

Linux only. Automatically configures `iptables`/`nftables` to redirect TCP connections. Requires `auto-route` to be enabled.

*On Android:*

Only local IPv4 connections are redirected. To share your VPN connection through a hotspot or tethering, use VPNHotspot.

*On Linux:*

With `auto-route` enabled, `auto-redirect` now works on routers without additional configuration.

## auto-detect-interface

Automatically detects the outbound network interface.

On devices with multiple active network interfaces, specifying the outbound interface manually is recommended.

## dns-hijack

Hijacks matching DNS requests and redirects them to the internal DNS module. If no protocol is specified, `udp://` is assumed.

!!! warning
    * On **macOS** and **Windows**, DNS requests sent to the local network cannot be hijacked automatically.
    * On **Android**, DNS hijacking does not work if **Private DNS** is enabled.

## strict-route

Enables strict routing rules when `auto-route` is enabled.

*On Linux:*

* Blocks unsupported networks.
* Routes all connections through the TUN interface.

This helps prevent traffic leaks and enables DNS hijacking on Android.

*On Windows:*

* Adds firewall rules to prevent DNS leaks caused by Windows' standard multi-homed DNS resolution behavior.

This may cause certain applications (such as VirtualBox) to stop working in some situations.

## mtu

Specifies the Maximum Transmission Unit (MTU).

This mainly affects throughput under extreme network conditions. The default value is suitable for most users.

## gso

Enables Generic Segmentation Offload (GSO).

Linux only.

## gso-max-size

Specifies the maximum size of a GSO packet.

## inet6-address

Specifies the IPv6 address assigned to the TUN interface.

!!! note
    * At startup, Mihomo checks whether any system network interface has IPv6 enabled. If none is found, this feature is disabled automatically. To force-enable the TUN IPv6 address, set the environment variable `SKIP_SYSTEM_IPV6_CHECK=1`.
    * You must also set the top-level `ipv6` option to `true` for this setting to take effect.

## udp-timeout

UDP NAT timeout, in seconds.

Default: `300` (5 minutes).

## iproute2-table-index

The routing table index used by `auto-route`.

Default: `2022`.

## iproute2-rule-index

The starting rule index used by `auto-route`.

Default: `9000`.

## endpoint-independent-nat

Enables Endpoint-Independent NAT (EIM).

This may slightly reduce performance, so it is not recommended unless required.

## loopback_address

Redirects outbound TCP connections destined for external IP addresses to a local loopback address, allowing them to be handled directly by a locally listening proxy.

!!! tip
    Setting this option to `10.7.0.1` provides the same behavior as SideStore/StosVPN.

## route-address-set

Adds the destination IP CIDR rules from the specified rule set to the firewall. Traffic that does **not** match these rules bypasses TUN routing.

Linux only. Requires `nftables`, `auto-route`, and `auto-redirect`.

!!! warning
    Incompatible with any `routing-mark` configuration.

## route-exclude-address-set

Adds the destination IP CIDR rules from the specified rule set to the firewall. Matching traffic bypasses TUN routing.

Linux only. Requires `nftables`, `auto-route`, and `auto-redirect`.

!!! warning
    Incompatible with any `routing-mark` configuration.

## route-address

When `auto-route` is enabled, routes only the specified network ranges instead of using the default route.

In most cases, this does not need to be configured.

## route-exclude-address

Excludes the specified network ranges from TUN routing when `auto-route` is enabled.

## include-interface

Limits TUN routing to the specified network interfaces.

By default, there is no restriction.

Cannot be used together with `exclude-interface`.

## exclude-interface

Excludes the specified network interfaces from TUN routing.

Cannot be used together with `include-interface`.

## include-uid

Specifies Linux user IDs whose traffic will be routed through TUN.

Traffic from users not listed will bypass TUN.

By default, there is no restriction.

!!! note
    UID-based routing is supported only on Linux and requires `auto-route`.

## include-uid-range

Specifies ranges of Linux user IDs whose traffic will be routed through TUN.

## exclude-uid

Excludes specific Linux user IDs from TUN routing.

## exclude-uid-range

Excludes ranges of Linux user IDs from TUN routing.

## include-android-user

Specifies Android user IDs whose traffic will be routed through TUN.

Traffic from users not listed will bypass TUN.

!!! note
    Android user and application rules are supported only on Android and require `auto-route`.

| Common User                  | ID  |
| ---------------------------- | --- |
| Owner                        | 0   |
| Work Profile / Clone Profile | 10  |
| App Clone                    | 999 |

## include-package

Specifies Android application package names whose traffic will be routed through TUN.

Applications not listed will bypass TUN.

## exclude-package

Excludes Android application package names from TUN routing.

## Legacy Options (Deprecated)

### inet4-route-address

When `auto-route` is enabled, routes only the specified IPv4 network ranges instead of using the default route.

In most cases, this does not need to be configured.

### inet6-route-address

When `auto-route` is enabled, routes only the specified IPv6 network ranges instead of using the default route.

In most cases, this does not need to be configured.

### inet4-route-exclude-address

Excludes the specified IPv4 network ranges from TUN routing when `auto-route` is enabled.

### inet6-route-exclude-address

Excludes the specified IPv6 network ranges from TUN routing when `auto-route` is enabled.

## TUN Network Stack Loopback Performance Test

The following benchmark compares the `system`, `gvisor`, and `lwip` stacks (from top to bottom).

The results are for Linux and are provided for reference only. Performance on Windows and macOS may differ.

![iperf](../../assets/image/tun/iperf.png)
