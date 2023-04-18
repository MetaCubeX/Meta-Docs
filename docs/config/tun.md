# Tun 模式

## 配置示例

<pre class="language-yaml"><code class="lang-yaml">tun:
  enable: true
  stack: system
  device: utun0
  auto-detect-interface: true
  auto-route: true
  dns-hijack:
  - any:53
  # mtu: 9000
  # strict_route: true
  # inet4_route_address:
  # - 0.0.0.0/1
  # - 128.0.0.0/1
<strong>  # inet6_route_address:
</strong>  # - "::/1"
  # - "8000::/1"
  # endpoint_independent_nat: false
  # include_uid:
  # - 0
  # include_uid_range:
  # - 1000-99999
  # exclude_uid:
  #- 1000
  # exclude_uid_range:
  # - 1000-99999
  # include_android_user:
  # - 0
  # - 10
  # include_package:
  # - com.android.chrome
  # exclude_package:
  # - com.android.captiveportallogin
</code></pre>

### enable

是否启用 tun 模式来路由全局流量。

可选：`true/false`

```
enable: true
```

### stack

tun模式堆栈,如无使用问题,建议使用 `system` 栈;macOS 用户推荐 `gvisor`栈,iOS无法使用 `system`栈

可选： `system/gvisor/lwip`

```
stack: system
```

!!! note "协议栈之间的区别"

    * system 使用系统协议栈,可以提供更稳定/全面的 tun 体验,且占用相对其他堆栈更低。
    * gvisor 通过在用户空间中实现网络协议栈,可以提供更高的安全性和隔离性,同时可以避免操作系统内核和用户空间之间的切换,从而在特定情况下具有更好的网络处理性能。
    * lwip 即 lightweight IP,是一款专为嵌入式系统设计的TCP/IP协议栈,采用了单线程的事件驱动模型,性能表现可能不如`system/gvisor`协议栈。
    * [性能测试](tun.md#tun-de-xie-yi-zhan-wang-luo-hui-huan-ce-shi)

### device

指定tun网卡名称,macOS设备只能使用utun开头的网卡名

```
device: utun0
```

### auto-route

自动设置全局路由,可以自动将全局流量路由进入tun网卡。

可选：`true/false`

```
auto-route: true
```

### auto-detect-interface

自动选择流量出口接口,多出口网卡同时连接的设备建议手动指定出口网卡

可选：`true/false`

```
auto-detect-interface: true
```

### dns-hijack

dns劫持,一般设置为 `any:53` 即可, 即劫持所有53端口的udp流量

```yaml
dns-hijack:
 - any:53
 - tcp://any:53
```

{% hint style="warning" %}
macOS 无法自动劫持发往局域网的dns请求

ANDROID 如开启私人dns则无法自动劫持dns请求

LINUX 如果 systemd-resolved 开启无法自动劫持dns请求

### strict\_route

严格路由,它可以防止地址泄漏,并使 DNS 劫持在 Android 和使用 systemd-resolved 的 Linux 上工作,但你的设备将无法其他设备被访问

可选：`true/false`

```
strict_route: true
```

### mtu

最大传输单元, 值为 `1-65534`, 会影响极限状态下的速率,一般用户默认即可。

```
mtu: 9000
```

### inet4\_route\_address

启用 `auto_route`时使用自定义ipv4路由而不是默认路由,一般无需配置。

```
inet4_route_address:
  - 0.0.0.0/1
  - 128.0.0.0/1
```

### inet6\_route\_address

启用 `auto_route`时使用自定义ipv6路由而不是默认路由,一般无需配置。

```
inet6_route_address:
  - "::/1"
  - "8000::/1"
```

### endpoint\_independent\_nat

启用独立于端点的NAT,性能可能会略有下降,所以不建议在不需要的时候开启。

<pre><code><strong>endpoint_independent_nat: false
</strong></code></pre>

### include\_uid

限制被路由用户,默认不限制。

!!! note
    UID 规则仅在Linux下被支持,并且需要 `auto_route`

```
include_uid:
  - 0
```

### include\_uid\_range

限制被路由的的用户范围

```
include_uid_range:
  - 1000-99999
```

### exclude\_uid

排除路由的的用户

```
exclude_uid:
  - 1000
```

### exclude\_uid\_range

排除路由的的用户范围

```
exclude_uid_range:
  - 1000-99999
```

### include\_android\_user

限制被路由的 Android 用户

!!! note
    Android用户和应用规则仅在Android下被支持,并且需要 `auto_route`

| 常用用户 | ID  |
| -------- | --- |
| 机主     | 0   |
| 手机分身 | 10  |
| 应用多开 | 999 |

```
include_android_user:
  - 0
  - 10
```

include\_package

限制被路由的Android应用包名

```
include_package:
  - com.android.chrome
```

### exclude\_package

排除路由的Android应用包名

```
exclude_package:
  - com.android.captiveportallogin
```

## Tun 的协议栈网络回环测试

从上到下分别为 `system/gvisor/lwip`,仅供参考,平台为linux,Windows和macOS可能会有差异

CPU为amd r7 1700 3.6Ghz,内存8G 3600mhz C16,硬盘为PM981A

![iperf](../assets/image/tun/iperf.png)
