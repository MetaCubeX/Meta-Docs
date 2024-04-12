# 传输层配置

```{.yaml linenums="1"}
proxies:
- name: "transport-opts-example"
  http-opts:
    method: "GET"
    path:
    - '/'
    - '/video'
    headers:
      Connection:
      - keep-alive
  h2-opts:
    host:
    - example.com
    path: /
  grpc-opts:
    grpc-service-name: example
  ws-opts:
    path: /path
    headers:
      Host: example.com
    max-early-data:
    early-data-header-name:
    v2ray-http-upgrade: false
    v2ray-http-upgrade-fast-open: false
```

!!! note
    Clash 的 H2 传输层未实现多路复用功能，如果需要使用多路复用，在 Mihomo 中更建议使用 gRPC 协议，或者 sing-mux

## http-opts

`http` 传输层设置，仅传输层为 `http` 时生效

### http-opts.method

请求方法

### http-opts.path

请求路径

### http-opts.headers

请求头

## h2-opts

`h2` 传输层设置，仅传输层为 `h2` 时生效

### h2-opts.host

主机域名列表，如果设置，客户端将随机选择，服务器将验证

### h2-opts.path

请求路径

## grpc-opts

`grpc` 传输层设置，仅传输层为 `grpc` 时生效

### grpc-opts.grpc-service-name

gRPC 服务名称

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
