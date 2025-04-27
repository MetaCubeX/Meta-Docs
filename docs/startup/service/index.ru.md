---
hide:
  # - navigation
  - toc
---

## Использование systemd

- Скачайте исполняемый двоичный файл из раздела [releases](https://github.com/MetaCubeX/mihomo/releases)

- Переименуйте скачанный двоичный файл в `mihomo` и переместите его в `/usr/local/bin/`

- Запустите mihomo в режиме демона.

Используйте следующие команды, чтобы скопировать бинарный файл mihomo в /usr/local/bin, а конфигурационный файл в /etc/mihomo:

```shell
cp mihomo /usr/local/bin
cp config.yaml /etc/mihomo
```

Создайте файл конфигурации systemd `/etc/systemd/system/mihomo.service`:

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

Используйте следующую команду для перезагрузки systemd:

```shell
systemctl daemon-reload
```

Включите сервис mihomo:

```shell
systemctl enable mihomo
```

Используйте следующую команду, чтобы немедленно запустить mihomo:

```shell
systemctl start mihomo
```

Используйте следующую команду для перезагрузки конфигурации mihomo:

```shell
systemctl reload mihomo
```

Используйте следующую команду для проверки состояния mihomo:

```shell
systemctl status mihomo
```

Используйте следующую команду для просмотра журнала работы mihomo:

```shell
journalctl -u mihomo -o cat -e
```

Или

```shell
journalctl -u mihomo -o cat -f
``` 