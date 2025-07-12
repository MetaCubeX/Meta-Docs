# VLESS

```{.yaml linenums="1"}
listeners:
- name: vless-in-1
  type: vless
  port: 10817 # 支持使用ports格式，例如200,302 or 200,204,401-429,501-503
  listen: 0.0.0.0
  # rule: sub-rule-name1 # 默认使用 rules，如果未找到 sub-rule 则直接使用 rules
  # proxy: proxy # 如果不为空则直接将该入站流量交由指定 proxy 处理 (当 proxy 不为空时，这里的 proxy 名称必须合法，否则会出错)
  users:
    - username: 1
      uuid: 9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68
      flow: xtls-rprx-vision
  # ws-path: "/" # 如果不为空则开启 websocket 传输层
  # grpc-service-name: "GunService" # 如果不为空则开启 grpc 传输层
  # 下面两项如果填写则开启 tls（需要同时填写）
  # certificate: ./server.crt
  # private-key: ./server.key
  # 如果填写reality-config则开启reality（注意不可与certificate和private-key同时填写）
  fingerprint: xxxx
  reality-config:
    dest: test.com:443
    private-key: jNXHt1yRo0vDuchQlIP6Z0ZvjT3KtzVI-T4E7RoLJS0 # 可由 mihomo generate reality-keypair 命令生成
    short-id:
      - 0123456789abcdef
    server-names:
      - test.com
  ### 注意，对于vless listener, 至少需要填写 “certificate和private-key” 或 “reality-config” 的其中一项 ###
```

## SSL证书
与 `reality-config` 互斥

*certificate*

> 公钥路径 `string`

*private-key*

> 私钥路径 `string`


## fingerprint
设置传输指纹，用于标识自身设备 `chrome/firefox/safari/edge/android/ios/other`

## reality-config

与 `ssl证书` 互斥， `reality` 配置项

*dest*
> 必要,设置 reality 的模拟目标地址，必须带端口号

*private-key*
> 必要, reality 的私钥，可使用 `mihomo generate reality-keypair` 生成密钥对

*short-id*
> 必要, 可使用 `openssl rand -hex 8` 生成短ID

*server-names*
> 必要，客户端的 server names 列表