=== "GeoX"
    === "config"
        ```yaml
        ######### 锚点 start #######
        # 策略组相关
        pr: &pr {type: select, proxies: [默认,香港,台湾,日本,新加坡,美国,其它地区,全部节点,自动选择,DIRECT]}
        
        #这里是订阅更新和延迟测试相关的
        p: &p {type: http, interval: 3600, health-check: {enable: true, url: https://www.gstatic.com/generate_204, interval: 300}}
        
        ######### 锚点 end #######
        
        
        # url 里填写自己的订阅,名称不能重复
        proxy-providers:
          provider1:
            <<: *p
            url: ""
        
          provider2:
            <<: *p
            url: ""
        
        ipv6: true
        allow-lan: true
        mixed-port: 7890
        unified-delay: false
        tcp-concurrent: true
        external-controller: 127.0.0.1:9090
        external-ui: ui
        external-ui-url: "https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip"
        
        geodata-mode: true
        geox-url:
          geoip: "https://mirror.ghproxy.com/https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/geoip-lite.dat"
          geosite: "https://mirror.ghproxy.com/https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/geosite.dat"
          mmdb: "https://mirror.ghproxy.com/https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/country-lite.mmdb"
        
        find-process-mode: strict
        global-client-fingerprint: chrome
        
        profile:
          store-selected: true
          store-fake-ip: true
        
        sniffer:
          enable: true
          sniff:
            HTTP:
              ports: [80, 8080-8880]
              override-destination: true
            TLS:
              ports: [443, 8443]
            QUIC:
              ports: [443, 8443]
          skip-domain:
            - "Mijia Cloud"
        
        tun:
          enable: true
          stack: mixed
          dns-hijack:
            - "any:53"
          auto-route: true
          auto-detect-interface: true
        
        dns:
          enable: true
          listen: :1053
          ipv6: true
          enhanced-mode: fake-ip
          fake-ip-filter:
            - "*"
            - "+.lan"
            - "+.local"
          nameserver:
            - https://doh.pub/dns-query
            - https://dns.alidns.com/dns-query
          proxy-server-nameserver:
            - https://doh.pub/dns-query
          nameserver-policy:
            "geosite:private":
              - https://doh.pub/dns-query
              - https://dns.alidns.com/dns-query
            "geosite:geolocation-!cn":
              - "https://dns.cloudflare.com/dns-query#dns"
              - "https://dns.google/dns-query#dns"
        
        proxy-groups:
          - {name: 默认, type: select, proxies: [自动选择, DIRECT, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点]}
          - {name: dns, type: select, proxies: [自动选择, 默认, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点]}
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
          - {name: 香港, type: select , include-all-providers: true, filter: "(?i)港|hk|hongkong|hong kong"}
          - {name: 台湾, type: select , include-all-providers: true, filter: "(?i)台|tw|taiwan"}
          - {name: 日本, type: select , include-all-providers: true, filter: "(?i)日|jp|japan"}
          - {name: 美国, type: select , include-all-providers: true, filter: "(?i)美|us|unitedstates|united states"}
          - {name: 新加坡, type: select , include-all-providers: true, filter: "(?i)(新|sg|singapore)"}
          - {name: 其它地区, type: select , include-all-providers: true, filter: "(?i)^(?!.*(?:🇭🇰|🇯🇵|🇺🇸|🇸🇬|🇨🇳|港|hk|hongkong|台|tw|taiwan|日|jp|japan|新|sg|singapore|美|us|unitedstates)).*"}
          - {name: 全部节点, type: select , include-all-providers: true}
          - {name: 自动选择, type: url-test, include-all-providers: true, tolerance: 10}
        
        rules:
          - GEOIP,lan,DIRECT,no-resolve
          - GEOSITE,biliintl,哔哩东南亚
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
          - GEOSITE,pixiv,Pixiv
          - GEOSITE,geolocation-!cn,其他
        
          - GEOIP,google,Google
          - GEOIP,netflix,NETFLIX
          - GEOIP,telegram,Telegram
          - GEOIP,twitter,Twitter
          - GEOSITE,CN,国内
          - GEOIP,CN,国内
          - MATCH,其他
        ```
    === "link"
        ```text
        https://wiki.metacubex.one/example/geox
        ```

=== "RULE-SET(yaml)"
    === "config"
        ```yaml
        ######### 锚点 start #######
        # 策略组相关
        pr: &pr {type: select, proxies: [默认,香港,台湾,日本,新加坡,美国,其它地区,全部节点,自动选择,DIRECT]}
        
        #这里是订阅更新和延迟测试相关的
        p: &p {type: http, interval: 3600, health-check: {enable: true, url: https://www.gstatic.com/generate_204, interval: 300}}
        
        ######### 锚点 end #######
        
        
        # url 里填写自己的订阅,名称不能重复
        proxy-providers:
          provider1:
            <<: *p
            url: ""
        
          provider2:
            <<: *p
            url: ""
        
        ipv6: true
        allow-lan: true
        mixed-port: 7890
        unified-delay: false
        tcp-concurrent: true
        external-controller: 127.0.0.1:9090
        external-ui: ui
        external-ui-url: "https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip"
        
        find-process-mode: strict
        global-client-fingerprint: chrome
        
        profile:
          store-selected: true
          store-fake-ip: true
        
        sniffer:
          enable: true
          sniff:
            HTTP:
              ports: [80, 8080-8880]
              override-destination: true
            TLS:
              ports: [443, 8443]
            QUIC:
              ports: [443, 8443]
          skip-domain:
            - "Mijia Cloud"
        tun:
          enable: true
          stack: mixed
          dns-hijack:
            - "any:53"
          auto-route: true
          auto-detect-interface: true
        
        dns:
          enable: true
          listen: :1053
          ipv6: true
          enhanced-mode: fake-ip
          fake-ip-range: 28.0.0.1/8
          fake-ip-filter:
            - "*"
            - "+.lan"
            - "+.local"
          default-nameserver:
            - 223.5.5.5
          nameserver:
            - https://doh.pub/dns-query
            - https://dns.alidns.com/dns-query
          proxy-server-nameserver:
            - https://doh.pub/dns-query
          nameserver-policy:
            "rule-set:private":
              - https://doh.pub/dns-query
              - https://dns.alidns.com/dns-query
            "rule-set:geolocation-!cn":
              - "https://dns.cloudflare.com/dns-query#dns"
              - "https://dns.google/dns-query#dns"
        
        proxy-groups:
          - {name: 默认, type: select, proxies: [自动选择, DIRECT, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点]}
          - {name: dns, type: select, proxies: [自动选择, 默认, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点]}
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
          - {name: 香港, type: select , include-all-providers: true, filter: "(?i)港|hk|hongkong|hong kong"}
          - {name: 台湾, type: select , include-all-providers: true, filter: "(?i)台|tw|taiwan"}
          - {name: 日本, type: select , include-all-providers: true, filter: "(?i)日|jp|japan"}
          - {name: 美国, type: select , include-all-providers: true, filter: "(?i)美|us|unitedstates|united states"}
          - {name: 新加坡, type: select , include-all-providers: true, filter: "(?i)(新|sg|singapore)"}
          - {name: 其它地区, type: select , include-all-providers: true, filter: "(?i)^(?!.*(?:🇭🇰|🇯🇵|🇺🇸|🇸🇬|🇨🇳|港|hk|hongkong|台|tw|taiwan|日|jp|japan|新|sg|singapore|美|us|unitedstates)).*"}
          - {name: 全部节点, type: select , include-all-providers: true}
          - {name: 自动选择, type: url-test, include-all-providers: true, tolerance: 10}
        
        rules:
          - GEOIP,lan,DIRECT,no-resolve
          - RULE-SET,biliintl_domain,哔哩东南亚
          - RULE-SET,ehentai_domain,ehentai
          - RULE-SET,github_domain,Github
          - RULE-SET,twitter_domain,Twitter
          - RULE-SET,youtube_domain,YouTube
          - RULE-SET,google_domain,Google
          - RULE-SET,telegram_domain,Telegram
          - RULE-SET,netflix_domain,NETFLIX
          - RULE-SET,bilibili_domain,哔哩哔哩
          - RULE-SET,bahamut_domain,巴哈姆特
          - RULE-SET,spotify_domain,Spotify
          - RULE-SET,pixiv_domain,Pixiv
          - RULE-SET,geolocation-!cn,其他
        
          - RULE-SET,google_ip,Google
          - RULE-SET,netflix_ip,NETFLIX
          - RULE-SET,telegram_ip,Telegram
          - RULE-SET,twitter_ip,Twitter
          - RULE-SET,cn_domain,国内
          - RULE-SET,cn_ip,国内
          - MATCH,其他
        
        rule-anchor:
          ip: &ip {type: http, interval: 86400, behavior: ipcidr, format: yaml}
          domain: &domain {type: http, interval: 86400, behavior: domain, format: yaml}
        rule-providers:
          private:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/private.yaml"
          cn_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/cn.yaml"
          biliintl_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/biliintl.yaml"
          ehentai_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/ehentai.yaml"
          github_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/github.yaml"
          twitter_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/twitter.yaml"
          youtube_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/youtube.yaml"
          google_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/google.yaml"
          telegram_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/telegram.yaml"
          netflix_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/netflix.yaml"
          bilibili_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/bilibili.yaml"
          bahamut_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/bahamut.yaml"
          spotify_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/spotify.yaml"
          pixiv_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/pixiv.yaml"
          geolocation-!cn:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/geolocation-!cn.yaml"
        
          cn_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/cn.yaml"
          google_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/google.yaml"
          netflix_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/netflix.yaml"
          twitter_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/twitter.yaml"
          telegram_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/telegram.yaml"
        ```
    === "link"
        ```text
        https://wiki.metacubex.one/example/yaml
        ```

=== "RULE-SET(text)"
    === "config"
        ```yaml
        ######### 锚点 start #######
        # 策略组相关
        pr: &pr {type: select, proxies: [默认,香港,台湾,日本,新加坡,美国,其它地区,全部节点,自动选择,DIRECT]}
        
        #这里是订阅更新和延迟测试相关的
        p: &p {type: http, interval: 3600, health-check: {enable: true, url: https://www.gstatic.com/generate_204, interval: 300}}
        
        ######### 锚点 end #######
        
        
        # url 里填写自己的订阅,名称不能重复
        proxy-providers:
          provider1:
            <<: *p
            url: ""
        
          provider2:
            <<: *p
            url: ""
        
        ipv6: true
        allow-lan: true
        mixed-port: 7890
        unified-delay: false
        tcp-concurrent: true
        external-controller: 127.0.0.1:9090
        external-ui: ui
        external-ui-url: "https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip"
        
        find-process-mode: strict
        global-client-fingerprint: chrome
        
        profile:
          store-selected: true
          store-fake-ip: true
        
        sniffer:
          enable: true
          sniff:
            HTTP:
              ports: [80, 8080-8880]
              override-destination: true
            TLS:
              ports: [443, 8443]
            QUIC:
              ports: [443, 8443]
          skip-domain:
            - "Mijia Cloud"
        tun:
          enable: true
          stack: mixed
          dns-hijack:
            - "any:53"
          auto-route: true
          auto-detect-interface: true
        
        dns:
          enable: true
          listen: :1053
          ipv6: true
          enhanced-mode: fake-ip
          fake-ip-range: 28.0.0.1/8
          fake-ip-filter:
            - "*"
            - "+.lan"
            - "+.local"
          default-nameserver:
            - 223.5.5.5
          nameserver:
            - https://doh.pub/dns-query
            - https://dns.alidns.com/dns-query
          proxy-server-nameserver:
            - https://doh.pub/dns-query
          nameserver-policy:
            "rule-set:private":
              - https://doh.pub/dns-query
              - https://dns.alidns.com/dns-query
            "rule-set:geolocation-!cn":
              - "https://dns.cloudflare.com/dns-query#dns"
              - "https://dns.google/dns-query#dns"
        
        proxy-groups:
          - {name: 默认, type: select, proxies: [自动选择, DIRECT, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点]}
          - {name: dns, type: select, proxies: [自动选择, 默认, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点]}
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
          - {name: 香港, type: select , include-all-providers: true, filter: "(?i)港|hk|hongkong|hong kong"}
          - {name: 台湾, type: select , include-all-providers: true, filter: "(?i)台|tw|taiwan"}
          - {name: 日本, type: select , include-all-providers: true, filter: "(?i)日|jp|japan"}
          - {name: 美国, type: select , include-all-providers: true, filter: "(?i)美|us|unitedstates|united states"}
          - {name: 新加坡, type: select , include-all-providers: true, filter: "(?i)(新|sg|singapore)"}
          - {name: 其它地区, type: select , include-all-providers: true, filter: "(?i)^(?!.*(?:🇭🇰|🇯🇵|🇺🇸|🇸🇬|🇨🇳|港|hk|hongkong|台|tw|taiwan|日|jp|japan|新|sg|singapore|美|us|unitedstates)).*"}
          - {name: 全部节点, type: select , include-all-providers: true}
          - {name: 自动选择, type: url-test, include-all-providers: true, tolerance: 10}
        
        rules:
          - GEOIP,lan,DIRECT,no-resolve
          - RULE-SET,biliintl_domain,哔哩东南亚
          - RULE-SET,ehentai_domain,ehentai
          - RULE-SET,github_domain,Github
          - RULE-SET,twitter_domain,Twitter
          - RULE-SET,youtube_domain,YouTube
          - RULE-SET,google_domain,Google
          - RULE-SET,telegram_domain,Telegram
          - RULE-SET,netflix_domain,NETFLIX
          - RULE-SET,bilibili_domain,哔哩哔哩
          - RULE-SET,bahamut_domain,巴哈姆特
          - RULE-SET,spotify_domain,Spotify
          - RULE-SET,pixiv_domain,Pixiv
          - RULE-SET,geolocation-!cn,其他
        
          - RULE-SET,google_ip,Google
          - RULE-SET,netflix_ip,NETFLIX
          - RULE-SET,telegram_ip,Telegram
          - RULE-SET,twitter_ip,Twitter
          - RULE-SET,cn_domain,国内
          - RULE-SET,cn_ip,国内
          - MATCH,其他
        
        rule-anchor:
          ip: &ip {type: http, interval: 86400, behavior: ipcidr, format: text}
          domain: &domain {type: http, interval: 86400, behavior: domain, format: text}
        rule-providers:
          private:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/private.list"
          cn_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/cn.list"
          biliintl_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/biliintl.list"
          ehentai_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/ehentai.list"
          github_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/github.list"
          twitter_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/twitter.list"
          youtube_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/youtube.list"
          google_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/google.list"
          telegram_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/telegram.list"
          netflix_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/netflix.list"
          bilibili_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/bilibili.list"
          bahamut_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/bahamut.list"
          spotify_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/spotify.list"
          pixiv_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/pixiv.list"
          geolocation-!cn:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/geolocation-!cn.list"
        
          cn_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/cn.list"
          google_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/google.list"
          netflix_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/netflix.list"
          twitter_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/twitter.list"
          telegram_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/telegram.list"
        ```
    === "link"
        ```text
        https://wiki.metacubex.one/example/text
        ```

=== "Stash(私货)"
    === "config"
        ```yaml
        ######### 锚点 start #######
        # 策略组相关
        pr: &pr {type: select, proxies: [默认, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点, 自动选择, DIRECT]}
        #这里是订阅更新和延迟测试相关的
        p: &p {type: http, interval: 3600, health-check: {enable: true, url: "https://www.gstatic.com/generate_204", interval: 300}}
        ######### 锚点 end #######
        
        # url 里填写自己的订阅,名称不能重复
        proxy-providers:
          provider1:
            <<: *p
            url: ""
          provider2:
            <<: *p
            url: ""
        ipv6: true
        allow-lan: true
        mixed-port: 7890
        external-controller: 127.0.0.1:9090
        profile:
          store-selected: true
          store-fake-ip: true
        dns:
          enable: true
          ipv6: true
          enhanced-mode: fake-ip
          fake-ip-range: 28.0.0.1/8
          fake-ip-filter:
            - "*"
            - "+.lan"
            - "+.local"
          default-nameserver:
            - 223.5.5.5
          nameserver:
            - https://doh.pub/dns-query
            - https://dns.alidns.com/dns-query
        
        proxy-groups:
        - {name: 默认, type: select, proxies: [自动选择, DIRECT, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点], icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Proxy.png"}
        - {name: Google, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Google_Search.png"}
        - {name: Apple, <<: *pr, icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Apple.png}
        - {name: Telegram, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Telegram.png"}
        - {name: Twitter, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Twitter.png"}
        - {name: ehentai, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Panda.png"}
        - {name: 哔哩哔哩, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/bilibili.png"}
        - {name: 哔哩东南亚, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/bilibili.png"}
        - {name: 巴哈姆特, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Bahamut.png"}
        - {name: YouTube, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/YouTube.png"}
        - {name: NETFLIX, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Netflix.png"}
        - {name: Spotify, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Spotify.png"}
        - {name: 国内, type: select, proxies: [DIRECT, 默认, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点, 自动选择], icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/China_Map.png"}
        - {name: 其他, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Final.png"}
          #分隔,下面是地区分组
        - {name: 香港, type: select, proxies: [香港自动选择], include-all: true, filter: "(?i)港|hk|hongkong|hong kong", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/HK.png"}
        - {name: 台湾, type: select, proxies: [台湾自动选择], include-all: true, filter: "(?i)台|tw|taiwan", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/TW.png"}
        - {name: 日本, type: select, proxies: [日本自动选择], include-all: true, filter: "(?i)日|jp|japan", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/JP.png"}
        - {name: 美国, type: select, proxies: [美国自动选择], include-all: true, filter: "(?i)美|us|unitedstates|united states", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/US.png"}
        - {name: 新加坡, type: select, proxies: [新加坡自动选择], include-all: true, filter: "(?i)(新|sg|singapore)", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/SG.png"}
        - {name: 其它地区, type: select, include-all: true, filter: "(?i)^(?!.*(?:🇭🇰|🇯🇵|🇺🇸|🇸🇬|🇨🇳|港|hk|hongkong|台|tw|taiwan|日|jp|japan|新|sg|singapore|美|us|unitedstates)).*", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Available.png"}
        - {name: 自动选择, type: url-test, include-all: true, tolerance: 10, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Auto.png"}
        - {name: 全部节点, type: select, include-all: true, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Global.png"}
        
        - {name: 香港自动选择, type: select, include-all: true, filter: "(?i)港|hk|hongkong|hong kong", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/HK.png"}
        - {name: 台湾自动选择, type: select, include-all: true, filter: "(?i)台|tw|taiwan", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/TW.png"}
        - {name: 日本自动选择, type: select, include-all: true, filter: "(?i)日|jp|japan", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/JP.png"}
        - {name: 美国自动选择, type: select, include-all: true, filter: "(?i)美|us|unitedstates|united states", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/US.png"}
        - {name: 新加坡自动选择, type: select, include-all: true, filter: "(?i)(新|sg|singapore)", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/SG.png"}
        
        rules:
        - GEOIP,lan,DIRECT,no-resolve
        - RULE-SET,biliintl_domain,哔哩东南亚
        - RULE-SET,ehentai_domain,ehentai
        - RULE-SET,github_domain,Github
        - RULE-SET,twitter_domain,Twitter
        - RULE-SET,youtube_domain,YouTube
        - RULE-SET,google_domain,Google
        - RULE-SET,telegram_domain,Telegram
        - RULE-SET,netflix_domain,NETFLIX
        - RULE-SET,bilibili_domain,哔哩哔哩
        - RULE-SET,bahamut_domain,巴哈姆特
        - RULE-SET,spotify_domain,Spotify
        - RULE-SET,pixiv_domain,Pixiv
        - RULE-SET,geolocation-!cn,其他
        
        - RULE-SET,google_ip,Google
        - RULE-SET,netflix_ip,NETFLIX
        - RULE-SET,telegram_ip,Telegram
        - RULE-SET,twitter_ip,Twitter
        - RULE-SET,cn_domain,国内
        - RULE-SET,cn_ip,国内
        - MATCH,其他
        
        rule-anchor:
          ip: &ip {type: http, interval: 86400, behavior: ipcidr, format: text}
          domain: &domain {type: http, interval: 86400, behavior: domain, format: text}
        rule-providers:
          private:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/private.list"
          cn_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/cn.list"
          biliintl_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/biliintl.list"
          ehentai_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/ehentai.list"
          github_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/github.list"
          twitter_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/twitter.list"
          youtube_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/youtube.list"
          google_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/google.list"
          telegram_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/telegram.list"
          netflix_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/netflix.list"
          bilibili_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/bilibili.list"
          bahamut_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/bahamut.list"
          spotify_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/spotify.list"
          pixiv_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/pixiv.list"
          geolocation-!cn:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/geolocation-!cn.list"
        
          cn_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/cn.list"
          google_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/google.list"
          netflix_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/netflix.list"
          twitter_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/twitter.list"
          telegram_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/telegram.list"
        ```
    === "link"
        ```text
        https://wiki.metacubex.one/example/stash
        ```