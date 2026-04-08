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
        # max-connections: 1 # Maximum connections. Conflict with max-streams.
        # min-streams: 0 # Minimum multiplexed streams in a connection before opening a new connection. Conflict with max-streams.
        # max-streams: 0 # Maximum multiplexed streams in a connection before opening a new connection. Conflict with max-connections and min-streams.
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
