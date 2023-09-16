# Tun 模式

## 配置示例

```yaml
tun:
  enable: true
  stack: system
  auto-route: true
  auto-detect-interface: true
  dns-hijack:
    - any:53
  # device: utun0
  # mtu: 9000
  # strict-route: true
  # inet4-route-address:
  # - 0.0.0.0/1
  # - 128.0.0.0/1
  # inet6-route-address:
  # - "::/1"
  # - "8000::/1"
  # endpoint-independent_nat: false
  # include-uid:
  # - 0
  # include-uid-range:
  # - 1000-99999
  # exclude-uid:
  #- 1000
  # exclude-uid-range:
  # - 1000-99999
  # include-android-user:
  # - 0
  # - 10
  # include-package:
  # - com.android.chrome
  # exclude-package:
  # - com.android.captiveportallogin
```

### enable

是否启用 tun 模式来路由全局流量。

可选：`true/false`

```yaml
enable: true
```

### stack

tun 模式堆栈,如无使用问题,建议使用 `system` 栈;MacOS 用户推荐 `gvisor`栈

可选： `system/gvisor/lwip`

```yaml
stack: system
```

!!! note "协议栈之间的区别"

    * system 使用系统协议栈,可以提供更稳定/全面的 tun 体验,且占用相对其他堆栈更低。
    * gvisor 通过在用户空间中实现网络协议栈,可以提供更高的安全性和隔离性,同时可以避免操作系统内核和用户空间之间的切换,从而在特定情况下具有更好的网络处理性能。
    * lwip 即 lightweight IP,是一款专为嵌入式系统设计的TCP/IP协议栈,采用了单线程的事件驱动模型,性能表现可能不如`system/gvisor`协议栈。
    * [性能测试](tun.md#tun_1)

### device

指定 tun 网卡名称,MacOS 设备只能使用 utun 开头的网卡名

```yaml
device: utun0
```

### auto-route

自动设置全局路由,可以自动将全局流量路由进入 tun 网卡。

可选：`true/false`

```yaml
auto-route: true
```

### auto-detect-interface

自动选择流量出口接口,多出口网卡同时连接的设备建议手动指定出口网卡

可选：`true/false`

```yaml
auto-detect-interface: true
```

### dns-hijack

dns 劫持,一般设置为 `any:53` 即可, 即劫持所有 53 端口的 udp 流量

```yaml
dns-hijack:
  - any:53
  - tcp://any:53
```

{% hint style="warning" %}
MACOS 无法自动劫持发往局域网的 dns 请求

ANDROID 如开启私人 dns 则无法自动劫持 dns 请求

LINUX 如果 systemd-resolved 开启无法自动劫持 dns 请求

### strict-route

严格路由,它可以防止地址泄漏,并使 DNS 劫持在 Android 和使用 systemd-resolved 的 Linux 上工作,但你的设备将无法被其他设备访问

可选：`true/false`

```yaml
strict-route: true
```

### mtu

最大传输单元, 值为 `1-65534`, 会影响极限状态下的速率,一般用户默认即可。

```yaml
mtu: 9000
```

### inet4-route-address

启用 `auto_route`时使用自定义 ipv4 路由而不是默认路由,一般无需配置。

```yaml
inet4-route-address:
  - 0.0.0.0/1
  - 128.0.0.0/1
```

### inet6-route-address

启用 `auto-route`时使用自定义 ipv6 路由而不是默认路由,一般无需配置。

```yaml
inet6-route-address:
  - "::/1"
  - "8000::/1"
```

### endpoint-independent-nat

启用独立于端点的 NAT,性能可能会略有下降,所以不建议在不需要的时候开启。

```yaml
endpoint-independent-nat: false
```

### include-uid

限制被路由用户,默认不限制。

!!! note
UID 规则仅在 Linux 下被支持,并且需要 `auto_route`

```yaml
include-uid:
  - 0
```

### include-uid-range

限制被路由的的用户范围

```yaml
include-uid-range:
  - 1000-99999
```

### exclude-uid

排除路由的的用户

```yaml
exclude-uid:
  - 1000
```

### exclude-uid-range

排除路由的的用户范围

```yaml
exclude-uid-range:
  - 1000-99999
```

### include-android-user

限制被路由的 Android 用户

!!! note
Android 用户和应用规则仅在 Android 下被支持,并且需要 `auto_route`

| 常用用户 | ID  |
| -------- | --- |
| 机主     | 0   |
| 手机分身 | 10  |
| 应用多开 | 999 |

```yaml
include-android-user:
  - 0
  - 10
```

### include-package

限制被路由的 Android 应用包名

```yaml
include-package:
  - com.android.chrome
```

### exclude-package

排除路由的 Android 应用包名

```yaml
exclude-package:
  - com.android.captiveportallogin
```

## Tun 的协议栈网络回环测试

从上到下分别为 `system/gvisor/lwip`,仅供参考,平台为 linux,Windows 和 MacOS 可能会有差异

CPU 为 amd r7 1700 3.6Ghz,内存 8G 3600mhz C16,硬盘为 PM981A

![iperf](../assets/image/tun/iperf.png)
