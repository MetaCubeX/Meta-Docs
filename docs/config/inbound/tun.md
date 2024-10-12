# TUN

```{.yaml linenums="1"}
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
  udp-timeout: 300
  iproute2-table-index: 2022
  iproute2-rule-index: 9000
  endpoint-independent-nat: false
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

## 旧写法
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

启用 tun

## stack

tun 模式堆栈，如无使用问题，建议使用 `mixed`栈，默认 `gvisor`

可用值： `system/gvisor/mixed`

!!! note "协议栈之间的区别"
    * `system` 使用系统协议栈，可以提供更稳定/全面的 tun 体验，且占用相对其他堆栈更低
    * `gvisor` 通过在用户空间中实现网络协议栈，可以提供更高的安全性和隔离性，同时可以避免操作系统内核和用户空间之间的切换，从而在特定情况下具有更好的网络处理性能
    * `mixed` 混合堆栈，tcp 使用 `system`栈，udp 使用 `gvisor`栈，使用体验可能相对更好
    * [简单性能测试](tun.md#tun_1)
    * Windows 下如果打开了防火墙需要放行一下内核（设置 -> Windows 安全中心 -> 允许应用通过防火墙 -> 选中 verge-mihomo），否则无法使用 `system` 和 `mixed` 协议栈

## device

指定 tun 网卡名称，MacOS 设备只能使用 utun 开头的网卡名

## auto-route

自动设置全局路由，可以自动将全局流量路由进入 tun 网卡。

## auto-redirect

仅支持 Linux，自动配置 iptables/nftables 以重定向 TCP 连接，需要`auto-route`已启用

*在 Android 中*：

仅转发本地 IPv4 连接。要通过热点或中继共享您的 VPN 连接，请使用 [VPNHotspot](https://github.com/Mygod/VPNHotspot)。

*在 Linux 中*：

带有 auto-route 的 auto-redirect 现在可以在路由器上按预期工作，无需干预。

## auto-detect-interface

自动选择流量出口接口，多出口网卡同时连接的设备建议手动指定出口网卡

## dns-hijack

dns 劫持，将匹配到的连接导入内部 [dns](../dns/index.md) 模块，不书写协议则为 udp://

!!! warning ""
    * 在 `MacOS`/`Windows` 无法自动劫持发往局域网的 dns 请求
    * 在 `Android` 如开启 `私人dns` 则无法自动劫持 dns 请求

## strict-route

启用 `auto-route` 时执行严格的路由规则

*在 Linux 中*:

* 让不支持的网络无法到达
* 将所有连接路由到 tun

它可以防止地址泄漏，并使 DNS 劫持在 Android 上工作。

*在 Windows 中*:

* 添加防火墙规则以阻止 Windows
  的 [普通多宿主 DNS 解析行为](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197552%28v%3Dws.10%29)
  造成的 DNS 泄露

它可能会使某些应用程序（如 VirtualBox）在某些情况下无法正常工作。

## mtu

最大传输单元，会影响极限状态下的速率，一般用户默认即可。

## gso

启用通用分段卸载，仅支持 Linux

## gso-max-size

数据块的最大长度

## udp-timeout

UDP NAT 过期时间，以秒为单位，默认为 300(5 分钟)

## iproute2-table-index

`auto-route` 生成的 iproute2 路由表索引，默认使用 `2022`

## iproute2-rule-index

`auto-route` 生成的 iproute2 规则起始索引，默认使用 `9000`

## endpoint-independent-nat

启用独立于端点的 NAT，性能可能会略有下降，所以不建议在不需要的时候开启。

## route-address-set

将指定规则集中的目标 IP CIDR 规则添加到防火墙，不匹配的流量将绕过路由
仅支持 Linux，且需要 nftables 以及`auto-route` 和 `auto-redirect` 已启用。

!!! warning ""
    与任意配置中的 routing-mark 冲突

## route-exclude-address-set

将指定规则集中的目标 IP CIDR 规则添加到防火墙，匹配的流量将绕过路由
仅支持 Linux，且需要 nftables 以及`auto-route` 和 `auto-redirect` 已启用。

!!! warning ""
    与任意配置中的 routing-mark 冲突

## route-address

启用 `auto-route`时路由自定义路由网段而不是默认路由，一般无需配置。

## route-exclude-address

启用 `auto-route` 时排除自定义网段

## include-interface

限制被路由的接口，默认不限制，与 `exclude-interface` 冲突，不可一起配置

## exclude-interface

排除路由的接口，与 `include-interface` 冲突，不可一起配置

## include-uid

包含的用户，使其被 Tun 路由流量，未被配置的用户不会被 Tun 路由流量，默认不限制

!!! note ""
    UID 规则仅在 Linux 下被支持,并且需要 `auto-route`

## include-uid-range

包含的用户范围，使其被 Tun 路由流量，未被配置的用户不会被 Tun 路由流量

## exclude-uid

排除用户，使其避免被 Tun 路由流量

## exclude-uid-range

排除用户范围，使其避免被 Tun 路由流量

## include-android-user

包含的 Android 用户，使其被 Tun 路由流量，未被配置的用户不会被 Tun 路由流量

!!! note ""
    Android 用户和应用规则仅在 Android 下被支持,并且需要 `auto-route`

| 常用用户 | ID  |
| -------- | --- |
| 机主     | 0   |
| 手机分身 | 10  |
| 应用多开 | 999 |

## include-package

包含的 Android 应用包名，使其被 Tun 路由流量，未配置的应用包不会被 Tun 路由流量

## exclude-package

排除 Android 应用包名，使其避免被 Tun 路由流量

## 旧写法，即将废弃

### inet4-route-address

启用 `auto-route`时路由自定义网段而不是默认路由，一般无需配置。

### inet6-route-address

启用 `auto-route`时路由自定义网段而不是默认路由，一般无需配置。

### inet4-route-exclude-address

启用 `auto-route` 时排除自定义网段

### inet6-route-exclude-address

启用 `auto-route` 时排除自定义网段

## Tun 的协议栈网络回环测试

从上到下分别为 `system/gvisor/lwip`,仅供参考，平台为 linux,Windows 和 MacOS 可能会有差异

![iperf](../../assets/image/tun/iperf.png)
