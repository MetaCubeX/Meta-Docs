---
hide:
  # - navigation
  - toc
---

# Clash 服务运行

## 使用 systemd

- 下载二进制可执行文件 [releases](https://github.com/MetaCubeX/Clash.Meta/releases)

- 将下载的二进制可执行文件重名名为 `Clash-Meta` 并移动到 `/usr/local/bin/`

- 以守护进程的方式，运行 Clash-Meta。

使用以下命令将 Clash 二进制文件复制到 /usr/local/bin, 配置文件复制到 /etc/Clash-Meta:

```shell
cp Clash-Meta /usr/local/bin
cp config.yaml /etc/Clash-Meta
```

创建 systemd 配置文件 `/etc/systemd/system/Clash-Meta.service`:

```ini
[Unit]
Description=Clash-Meta Daemon, Another Clash Kernel.
After=network.target NetworkManager.service systemd-networkd.service iwd.service

[Service]
Type=simple
LimitNPROC=500
LimitNOFILE=1000000
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_RAW CAP_NET_BIND_SERVICE CAP_SYS_TIME
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_RAW CAP_NET_BIND_SERVICE CAP_SYS_TIME
Restart=always
ExecStartPre=/usr/bin/sleep 1s
ExecStart=/usr/local/bin/Clash-Meta -d /etc/Clash-Meta
ExecReload=/bin/kill -HUP $MAINPID

[Install]
WantedBy=multi-user.target
```

使用以下命令重新加载 systemd:

```shell
systemctl daemon-reload
```

启用 clash-meta 服务：

```shell
systemctl enable Clash-Meta
```

使用以下命令立即启动 Clash:

```shell
systemctl start Clash-Meta
```

使用以下命令使 Clash-Meta 重新加载:

```shell
systemctl reload Clash-Meta
```

使用以下命令检查 Clash 的运行状况和日志:

```shell
systemctl status clash
journalctl -xe
```
