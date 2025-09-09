# TUN

注意，listeners中的tun仅提供给高级用户使用，普通用户应使用顶层配置中的tun

```{.yaml linenums="1"}
listeners:
- name: tun-in
  type: tun
  stack: system
  dns-hijack:
  - 0.0.0.0:53
  # auto-detect-interface: false
  # auto-route: false
  # mtu: 9000
  inet4-address:
  - 198.19.0.1/30
  inet6-address:
  - "fdfe:dcba:9877::1/126"
  # strict-route: true
  # inet4-route-address:由
  # - 0.0.0.0/1
  # - 128.0.0.0/1
  # inet6-route-address:
  # - "::/1"
  # - "8000::/1"
  # endpoint-independent-nat: false
  # include-uid:
  # - 0
  # include-uid-range:
  # - 1000-99999
  # exclude-uid:
  # - 1000
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
