# Transport Configuration

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

`http` transport settings. Only effective when the transport layer is `http`.

### http-opts.method

HTTP request method.

### http-opts.path

HTTP request path.

### http-opts.headers

HTTP request headers.

!!! note
    Mihomo's H2 transport layer does not implement multiplexing. If multiplexing is needed, gRPC or sing-mux is recommended in Mihomo.

## h2-opts

`h2` transport settings. Only effective when the transport layer is `h2`.

### h2-opts.host

List of hostnames. If set, the client randomly selects one and the server validates it.

### h2-opts.path

HTTP request path.

## grpc-opts

`grpc` transport settings. Only effective when the transport layer is `grpc`.

### grpc-opts.grpc-service-name

gRPC service name.

### grpc-opts.grpc-user-agent

gRPC User-Agent.

### grpc-opts.ping-interval

gRPC heartbeat interval. Disabled by default, in seconds.

### grpc-opts.max-connections

Maximum number of connections. The default value is 1, meaning only one underlying connection is used.

Conflicts with `max-streams`.

### grpc-opts.min-streams

Minimum number of multiplexed streams in a connection before opening a new connection.

Conflicts with `max-streams`.

### grpc-opts.max-streams

Maximum number of multiplexed streams in a connection before opening a new connection.

Conflicts with `max-connections` and `min-streams`.

## ws-opts

`ws` transport settings. Only effective when the transport layer is `ws`.

### ws-opts.path

Request path.

### ws-opts.headers

Request headers.

### ws-opts.max-early-data

Early Data first-packet length threshold.

### ws-opts.early-data-header-name

Early Data header name.

### ws-opts.v2ray-http-upgrade

Use HTTP upgrade.

### ws-opts.v2ray-http-upgrade-fast-open

Enable fast open for HTTP upgrade.

## mkcp-opts

`mkcp` transport settings. Only effective when the transport layer is `mkcp`.

!!! note
    Only VMess supports the mKCP transport layer. Do not use it with other protocols.

### mkcp-opts.mtu

Maximum transmission unit.

### mkcp-opts.tti

Transmission time interval in milliseconds.

### mkcp-opts.uplink-capacity

Uplink capacity in MB/s.

### mkcp-opts.downlink-capacity

Downlink capacity in MB/s.

### mkcp-opts.congestion

Whether to enable congestion control.

### mkcp-opts.write-buffer

Write buffer size in bytes.

### mkcp-opts.read-buffer

Read buffer size in bytes.

### mkcp-opts.seed

Seed used for AES-GCM authentication. Leave empty to use the default authentication.

### mkcp-opts.header

Packet header camouflage. Available values: `none`/`srtp`/`utp`/`wechat-video`/`dtls`/`wireguard`.

## mekya-opts

`mekya` transport settings. Only effective when the transport layer is `mekya`.

!!! note
    Only VMess supports the Mekya transport layer. Do not use it with other protocols.

### mekya-opts.url

Mekya server URL.

### mekya-opts.max-write-delay

Maximum aggregation wait time after the first packet, in milliseconds.

### mekya-opts.max-request-size

Maximum payload size of a single HTTP request, in bytes.

### mekya-opts.polling-interval-initial

Empty polling interval in milliseconds.

### mekya-opts.h2-pool-size

HTTP/2 connection pool size.

### mekya-opts.kcp

Internal KCP parameters for Mekya. The fields are the same as [mkcp-opts](#mkcp-opts).

## xhttp-opts

`xhttp` transport settings. Only effective when the transport layer is `xhttp`.

By default, only h2 is supported. To enable h3 mode, set `alpn: [h3]`. To enable HTTP/1.1 mode, set `alpn: [http/1.1]`.

!!! note
    Only VLESS supports the xhttp transport layer. Do not use it with other protocols.

### xhttp-opts.path

Request path.

### xhttp-opts.host

Hostname.

### xhttp-opts.mode

Mode. Available values: `auto`, `stream-one`, `stream-up`, or `packet-up`.

### xhttp-opts.headers

Request headers.

### xhttp-opts.no-grpc-header

Sets whether stream-up/one uplink sends a `Content-Type: application/grpc` header to mimic gRPC.

### xhttp-opts.x-padding-bytes

The request headers sent by the client include `Referer: ...?x_padding=XXX...` by default. The default length is 100-1000 and is randomized for each request. The server checks whether it falls within the allowed range.

### xhttp-opts.x-padding-obfs-mode

Enables padding obfuscation. Defaults to false for backward compatibility.

### xhttp-opts.x-padding-key

Key name used to store the padding value. Its meaning depends on `x-padding-placement`:

* Query parameter name in the URL when placement is `queryInHeader`.
* Cookie name.
* HTTP header name.
* URL query parameter name.

### xhttp-opts.x-padding-header

HTTP header name. Only relevant when `x-padding-placement` is `header` or `queryInHeader`.

### xhttp-opts.x-padding-placement

Defines where the padding value is placed. Available values: `queryInHeader`, `cookie`, `header`, `query`. Only valid when `x-padding-obfs-mode` is true.

### xhttp-opts.x-padding-method

Defines how the padding value is generated.

* `repeat-x`: default method, a long sequence containing X characters.
* `tokenish`: generates a random Base62 token.

### xhttp-opts.uplink-http-method

Changes the HTTP method used for uplink data transfer. Use any method that supports a request body, such as `PUT` or `PATCH`, and is allowed by the CDN.

### xhttp-opts.session-placement

Placement of the session ID. Options: `path`, `query`, `cookie`, `header`.

### xhttp-opts.session-key

Key name for the session ID. Not applicable when placement is `path`.

### xhttp-opts.session-table

Character table used to generate session IDs. Available values: `""`, `uuid`, `ALPHABET`, `Alphabet`, `BASE36`, `Base62`, `HEX`, `alphabet`, `base36`, `hex`, `number`.

### xhttp-opts.session-length

Session ID length range. The start value cannot be `0`; the total ID space must be larger than 2.1 billion. Effective only when `session-table` is non-empty or `uuid`.

### xhttp-opts.seq-placement

Placement of the sequence number. Options: `path`, `query`, `cookie`, `header`. If `session-placement` is set to `path`, `seq-placement` must also be set to `path`.

### xhttp-opts.seq-key

Key name for the sequence number. Not applicable when placement is `path`.

### xhttp-opts.uplink-data-placement

Placement of split uplink data fragments, only for `packet-up` mode. Options: `cookie` or `header`.

### xhttp-opts.uplink-data-key

Base name of the key used to pass data fragments. The client automatically splits data into multiple chunks and the server reassembles them.

### xhttp-opts.uplink-chunk-size

Maximum size of each uplink data chunk, in bytes. Only applicable when `uplink-data-placement` is not `body`.

Minimum value: 64 bytes.

Recommended values:

* For `cookie` placement: 3072 bytes (default 3KB).
* For `header` placement: 4096 bytes (default 4KB).

If unspecified, an appropriate default is selected automatically based on `uplink-data-placement`.

### xhttp-opts.sc-max-each-post-bytes

Maximum amount of data carried by each client POST. Default: 1000000, or 1MB. This value should be smaller than the maximum allowed by CDNs and other HTTP middleboxes. The server also rejects POSTs larger than this value.

### xhttp-opts.sc-min-posts-interval-ms

Minimum interval for the client to initiate POST requests based on a single proxy request. Default: 30 milliseconds.

### xhttp-opts.reuse-settings

Connection reuse settings, also known as XMUX.

Note: unlike the original implementation, this option has no default value. If omitted, connection reuse is disabled and each request opens a new underlying connection.

### xhttp-opts.reuse-settings.max-concurrency

Maximum number of proxy requests that can exist simultaneously in each underlying connection. When the number of proxy requests in the connection reaches this value, a new connection is established to accommodate more requests.

### xhttp-opts.reuse-settings.max-connections

Maximum number of simultaneous connections. Before this value is reached, each new proxy request opens a new connection. After that, existing connections start to be reused.

This conflicts with `max-concurrency`; only one can be used.

### xhttp-opts.reuse-settings.c-max-reuse-times

Maximum number of times a connection can be reused. After this count is reached, it will not be assigned new proxy requests and will be closed after the last internal proxy request finishes.

### xhttp-opts.reuse-settings.h-max-request-times

Maximum cumulative number of requests carried by a connection. This counter is not strict, and Golang GET requests have automatic retries, so filling this is not recommended.

### xhttp-opts.reuse-settings.h-max-reusable-secs

After a TCP/QUIC connection lasts this long, it will no longer be assigned new HTTP requests and will be closed after the last internal HTTP request finishes.

### xhttp-opts.reuse-settings.h-keep-alive-period

How often the client sends keepalive packets when the H2/H3 connection is idle. Default: 0, meaning Chrome H2's 45 seconds or quic-go H3's 10 seconds. This is the only XMUX option that does not allow ranges, because randomizing it is itself a fingerprint, and it allows negative values such as -1 to disable idle keepalive packets. Leaving it at 0 is recommended.

### xhttp-opts.download-settings

Separate upload/download settings.

Note: this option overrides the original configuration. Any item left unset inherits the uplink parameter. Only the options listed in the example can be overridden; unlisted options are not overwritten.
