# TPROXY

```{.yaml linenums="1"}
listeners:
- name: tproxy-in
  type: tproxy
  port: 7894
  listen: 0.0.0.0
  udp: true
```

## [通用字段](./index.md)

!!! warning "系统前置要求：src_valid_mark 必须为 0"

    TPROXY 需要内核侧的配合（fwmark + 策略路由表 + 本地路由，通常由前端自动配置），并且
    **`net.ipv4.conf.all.src_valid_mark` 必须为 `0`**。注意该 sysctl 取 `max(conf/all, conf/<iface>)`，
    所以只把某一个接口（如 `br-lan`）设为 0 是无效的。若它被置为 `1`，转发进来的连接会在内核
    `fib_validate_source()` 的源地址反查中带着 fwmark 命中本地路由表，判定为 `RTN_LOCAL` 后被当作
    martian source **静默丢弃**：

    - 现象：**局域网客户端全部无法上网**（TCP/UDP 黑洞、DNS 正常），而**路由器本机正常**，
      且插件/核心/防火墙的日志与丢包计数全部正常，极其难以排查；
    - 常见来源：Tailscale 1.98+（默认 `NetfilterMode=on`）启动时会写入 `1`，见
      [tailscale/tailscale#19796](https://github.com/tailscale/tailscale/issues/19796)；
    - 解决：`tailscale set --netfilter-mode=off` 后重启，或 `sysctl -w net.ipv4.conf.all.src_valid_mark=0`；
    - 检查：`sysctl -n net.ipv4.conf.all.src_valid_mark` 应为 `0`。

## 协议配置

### udp

是否监听 UDP
