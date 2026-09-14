# TPROXY

```{.yaml linenums="1"}
listeners:
- name: tproxy-in
  type: tproxy
  port: 7894
  listen: 0.0.0.0
  udp: true
```

## [General Fields](./index.md)

!!! warning "System prerequisite: src_valid_mark must be 0"

    TPROXY requires kernel-side setup (fwmark + policy routing table + local route, usually configured
    by a front-end) and **`net.ipv4.conf.all.src_valid_mark` must be `0`**. Note that this sysctl is
    evaluated as `max(conf/all, conf/<iface>)`, so setting it to 0 for a single interface (e.g. `br-lan`)
    is not enough. If it is `1`, forwarded connections get their fwmark used in the reverse-path lookup of
    `fib_validate_source()`, land on the local route (`RTN_LOCAL`) and are **silently dropped** as martian
    sources:

    - Symptom: **all LAN clients lose Internet access** (TCP/UDP black hole, DNS still works) while the
      **router itself keeps working**, and app/core/firewall logs and drop counters show nothing unusual;
    - Common source: Tailscale 1.98+ (default `NetfilterMode=on`) writes `1` on startup, see
      [tailscale/tailscale#19796](https://github.com/tailscale/tailscale/issues/19796);
    - Fix: `tailscale set --netfilter-mode=off` and reboot, or `sysctl -w net.ipv4.conf.all.src_valid_mark=0`;
    - Check: `sysctl -n net.ipv4.conf.all.src_valid_mark` should print `0`.

## Protocol Configuration

### udp

Whether to listen for UDP.
