---
hide:
  # - navigation
  - toc
---

## 使用 systemd

- 下载二进制可执行文件 [releases](https://github.com/MetaCubeX/mihomo/releases)

- 将下载的二进制可执行文件重名名为 `mihomo` 并移动到 `/usr/local/bin/`

- 以守护进程的方式，运行 mihomo。

使用以下命令将 Clash 二进制文件复制到 /usr/local/bin, 配置文件复制到 /etc/mihomo:

```shell
cp mihomo /usr/local/bin
cp config.yaml /etc/mihomo
```

创建 systemd 配置文件 `/etc/systemd/system/mihomo.service`:

```ini
[Unit]
Description=mihomo Daemon, Another Clash Kernel.
After=network.target NetworkManager.service systemd-networkd.service iwd.service

[Service]
Type=simple
LimitNPROC=500
LimitNOFILE=1000000
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_RAW CAP_NET_BIND_SERVICE CAP_SYS_TIME CAP_SYS_PTRACE CAP_DAC_READ_SEARCH CAP_DAC_OVERRIDE
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_RAW CAP_NET_BIND_SERVICE CAP_SYS_TIME CAP_SYS_PTRACE CAP_DAC_READ_SEARCH CAP_DAC_OVERRIDE
Restart=always
ExecStartPre=/usr/bin/sleep 1s
ExecStart=/usr/local/bin/mihomo -d /etc/mihomo
ExecReload=/bin/kill -HUP $MAINPID

[Install]
WantedBy=multi-user.target
```

使用以下命令重新加载 systemd:

```shell
systemctl daemon-reload
```

启用 mihomo 服务：

```shell
systemctl enable mihomo
```

使用以下命令立即启动 mihomo:

```shell
systemctl start mihomo
```

使用以下命令使 mihomo 重新加载：

```shell
systemctl reload mihomo
```

使用以下命令检查 mihomo 的运行状况：

```shell
systemctl status mihomo
```

使用以下命令检查 mihomo 的运行日志：

```shell
journalctl -u mihomo -o cat -e
```

或

```shell
journalctl -u mihomo -o cat -f
```
