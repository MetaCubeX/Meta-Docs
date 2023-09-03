---
hide:
  - navigation
  # - toc
---
# 完整配置示例

## Alpha 分支

开发版,新特性支持

[https://github.com/MetaCubeX/Clash.Meta/blob/Alpha/docs/config.yaml](https://github.com/MetaCubeX/Clash.Meta/blob/Alpha/docs/config.yaml)

## Meta 分支

稳定版,每月一更,部分文档内容不会及时发布在meta分支内

[https://github.com/MetaCubeX/Clash.Meta/blob/Meta/docs/config.yaml](https://github.com/MetaCubeX/Clash.Meta/blob/Meta/docs/config.yaml)

## 懒人配置

在 `proxy-providers` 补充你的订阅即可食用

<!-- prettier-ignore-start -->

```yaml
######### 锚点 start #######
# proxy 相关
pr: &pr {type: select, proxies: [默认,香港,台湾,日本,新加坡,美国,其它地区,全部节点,自动选择,DIRECT]}

#这里是订阅更新和延迟测试相关的
p: &p {type: http, interval: 3600, health-check: {enable: true, url: https://www.gstatic.com/generate_204, interval: 300}}

use: &use
  type: select
  use:
  - provider1
  - provider2
  
######### 锚点 end #######


# url 里填写自己的订阅,名称不能重复
proxy-providers:
  provider1:
    <<: *p
    url: ""

  provider2:
    <<: *p
    url: ""

mode: rule
ipv6: true
log-level: info
allow-lan: true
mixed-port: 7890
unified-delay: false
tcp-concurrent: true
external-controller: 127.0.0.1:9090

geodata-mode: true
geox-url:
  geoip: "https://fastly.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat"
  geosite: "https://fastly.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat"
  mmdb: "https://fastly.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb"

find-process-mode: strict
global-client-fingerprint: chrome

profile:
  store-selected: true
  store-fake-ip: true

sniffer:
  enable: true
  sniff:
    TLS:
      ports: [443, 8443]
    HTTP:
      ports: [80, 8080-8880]
      override-destination: true

tun:
  enable: true
  stack: system
  dns-hijack:
    - 'any:53'
  auto-route: true
  auto-detect-interface: true

dns:
  enable: true
  listen: :1053
  ipv6: true
  enhanced-mode: fake-ip
  fake-ip-range: 28.0.0.1/8
  fake-ip-filter:
    - '*'
    - '+.lan'
    - '+.local'
  default-nameserver:
    - 223.5.5.5
  nameserver:
    - 'tls://8.8.4.4#dns'
    - 'tls://1.0.0.1#dns'
  proxy-server-nameserver:
    - https://doh.pub/dns-query
  nameserver-policy:
    "geosite:cn,private":
      - https://doh.pub/dns-query
      - https://dns.alidns.com/dns-query

proxies:
  # - name: "WARP"
  #   type: wireguard
  #   server: engage.cloudflareclient.com
  #   port: 2408
  #   ip: "172.16.0.2/32"
  #   ipv6: "2606::1/128"        # 自行替换
  #   private-key: "private-key" # 自行替换
  #   public-key: "public-key"   # 自行替换
  #   udp: true
  #   reserved: "abba"           # 自行替换
  #   mtu: 1280
  #   dialer-proxy: "dns"
  #   remote-dns-resolve: true
  #   dns:
  #     - https://dns.cloudflare.com/dns-query

proxy-groups:

  - {name: 默认, type: select, proxies: [DIRECT, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点, 自动选择]}
  
  # - {name: dns, type: select, proxies: [DIRECT, WARP, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点, 自动选择]}  # 加入 WARP
  - {name: dns, type: select, proxies: [DIRECT, 自动选择, 默认, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点]}

  - {name: Google, <<: *pr}

  - {name: Telegram, <<: *pr}

  - {name: Twitter, <<: *pr}

  - {name: Pixiv, <<: *pr}

  - {name: ehentai, <<: *pr}

  - {name: 哔哩哔哩, <<: *pr}

  - {name: 哔哩东南亚, <<: *pr}

  - {name: 巴哈姆特, <<: *pr}

  - {name: YouTube, <<: *pr}

  - {name: NETFLIX, <<: *pr}

  - {name: Spotify, <<: *pr}

  - {name: Github, <<: *pr}

  - {name: 国内, type: select, proxies: [DIRECT, 默认, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点, 自动选择]}

  - {name: 其他, <<: *pr}

#分隔,下面是地区分组
  - {name: 香港, <<: *use,filter: "(?i)港|hk|hongkong|hong kong"}

  - {name: 台湾, <<: *use, filter: "(?i)台|tw|taiwan"}

  - {name: 日本, <<: *use, filter: "(?i)日本|jp|japan"}

  - {name: 美国, <<: *use, filter: "(?i)美|us|unitedstates|united states"}

  - {name: 新加坡, <<: *use, filter: "(?i)(新|sg|singapore)"}

  - {name: 其它地区, <<: *use, filter: "(?i)^(?!.*(?:🇭🇰|🇯🇵|🇺🇸|🇸🇬|🇨🇳|港|hk|hongkong|台|tw|taiwan|日|jp|japan|新|sg|singapore|美|us|unitedstates)).*"}

  - {name: 全部节点, <<: *use}

  - {name: 自动选择, <<: *use, tolerance: 2, type: url-test}

rules:
  # - AND,(AND,(DST-PORT,443),(NETWORK,UDP)),(GEOSITE,geolocation-!cn),REJECT # quic
  - GEOSITE,biliintl, 哔哩东南亚
  - GEOSITE,ehentai,ehentai
  - GEOSITE,github,Github
  - GEOSITE,twitter,Twitter
  - GEOSITE,youtube,YouTube
  - GEOSITE,google,Google
  - GEOSITE,telegram,Telegram
  - GEOSITE,netflix,NETFLIX
  - GEOSITE,bilibili,哔哩哔哩
  - GEOSITE,bahamut,巴哈姆特
  - GEOSITE,spotify,Spotify
  - GEOSITE,geolocation-!cn,其他

  - GEOIP,google,Google
  - GEOIP,netflix,NETFLIX
  - GEOIP,telegram,Telegram
  - GEOIP,twitter,Twitter
  - GEOSITE,pixiv,Pixiv
  - GEOSITE,CN,国内
  - GEOIP,CN,国内
  - MATCH,其他
```

<!-- prettier-ignore-end -->

---
