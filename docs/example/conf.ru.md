=== "GeoX"
    === "config"
        ```{.yaml linenums="1"}
        # В url укажите свою подписку, имена не должны повторяться
        proxy-providers:
          provider1:
            url: ""
            type: http
            interval: 86400
            health-check: {enable: true,url: "https://www.gstatic.com/generate_204", interval: 300}
            override:
              additional-prefix: "[provider1]"

          provider2:
            url: ""
            type: http
            interval: 86400
            health-check: {enable: true,url: "https://www.gstatic.com/generate_204",interval: 300}
            override:
              additional-prefix: "[provider2]"

        proxies:
          - name: "Прямое соединение"
            type: direct
            udp: true

        mixed-port: 7890
        ipv6: true
        allow-lan: true
        unified-delay: false
        tcp-concurrent: true
        external-controller: 127.0.0.1:9090
        external-ui: ui
        external-ui-url: "https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip"

        geodata-mode: true
        geox-url:
          geoip: "https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/geoip-lite.dat"
          geosite: "https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/geosite.dat"
          mmdb: "https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/country-lite.mmdb"
          asn: "https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/GeoLite2-ASN.mmdb"
        find-process-mode: strict

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
            - "+.push.apple.com"

        tun:
          enable: true
          stack: mixed
          dns-hijack:
            - "any:53"
            - "tcp://any:53"
          auto-route: true
          auto-redirect: true
          auto-detect-interface: true

        dns:
          enable: true
          ipv6: true
          enhanced-mode: fake-ip
          fake-ip-filter:
            - "*"
            - "+.lan"
            - "+.local"
            - "+.market.xiaomi.com"
          default-nameserver:
            - tls://223.5.5.5
            - tls://223.6.6.6
          nameserver:
            - https://doh.pub/dns-query
            - https://dns.alidns.com/dns-query

        proxy-groups:

          - name: По умолчанию
            type: select
            proxies: [Автовыбор,Прямое соединение,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы]

          - name: Google
            type: select
            proxies: [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: Telegram
            type: select
            proxies: [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: Twitter
            type: select
            proxies: [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: Билибили
            type: select
            proxies: [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: Bahamut
            type: select
            proxies: [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: YouTube
            type: select
            proxies: [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: NETFLIX
            type: select
            proxies: [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: Spotify
            type: select
            proxies:  [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: Github
            type: select
            proxies:  [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: Китай
            type: select
            proxies:  [Прямое соединение,По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор]

          - name: Прочее
            type: select
            proxies:  [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          #Разделитель, ниже региональные группы
          - name: Гонконг
            type: select
            include-all: true
            exclude-type: direct
            filter: "(?i)港|hk|hongkong|hong kong"

          - name: Тайвань
            type: select
            include-all: true
            exclude-type: direct
            filter: "(?i)台|tw|taiwan"

          - name: Япония
            type: select
            include-all: true
            exclude-type: direct
            filter: "(?i)日|jp|japan"

          - name: США
            type: select
            include-all: true
            exclude-type: direct
            filter: "(?i)美|us|unitedstates|united states"

          - name: Сингапур
            type: select
            include-all: true
            exclude-type: direct
            filter: "(?i)(新|sg|singapore)"

          - name: Другие регионы
            type: select
            include-all: true
            exclude-type: direct
            filter: "(?i)^(?!.*(?:🇭🇰|🇯🇵|🇺🇸|🇸🇬|🇨🇳|港|hk|hongkong|台|tw|taiwan|日|jp|japan|新|sg|singapore|美|us|unitedstates)).*"

          - name: Все узлы
            type: select
            include-all: true
            exclude-type: direct

          - name: Автовыбор
            type: url-test
            include-all: true
            exclude-type: direct
            tolerance: 10

        rules:
          - GEOIP,lan,Прямое соединение,no-resolve
          - GEOSITE,github,Github
          - GEOSITE,twitter,Twitter
          - GEOSITE,youtube,YouTube
          - GEOSITE,google,Google
          - GEOSITE,telegram,Telegram
          - GEOSITE,netflix,NETFLIX
          - GEOSITE,bilibili,Билибили
          - GEOSITE,bahamut,Bahamut
          - GEOSITE,spotify,Spotify
          - GEOSITE,CN,Китай
          - GEOSITE,geolocation-!cn,Прочее

          - GEOIP,google,Google
          - GEOIP,netflix,NETFLIX
          - GEOIP,telegram,Telegram
          - GEOIP,twitter,Twitter
          - GEOIP,CN,Китай
          - MATCH,Прочее
        ```
    === "link"
        ```text
        https://wiki.metacubex.one/example/geox
        ```

=== "RULE-SET"
    === "config"
        ```{.yaml linenums="1"}
        # В url укажите свою подписку, имена не должны повторяться
        proxy-providers:
          provider1:
            url: ""
            type: http
            interval: 86400
            health-check: {enable: true,url: "https://www.gstatic.com/generate_204",interval: 300}
            override:
              additional-prefix: "[provider1]"

          provider2:
            url: ""
            type: http
            interval: 86400
            health-check: {enable: true,url: "https://www.gstatic.com/generate_204",interval: 300}
            override:
              additional-prefix: "[provider2]"

        proxies:
          - name: "Прямое соединение"
            type: direct
            udp: true

        mixed-port: 7890
        ipv6: true
        allow-lan: true
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
            - "+.push.apple.com"

        tun:
          enable: true
          stack: mixed
          dns-hijack:
            - "any:53"
            - "tcp://any:53"
          auto-route: true
          auto-redirect: true
          auto-detect-interface: true

        dns:
          enable: true
          ipv6: true
          enhanced-mode: fake-ip
          fake-ip-filter:
            - "*"
            - "+.lan"
            - "+.local"
            - "+.market.xiaomi.com"
          default-nameserver:
            - tls://223.5.5.5
            - tls://223.6.6.6
          nameserver:
            - https://doh.pub/dns-query
            - https://dns.alidns.com/dns-query

        proxy-groups:

          - name: По умолчанию
            type: select
            proxies: [Автовыбор,Прямое соединение,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы]

          - name: Google
            type: select
            proxies: [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: Telegram
            type: select
            proxies: [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: Twitter
            type: select
            proxies: [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: Билибили
            type: select
            proxies: [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: Bahamut
            type: select
            proxies: [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: YouTube
            type: select
            proxies: [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: NETFLIX
            type: select
            proxies: [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: Spotify
            type: select
            proxies:  [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: Github
            type: select
            proxies:  [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          - name: Китай
            type: select
            proxies:  [Прямое соединение,По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор]

          - name: Прочее
            type: select
            proxies:  [По умолчанию,Гонконг,Тайвань,Япония,Сингапур,США,Другие регионы,Все узлы,Автовыбор,Прямое соединение]

          #Разделитель, ниже региональные группы
          - name: Гонконг
            type: select
            include-all: true
            exclude-type: direct
            filter: "(?i)港|hk|hongkong|hong kong"

          - name: Тайвань
            type: select
            include-all: true
            exclude-type: direct
            filter: "(?i)台|tw|taiwan"

          - name: Япония
            type: select
            include-all: true
            exclude-type: direct
            filter: "(?i)日|jp|japan"

          - name: США
            type: select
            include-all: true
            exclude-type: direct
            filter: "(?i)美|us|unitedstates|united states"

          - name: Сингапур
            type: select
            include-all: true
            exclude-type: direct
            filter: "(?i)(新|sg|singapore)"

          - name: Другие регионы
            type: select
            include-all: true
            exclude-type: direct
            filter: "(?i)^(?!.*(?:🇭🇰|🇯🇵|🇺🇸|🇸🇬|🇨🇳|港|hk|hongkong|台|tw|taiwan|日|jp|japan|新|sg|singapore|美|us|unitedstates)).*"

          - name: Все узлы
            type: select
            include-all: true
            exclude-type: direct

          - name: Автовыбор
            type: url-test
            include-all: true
            exclude-type: direct
            tolerance: 10

        rules:
          - RULE-SET,private_ip,Прямое соединение,no-resolve
          - RULE-SET,github_domain,Github
          - RULE-SET,twitter_domain,Twitter
          - RULE-SET,youtube_domain,YouTube
          - RULE-SET,google_domain,Google
          - RULE-SET,telegram_domain,Telegram
          - RULE-SET,netflix_domain,NETFLIX
          - RULE-SET,bilibili_domain,Билибили
          - RULE-SET,bahamut_domain,Bahamut
          - RULE-SET,spotify_domain,Spotify
          - RULE-SET,cn_domain,Китай
          - RULE-SET,geolocation-!cn,Прочее

          - RULE-SET,google_ip,Google
          - RULE-SET,netflix_ip,NETFLIX
          - RULE-SET,telegram_ip,Telegram
          - RULE-SET,twitter_ip,Twitter
          - RULE-SET,cn_ip,Китай
          - MATCH,Прочее

        rule-anchor:
          ip: &ip {type: http, interval: 86400, behavior: ipcidr, format: mrs}
          domain: &domain {type: http, interval: 86400, behavior: domain, format: mrs}
        rule-providers:
          private_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/private.mrs"
          cn_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/cn.mrs"
          biliintl_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/biliintl.mrs"
          ehentai_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/ehentai.mrs"
          github_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/github.mrs"
          twitter_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/twitter.mrs"
          youtube_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/youtube.mrs"
          google_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/google.mrs"
          telegram_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/telegram.mrs"
          netflix_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/netflix.mrs"
          bilibili_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/bilibili.mrs"
          bahamut_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/bahamut.mrs"
          spotify_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/spotify.mrs"
          pixiv_domain:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/pixiv.mrs"
          geolocation-!cn:
            <<: *domain
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/geolocation-!cn.mrs"

          private_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/private.mrs"
          cn_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/cn.mrs"
          google_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/google.mrs"
          netflix_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/netflix.mrs"
          twitter_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/twitter.mrs"
          telegram_ip:
            <<: *ip
            url: "https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geoip/telegram.mrs"
        ```
    === "link"
        ```text
        https://wiki.metacubex.one/example/mrs
        ```

=== "Stash(личные предпочтения)"
    === "config"
        ```{.yaml linenums="1"}
        ######### Якоря start #######
        # Связанные с группами прокси
        pr: &pr {type: select, proxies: [По умолчанию, Гонконг, Гонконг автовыбор, Тайвань, Тайвань автовыбор, Япония, Япония автовыбор, Сингапур, Сингапур автовыбор, США, США автовыбор, Другие регионы, Все узлы, Автовыбор, Прямое соединение]}
        #Здесь настройки обновления подписки и тестирования задержки
        p: &p {type: http, interval: 3600, health-check: {enable: true, url: "https://www.gstatic.com/generate_204", interval: 300}}
        ######### Якоря end #######

        # В url укажите свою подписку, имена не должны повторяться
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

        proxies:
        - name: "Прямое соединение"
          type: direct
          udp: true
        proxy-groups:
        - {name: По умолчанию, type: select, proxies: [Автовыбор, Прямое соединение, Гонконг, Гонконг автовыбор, Тайвань, Тайвань автовыбор, Япония, Япония автовыбор, Сингапур, Сингапур автовыбор, США, США автовыбор, Другие регионы, Все узлы], icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Proxy.png"}
        - {name: Google, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Google_Search.png"}
        - {name: Apple, <<: *pr, icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Apple.png}
        - {name: Telegram, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Telegram.png"}
        - {name: Twitter, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Twitter.png"}
        - {name: ehentai, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Panda.png"}
        - {name: Билибили, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/bilibili.png"}
        - {name: Билибили Юго-Восточная Азия, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/bilibili.png"}
        - {name: Bahamut, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Bahamut.png"}
        - {name: YouTube, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/YouTube.png"}
        - {name: NETFLIX, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Netflix.png"}
        - {name: Spotify, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Spotify.png"}
        - {name: Китай, type: select, proxies: [Прямое соединение, По умолчанию, Гонконг, Гонконг автовыбор, Тайвань, Тайвань автовыбор, Япония, Япония автовыбор, Сингапур, Сингапур автовыбор, США, США автовыбор, Другие регионы, Все узлы, Автовыбор], icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/China_Map.png"}
        - {name: Прочее, <<: *pr, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Final.png"}
          #Разделитель, ниже региональные группы
        - {name: Гонконг, type: select, include-all: true, filter: "(?i)(?!Прямое соединение)(港|hk|hongkong|hong kong)", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/HK.png"}
        - {name: Тайвань, type: select, include-all: true, filter: "(?i)(?!Прямое соединение)(台|tw|taiwan)", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/TW.png"}
        - {name: Япония, type: select, include-all: true, filter: "(?i)(?!Прямое соединение)(日|jp|japan)", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/JP.png"}
        - {name: США, type: select, include-all: true, filter: "(?i)(?!Прямое соединение)(美|us|unitedstates|united states)", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/US.png"}
        - {name: Сингапур, type: select, include-all: true, filter: "(?i)(?!Прямое соединение)(新|sg|singapore)", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/SG.png"}
        - {name: Другие регионы, type: select, include-all: true, filter: "(?i)^(?!.*(?:🇭🇰|🇯🇵|🇺🇸|🇸🇬|🇨🇳|港|hk|hongkong|台|tw|taiwan|日|jp|japan|新|sg|singapore|美|us|unitedstates|Прямое соединение)).*", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Available.png"}
        - {name: Автовыбор, type: url-test, include-all: true, tolerance: 10, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Auto.png"}
        - {name: Все узлы, type: select, include-all: true, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/Global.png"}

        - {name: Гонконг автовыбор,type: url-test, include-all: true, filter: "(?i)(?!Прямое соединение)(港|hk|hongkong|hong kong)", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/HK.png"}
        - {name: Тайвань автовыбор,type: url-test, include-all: true, filter: "(?i)(?!Прямое соединение)(台|tw|taiwan)", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/TW.png"}
        - {name: Япония автовыбор,type: url-test, include-all: true, filter: "(?i)(?!Прямое соединение)(日|jp|japan)", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/JP.png"}
        - {name: США автовыбор,type: url-test, include-all: true, filter: "(?i)(?!Прямое соединение)(美|us|unitedstates|united states)", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/US.png"}
        - {name: Сингапур автовыбор,type: url-test, include-all: true, filter: "(?i)(新|sg|singapore)", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/mini/SG.png"}

        rules:
        - GEOIP,lan,Прямое соединение,no-resolve
        - RULE-SET,biliintl_domain,Билибили Юго-Восточная Азия
        - RULE-SET,ehentai_domain,ehentai
        - RULE-SET,github_domain,Прочее
        - RULE-SET,twitter_domain,Twitter
        - RULE-SET,youtube_domain,YouTube
        - RULE-SET,google_domain,Google
        - RULE-SET,telegram_domain,Telegram
        - RULE-SET,netflix_domain,NETFLIX
        - RULE-SET,bilibili_domain,Билибили
        - RULE-SET,bahamut_domain,Bahamut
        - RULE-SET,spotify_domain,Spotify
        - RULE-SET,pixiv_domain,Прочее
        - RULE-SET,geolocation-!cn,Прочее

        - RULE-SET,google_ip,Google
        - RULE-SET,netflix_ip,NETFLIX
        - RULE-SET,telegram_ip,Telegram
        - RULE-SET,twitter_ip,Twitter
        - RULE-SET,cn_domain,Китай
        - RULE-SET,cn_ip,Китай
        - MATCH,Прочее

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
