---
hide:
  # - navigation
  - toc
---

## Using systemd

- Download the binary executable file from [releases](https://github.com/MetaCubeX/mihomo/releases).

- Rename the downloaded binary executable file to `mihomo` and move it to `/usr/local/bin/`.

- Run Mihomo as a daemon.

Use the following commands to copy the Mihomo binary file to /usr/local/bin and the configuration file to /etc/mihomo:

```shell
cp mihomo /usr/local/bin
cp config.yaml /etc/mihomo
```

Create a systemd configuration file `/etc/systemd/system/mihomo.service`:

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

Reload systemd using the following command:

```shell
systemctl daemon-reload
```

Enable the Mihomo service:

```shell
systemctl enable mihomo
```

Start Mihomo immediately with the following command:

```shell
systemctl start mihomo
```

Reload Mihomo with the following command:

```shell
systemctl reload mihomo
```

Check the status of Mihomo with the following command:

```shell
systemctl status mihomo
```

Check the running logs of Mihomo with the following command:

```shell
journalctl -u mihomo -o cat -e
```

Or

```shell
journalctl -u mihomo -o cat -f
```
