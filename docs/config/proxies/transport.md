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
      alpn:
        - h2
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
        # sc-max-each-post-bytes: 1000000
        # reuse-settings: # aka XMUX
        #   max-connections: "16-32"
        #   max-concurrency: "0"
        #   c-max-reuse-times: "0"
        #   h-max-request-times: "600-900"
        #   h-max-reusable-secs: "1800-3000"
        # download-settings:
        #   ## xhttp part
        #   path: "/"
        #   host: xxx.com
        #   headers:
        #     X-Forwarded-For: ""
        #   no-grpc-header: false
        #   x-padding-bytes: "100-1000"
        #   sc-max-each-post-bytes: 1000000
        #   reuse-settings: # aka XMUX
        #     max-connections: "16-32"
        #     max-concurrency: "0"
        #     c-max-reuse-times: "0"
        #     h-max-request-times: "600-900"
        #     h-max-reusable-secs: "1800-3000"
        #   ## proxy part
        #   server: server
        #   port: 443
        #   tls: true
        #   alpn:
        #     - h2
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

最大连接数量。默认值为1即只使用一条底层链接

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

## xhttp-opts

`xhttp` 传输层设置，仅传输层为 `xhttp` 时生效

!!! note
    仅VLESS支持xhttp传输层，请勿在其他协议上使用

### xhttp-opts.path

请求路径

### xhttp-opts.host

主机名

### xhttp-opts.mode

模式，可选值：`auto`, `stream-one`, `stream-up` or `packet-up`

### xhttp-opts.headers

请求头

### xhttp-opts.no-grpc-header

设置stream-up/one上行是否发送`Content-Type: application/grpc`头伪装成 gRPC

### xhttp-opts.x-padding-bytes

客户端发的 request header 均默认包含 `Referer: ...?x_padding=XXX...` ，默认长度为 100-1000，每次请求随机，服务端默认会检查它是否在服务端允许的范围内

### xhttp-opts.sc-max-each-post-bytes

客户端每个 POST 最多携带多少数据，默认值 1000000 即 1MB，该值应小于 CDN 等 HTTP 中间盒所允许的最大值，服务端也会拒绝大于该值的 POST

注意：和原版实现不同，此项只允许填写单个数字，不允许填写范围

### xhttp-opts.reuse-settings

链接复用设置（即XMUX）

注意：和原版实现不同，此项没有默认值，如果不填写则不开启链接复用，即每次打开一个新的底层链接

### xhttp-opts.reuse-settings.max-connections

最多同时存在多少条连接，连接数达到该值前每个新的代理请求都会开启一条新的连接，此后会开始复用已有的连接

该值与 max-concurrency 冲突，只能二选一

### xhttp-opts.reuse-settings.max-concurrency

每条底层连接中最多同时存在多少代理请求，连接中的代理请求数量达到该值后会建立新的连接，以容纳更多的代理请求

### xhttp-opts.reuse-settings.c-max-reuse-times

一条连接最多被复用几次，复用该次数后将不会被分配新的代理请求，将在内部最后一条代理请求关闭后断开

### xhttp-opts.reuse-settings.h-max-request-times

一条连接最多累计承载多少次，该项计数不严谨，且 Golang 的 GET 请求有自动重试，故不建议填写

### xhttp-opts.reuse-settings.h-max-reusable-secs

TCP/QUIC 连接持续该时间后将不会被分配新的 HTTP 请求，将在内部最后一个 HTTP 请求关闭后断开

### xhttp-opts.download-settings

上下行分离设置

注意：此项用于覆盖原始配置，每一项如果不填写则会沿用上行参数（仅支持覆盖实例中列出的选项，未列出的不会被覆写）

