# NTP

```{.yaml linenums="1"}
ntp:
  enable: true
  write-to-system: true
  server: time.apple.com
  port: 123
  interval: 30
  # dialer-proxy: DIRECT

```
## enable

Whether to enable the NTP service.

## write-to-system

Whether to sync to the system time. Requires root or administrator privileges to run.

## server

NTP server address. Defaults to `time.apple.com`.

## port

NTP server port. Defaults to `123`.

## interval

Time synchronization interval, in minutes. Defaults to `30`.

## dialer-proxy

Optional, defaults to `DIRECT`.

!!! note
    By default, NTP connects directly to the target without using outbound proxies or routing rules. Use `dialer-proxy` to force a specific proxy.

