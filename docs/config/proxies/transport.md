# 传输层配置

=== "http"
    ```{.yaml linenums="1"}
    proxies:
    - name: "http-opts-example"
      type: xxxxx
      network: http
      http-opts:
        method: "GET"
        path:
        - '/'
        - '/video'
        headers:
          Connection:
          - keep-alive
    ```

=== "h2"
    ```{.yaml linenums="1"}
    proxies:
    - name: "h2-opts-example"
      type: xxxxx
      network: h2
      h2-opts:
        host:
        - example.com
        path: /
    ```

=== "grpc"
    ```{.yaml linenums="1"}
    proxies:
    - name: "grpc-opts-example"
      type: xxxxx
      network: grpc
      grpc-opts:
        grpc-service-name: example
        # grpc-user-agent:
        # ping-interval: 0
        # max-connections: 1
        # min-streams: 0
        # max-streams: 0
    ```
=== "ws"
    ```{.yaml linenums="1"}
    proxies:
    - name: "ws-opts-example"
      type: xxxxx
      network: ws
      ws-opts:
        path: /path
        headers:
          Host: example.com
        max-early-data:
        early-data-header-name:
        v2ray-http-upgrade: false
        v2ray-http-upgrade-fast-open: false
    ```
=== "mkcp"
    ```{.yaml linenums="1"}
    proxies:
    - name: "mkcp-opts-example"
      type: vmess
      server: server
      port: 443
      uuid: uuid
      alterId: 32
      cipher: auto
      network: mkcp
      mkcp-opts:
        mtu: 1350
        tti: 50
        uplink-capacity: 5
        downlink-capacity: 20
        congestion: false
        write-buffer: 2097152
        read-buffer: 2097152
        seed: ""
        header: ""
    ```
=== "mekya"
    ```{.yaml linenums="1"}
    proxies:
    - name: "mekya-opts-example"
      type: vmess
      server: server
      port: 443
      uuid: uuid
      alterId: 32
      cipher: auto
      tls: true
      network: mekya
      mekya-opts:
        url: https://server:443/mekya
        max-write-delay: 80
        max-request-size: 96000
        polling-interval-initial: 200
        h2-pool-size: 8
        kcp:
          mtu: 1350
          tti: 15
          uplink-capacity: 40
          downlink-capacity: 2000
          congestion: false
          write-buffer: 67108864
          read-buffer: 67108864
          seed: ""
          header: ""
    ```
=== "xhttp"
    ```{.yaml linenums="1"}
    proxies:
    - name: "xhttp-opts-example"
      type: vless
      server: server
      port: 443
      uuid: uuid
      udp: true
      tls: true
      network: xhttp
      alpn: [h2] # By default, only h2 mode is supported. To enable h3 mode, you need to set alpn: [h3]; to enable HTTP/1.1 mode, you need to set alpn: [http/1.1].
      # ech-opts: ...
      # reality-opts: ...
      # skip-cert-verify: false
      # fingerprint: ...
      # certificate: ...
      # private-key: ...
      servername: xxx.com
      client-fingerprint: chrome
      encryption: ""
      xhttp-opts:
        path: "/"
        host: xxx.com
        # mode: "stream-one" # Available: "stream-one", "stream-up" or "packet-up"
        # headers:
        #   X-Forwarded-For: ""
        # no-grpc-header: false
        # x-padding-bytes: "100-1000"
        # x-padding-obfs-mode: false
        # x-padding-key: x_padding
        # x-padding-header: Referer
        # x-padding-placement: queryInHeader # Available: queryInHeader, cookie, header, query
        # x-padding-method: repeat-x # Available: repeat-x, tokenish
        # uplink-http-method: POST # Available: POST, PUT, PATCH, DELETE
        # session-placement: path # Available: path, query, cookie, header
        # session-key: ""
        # session-table: "" # Available: "", "uuid", "ALPHABET", "Alphabet", "BASE36", "Base62", "HEX", "alphabet", "base36", "hex", "number"
        # session-length: "16-32"
        # seq-placement: path # Available: path, query, cookie, header
        # seq-key: ""
        # uplink-data-placement: body # Available: body, cookie, header
        # uplink-data-key: ""
        # uplink-chunk-size: 0 # only applicable when uplink-data-placement is not body
        # sc-max-each-post-bytes: 1000000
        # sc-min-posts-interval-ms: 30
        # reuse-settings: # aka XMUX
        #   max-concurrency: "16-32"
        #   max-connections: "0"
        #   c-max-reuse-times: "0"
        #   h-max-request-times: "600-900"
        #   h-max-reusable-secs: "1800-3000"
        #   h-keep-alive-period: 0
        # download-settings:
        #   ## xhttp part
        #   path: "/"
        #   host: xxx.com
        #   headers:
        #     X-Forwarded-For: ""
        #   reuse-settings: # aka XMUX
        #     max-concurrency: "16-32"
        #     max-connections: "0"
        #     c-max-reuse-times: "0"
        #     h-max-request-times: "600-900"
        #     h-max-reusable-secs: "1800-3000"
        #     h-keep-alive-period: 0
        #   ## proxy part
        #   server: server
        #   port: 443
        #   tls: true
        #   alpn: ...
        #   ech-opts: ...
        #   reality-opts: ...
        #   skip-cert-verify: false
        #   fingerprint: ...
        #   certificate: ...
        #   private-key: ...
        #   servername: xxx.com
        #   client-fingerprint: chrome
    ```

## http-opts

`http` 传输层设置，仅传输层为 `http` 时生效

### http-opts.method

http 请求方法

### http-opts.path

http 请求路径

### http-opts.headers

http 请求头

!!! note
    Mihomo 的 H2 传输层未实现多路复用功能，如果需要使用多路复用，在 Mihomo 中更建议使用 gRPC 协议，或者 sing-mux

## h2-opts

`h2` 传输层设置，仅传输层为 `h2` 时生效

### h2-opts.host

主机域名列表，如果设置，客户端将随机选择，服务器将验证

### h2-opts.path

http 请求路径

## grpc-opts

`grpc` 传输层设置，仅传输层为 `grpc` 时生效

### grpc-opts.grpc-service-name

gRPC 服务名称

### grpc-opts.grpc-user-agent

gRPC UserAgent

### grpc-opts.ping-interval

gRPC 心跳包间隔，默认关闭，单位为秒

### grpc-opts.max-connections

最大连接数量。默认值为 1 即只使用一条底层链接

与 `max-streams` 冲突

### grpc-opts.min-streams

在打开新连接之前，连接中的最小多路复用流数量

与 `max-streams` 冲突

### grpc-opts.max-streams

在打开新连接之前，连接中的最大多路复用流数量

与 `max-connections` 和 `min-streams` 冲突

## ws-opts

`ws` 传输层设置，仅传输层为 `ws` 时生效

### ws-opts.path

请求路径

### ws-opts.headers

请求头

### ws-opts.max-early-data

Early Data 首包长度阈值

### ws-opts.early-data-header-name

### ws-opts.v2ray-http-upgrade

使用 http upgrade

### ws-opts.v2ray-http-upgrade-fast-open

启用 http upgrade 的 fast open

## mkcp-opts

`mkcp` 传输层设置，仅传输层为 `mkcp` 时生效

!!! note
    仅 VMess 支持 mKCP 传输层，请勿在其他协议上使用

### mkcp-opts.mtu

最大传输单元

### mkcp-opts.tti

传输时间间隔，单位毫秒

### mkcp-opts.uplink-capacity

上行容量，单位 MB/s

### mkcp-opts.downlink-capacity

下行容量，单位 MB/s

### mkcp-opts.congestion

是否启用拥塞控制

### mkcp-opts.write-buffer

写缓冲区大小，单位字节

### mkcp-opts.read-buffer

读缓冲区大小，单位字节

### mkcp-opts.seed

启用 AES-GCM 认证时使用的种子，留空使用默认认证

### mkcp-opts.header

伪装包头，可选 `none`/`srtp`/`utp`/`wechat-video`/`dtls`/`wireguard`

## mekya-opts

`mekya` 传输层设置，仅传输层为 `mekya` 时生效

!!! note
    仅 VMess 支持 Mekya 传输层，请勿在其他协议上使用

### mekya-opts.url

Mekya 服务端 URL

### mekya-opts.max-write-delay

首包后的最大聚合等待时间，单位毫秒

### mekya-opts.max-request-size

单次 HTTP 请求的最大负载大小，单位字节

### mekya-opts.polling-interval-initial

空轮询间隔，单位毫秒

### mekya-opts.h2-pool-size

HTTP/2 连接池大小

### mekya-opts.kcp

Mekya 内部 KCP 参数，字段含义同 [mkcp-opts](#mkcp-opts)

## xhttp-opts

`xhttp` 传输层设置，仅传输层为 `xhttp` 时生效

默认仅支持 h2，如果开启 h3 模式需要设置`alpn: [h3]`，如果开启 http1.1 模式需要设置`alpn: [http/1.1]`

!!! note
    仅 VLESS 支持 xhttp 传输层，请勿在其他协议上使用


### xhttp-opts.path

请求路径

### xhttp-opts.host

主机名

### xhttp-opts.mode

模式，可选值：`auto`, `stream-one`, `stream-up` or `packet-up`

### xhttp-opts.headers

请求头

### xhttp-opts.no-grpc-header

设置 stream-up/one 上行是否发送`Content-Type: application/grpc`头伪装成 gRPC

### xhttp-opts.x-padding-bytes

客户端发的 request header 均默认包含 `Referer: ...?x_padding=XXX...` ，默认长度为 100-1000，每次请求随机，服务端默认会检查它是否在服务端允许的范围内

### xhttp-opts.x-padding-obfs-mode

启用填充混淆。为向后兼容，默认为 false

### xhttp-opts.x-padding-key

用于存储填充值的键名。其含义取决于 `x-padding-placement`：

* URL 中查询参数的名称（如果 placement 为 `queryInHeader`）
* cookie 的名称
* HTTP 标头的名称
* URL 查询参数的名称

### xhttp-opts.x-padding-header

HTTP 标头的名称。仅当 `x-padding-placement` 为 `header` 或 `queryInHeader` 时才相关

### xhttp-opts.x-padding-placement

定义填充值的放置位置。可选值：`queryInHeader`、`cookie`、`header`、`query`。仅当 `x-padding-obfs-mode` 为 true 时才有效

### xhttp-opts.x-padding-method

定义填充值的生成方式

* `repeat-x`：默认方法（一个包含 X 个字符的长序列）
* `tokenish`：生成一个随机的 Base62 标记

### xhttp-opts.uplink-http-method

更改用于上行数据传输的 HTTP 方法。使用任何支持请求体（例如 `PUT`、`PATCH`）且 CDN 允许的方法

### xhttp-opts.session-placement

会话 ID 的放置位置。选项：`path`, `query`, `cookie`, `header`

### xhttp-opts.session-key

会话 ID 的键名（如果放置位置为`path`，则不适用）

### xhttp-opts.session-table

生成会话 ID 时使用的字符表。可选值：`""`、`uuid`、`ALPHABET`、`Alphabet`、`BASE36`、`Base62`、`HEX`、`alphabet`、`base36`、`hex`、`number`

### xhttp-opts.session-length

会话 ID 长度范围。起始值不可为 `0`，总的 id 空间必须大于 21 亿，仅当 `session-table` 不为空或为 `uuid` 时生效

### xhttp-opts.seq-placement

序列号的放置位置。选项：`path`、`query`、`cookie`、`header`。如果`session-placement`设置为`path`，则`seq-placement`也必须设置为`path`

### xhttp-opts.seq-key

序列号的键名（如果放置位置为`path`，则不适用）

### xhttp-opts.uplink-data-placement

将拆分后的上行链路数据片段放置位置（仅适用于`packet-up`模式）。选项：`cookie` 或 `header`

### xhttp-opts.uplink-data-key

用于传递数据片段的键的基本名称。客户端会自动将数据分割成多个数据块，服务器会将它们重新组装起来

### xhttp-opts.uplink-chunk-size

每个上行链路数据块的最大大小（以字节为单位）（仅当 `uplink-data-placement` 不是 `body` 时适用）

最小值：64 字节

推荐值：

* 对于`cookie`放置：3072 字节（默认 3KB）
* 对于`header`放置：4096 字节（默认 4KB）

如果未指定，则会根据 `uplink-data-placement` 自动选择合适的默认值

### xhttp-opts.sc-max-each-post-bytes

客户端每个 POST 最多携带多少数据，默认值 1000000 即 1MB，该值应小于 CDN 等 HTTP 中间盒所允许的最大值，服务端也会拒绝大于该值的 POST

### xhttp-opts.sc-min-posts-interval-ms

基于单个代理请求，客户端发起 POST 请求的最小间隔，默认值 30 毫秒

### xhttp-opts.reuse-settings

链接复用设置（即 XMUX）

注意：和原版实现不同，此项没有默认值，如果不填写则不开启链接复用，即每次打开一个新的底层链接

### xhttp-opts.reuse-settings.max-concurrency

每条底层连接中最多同时存在多少代理请求，连接中的代理请求数量达到该值后会建立新的连接，以容纳更多的代理请求

### xhttp-opts.reuse-settings.max-connections

最多同时存在多少条连接，连接数达到该值前每个新的代理请求都会开启一条新的连接，此后会开始复用已有的连接

该值与 max-concurrency 冲突，只能二选一

### xhttp-opts.reuse-settings.c-max-reuse-times

一条连接最多被复用几次，复用该次数后将不会被分配新的代理请求，将在内部最后一条代理请求关闭后断开

### xhttp-opts.reuse-settings.h-max-request-times

一条连接最多累计承载多少次，该项计数不严谨，且 Golang 的 GET 请求有自动重试，故不建议填写

### xhttp-opts.reuse-settings.h-max-reusable-secs

TCP/QUIC 连接持续该时间后将不会被分配新的 HTTP 请求，将在内部最后一个 HTTP 请求关闭后断开

### xhttp-opts.reuse-settings.h-keep-alive-period

H2 / H3 连接空闲时客户端每隔多少秒发一次保活包，默认 0，即 Chrome H2 的 45 秒，或 quic-go H3 的 10 秒。它是 XMUX 里唯一不允许填范围（该项取随机才是特征）且允许填负数（比如填 -1 关掉空闲保活包）的项，建议留 0。

### xhttp-opts.download-settings

上下行分离设置

注意：此项用于覆盖原始配置，每一项如果不填写则会沿用上行参数（仅支持覆盖实例中列出的选项，未列出的不会被覆写）

