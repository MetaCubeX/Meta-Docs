```yaml
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
  # strict_route: true
  # inet4_route_address:由
  # - 0.0.0.0/1
  # - 128.0.0.0/1
  # inet6_route_address:
  # - "::/1"
  # - "8000::/1"
  # endpoint_independent_nat: false
  # include_uid:
  # - 0
  # include_uid_range:
  # - 1000-99999
  # exclude_uid:
  # - 1000
  # exclude_uid_range:
  # - 1000-99999
  # include_android_user:
  # - 0
  # - 10
  # include_package:
  # - com.android.chrome
  # exclude_package:
  # - com.android.captiveportallogin
```
