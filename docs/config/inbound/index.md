Clash.Meta 使用流量入站，可以作为服务器。

## 局域网入站

用于监听局域网流量的入站，适用于无加密传输：

```{.yaml linenums="1"}
listeners:
  - name: socks5-in-1
    type: socks
    port: 10808
    #listen: 0.0.0.0 # 默认监听 0.0.0.0
    # rule: sub-rule-name1 # 默认使用 rules,如果未找到 sub-rule 则直接使用 rules
    # proxy: proxy # 如果不为空则直接将该入站流量交由指定proxy处理
    # udp: false # 默认 true

  - name: http-in-1
    type: http
    port: 10809
    listen: 0.0.0.0
    # rule: sub-rule-name1 # 默认使用 rules,如果未找到 sub-rule 则直接使用 rules
    # proxy: proxy # 如果不为空则直接将该入站流量交由指定proxy处理(当proxy不为空时,这里的proxy名称必须合法,否则会出错)

  - name: mixed-in-1
    type: mixed #  HTTP(S) 和 SOCKS 代理混合
    port: 10810
    listen: 0.0.0.0
    # rule: sub-rule-name1 # 默认使用 rules,如果未找到 sub-rule 则直接使用 rules
    # proxy: proxy # 如果不为空则直接将该入站流量交由指定proxy处理(当proxy不为空时,这里的proxy名称必须合法,否则会出错)
    # udp: false # 默认 true

  - name: reidr-in-1
    type: redir
    port: 10811
    listen: 0.0.0.0
    # rule: sub-rule-name1 # 默认使用 rules,如果未找到 sub-rule 则直接使用 rules
    # proxy: proxy # 如果不为空则直接将该入站流量交由指定proxy处理(当proxy不为空时,这里的proxy名称必须合法,否则会出错)

  - name: tproxy-in-1
    type: tproxy
    port: 10812
    listen: 0.0.0.0
    # rule: sub-rule-name1 # 默认使用 rules,如果未找到 sub-rule 则直接使用 rules
    # proxy: proxy # 如果不为空则直接将该入站流量交由指定proxy处理(当proxy不为空时,这里的proxy名称必须合法,否则会出错)
    # udp: false # 默认 true

  - name: tunnel-in-1
    type: tunnel
    port: 10816
    listen: 0.0.0.0
    # rule: sub-rule-name1 # 默认使用 rules,如果未找到 sub-rule 则直接使用 rules
    # proxy: proxy # 如果不为空则直接将该入站流量交由指定proxy处理(当proxy不为空时,这里的proxy名称必须合法,否则会出错)
    network: [tcp, udp]
    target: target.com

  - name: tun-in-1
    type: tun
    # rule: sub-rule-name1 # 默认使用 rules,如果未找到 sub-rule 则直接使用 rules
    # proxy: proxy # 如果不为空则直接将该入站流量交由指定proxy处理(当proxy不为空时,这里的proxy名称必须合法,否则会出错)
    stack: system # gvisor / lwip
    dns-hijack:
      - 0.0.0.0:53 # 需要劫持的 DNS
    # auto-detect-interface: false # 自动识别出口网卡
    # auto-route: false # 配置路由表
    # mtu: 9000 # 最大传输单元
    inet4-address: # 必须手动设置ipv4地址段
      - 198.19.0.1/30
    inet6-address: # 必须手动设置ipv6地址段
      - "fdfe:dcba:9877::1/126"
    # strict-route: true # 将所有连接路由到tun来防止泄漏,但你的设备将无法其他设备被访问
    #    inet4-route-address: # 启用 auto-route 时使用自定义路由而不是默认路由
    #      - 0.0.0.0/1
    #      - 128.0.0.0/1
    #    inet6-route-address: # 启用 auto-route 时使用自定义路由而不是默认路由
    #      - "::/1"
    #      - "8000::/1"
    # endpoint-independent-nat: false # 启用独立于端点的 NAT
    # include-uid: # UID 规则仅在 Linux 下被支持,并且需要 auto-route
    # - 0
    # include-uid-range: # 限制被路由的的用户范围
    # - 1000-99999
    # exclude-uid: # 排除路由的的用户
    #- 1000
    # exclude-uid-range: # 排除路由的的用户范围
    # - 1000-99999

    # Android 用户和应用规则仅在 Android 下被支持
    # 并且需要 auto-route

    # include-android-user: # 限制被路由的 Android 用户
    # - 0
    # - 10
    # include-package: # 限制被路由的 Android 应用包名
    # - com.android.chrome
    # exclude-package: # 排除被路由的 Android 应用包名
    # - com.android.captiveportallogin
```

## 互联网入站

用于加密传输流量的入站如下：

```{.yaml linenums="1"}
listeners:
  - name: shadowsocks-in-1
    type: shadowsocks
    port: 10813
    listen: 0.0.0.0
    # rule: sub-rule-name1 # 默认使用 rules,如果未找到 sub-rule 则直接使用 rules
    # proxy: proxy # 如果不为空则直接将该入站流量交由指定proxy处理(当proxy不为空时,这里的proxy名称必须合法,否则会出错)
    password: vlmpIPSyHH6f4S8WVPdRIHIlzmB+GIRfoH3aNJ/t9Gg=
    cipher: 2022-blake3-aes-256-gcm

  - name: vmess-in-1
    type: vmess
    port: 10814
    listen: 0.0.0.0
    # rule: sub-rule-name1 # 默认使用 rules,如果未找到 sub-rule 则直接使用 rules
    # proxy: proxy # 如果不为空则直接将该入站流量交由指定proxy处理(当proxy不为空时,这里的proxy名称必须合法,否则会出错)
    users:
      - username: 1
        uuid: 9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68
        alterId: 1

  - name: tuic-in-1
    type: tuic
    port: 10815
    listen: 0.0.0.0
    # rule: sub-rule-name1 # 默认使用 rules，如果未找到 sub-rule 则直接使用 rules
    # proxy: proxy # 如果不为空则直接将该入站流量交由指定proxy处理(当proxy不为空时，这里的proxy名称必须合法，否则会出错)
    # token:    # tuicV4填写（不可同时填写users）
    #   - TOKEN
    # users:    # tuicV5填写（不可同时填写token）
    #   00000000-0000-0000-0000-000000000000: PASSWORD-0
    #   00000000-0000-0000-0000-000000000001: PASSWORD-1
    #  certificate: ./server.crt
    #  private-key: ./server.key
    #  congestion-controller: bbr
    #  max-idle-time: 15000
    #  authentication-timeout: 1000
    #  alpn:
    #    - h3
    #  max-udp-relay-packet-size: 1500


```

!!! note
    proxy 如果不为空，则将该入站流量交由指定[proxy](../proxies/index.md)处理

    rule 如果定义的 [子规则 (sub-rule)](../sub-rule.md)不存在 则直接使用 rules

## 入口配置

入口配置与 Listener 等价，传入流量将和 socks,mixed 等入口一样按照 mode 所指定的方式进行匹配处理

```{.yaml linenums="1"}
# shadowsocks,vmess 入口配置（传入流量将和socks,mixed等入口一样按照mode所指定的方式进行匹配处理）
ss-config: ss://2022-blake3-aes-256-gcm:vlmpIPSyHH6f4S8WVPdRIHIlzmB+GIRfoH3aNJ/t9Gg=@:23456
vmess-config: vmess://1:9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68@:12345

# tuic服务器入口（传入流量将和socks,mixed等入口一样按照mode所指定的方式进行匹配处理）
tuic-server:
 enable: true
 listen: 127.0.0.1:10443
 token:    # tuicV4填写（不可同时填写users）
   - TOKEN
 users:    # tuicV5填写（不可同时填写token）
   00000000-0000-0000-0000-000000000000: PASSWORD-0
   00000000-0000-0000-0000-000000000001: PASSWORD-1
 certificate: ./server.crt
 private-key: ./server.key
 congestion-controller: bbr
 max-idle-time: 15000
 authentication-timeout: 1000
 alpn:
   - h3
 max-udp-relay-packet-size: 1500
```
