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
  # -------------------------
  # vless encryption服务端配置：
  # （原生外观 / 只 XOR 公钥 / 全随机数。1-RTT 每次下发随机 300 到 600 秒的 ticket 以便 0-RTT 复用 / 只允许 1-RTT）
  # 填写 "600s" 会每次随机取 50% 到 100%，即相当于填写 "300-600s"
  # / 是只能选一个，后面 base64 至少一个，无限串联，使用 mihomo generate vless-x25519 和 mihomo generate vless-mlkem768 生成，替换值时需去掉括号
  #
  # Padding 是可选的参数，仅作用于 1-RTT 以消除握手的长度特征，双端默认值均为 "100-111-1111.75-0-111.50-0-3333"：
  # 在 1-RTT client/server hello 后以 100% 的概率粘上随机 111 到 1111 字节的 padding
  # 以 75% 的概率等待随机 0 到 111 毫秒（"probability-from-to"）
  # 再次以 50% 的概率发送随机 0 到 3333 字节的 padding（若为 0 则不 Write()）
  # 服务端、客户端可以设置不同的 padding 参数，按 len、gap 的顺序无限串联，第一个 padding 需概率 100%、至少 35 字节
  # -------------------------
  # decryption: "mlkem768x25519plus.native/xorpub/random.600s(300-600s)/0s.(padding len).(padding gap).(X25519 PrivateKey).(ML-KEM-768 Seed)..."
  # 下面两项如果填写则开启 tls（需要同时填写）
  # certificate: ./server.crt # 证书 PEM 格式，或者 证书的路径
  # private-key: ./server.key # 证书对应的私钥 PEM 格式，或者私钥路径
  # 下面两项为mTLS配置项，如果client-auth-type设置为 "verify-if-given" 或 "require-and-verify" 则client-auth-cert必须不为空
  # client-auth-type: "" # 可选值：""、"request"、"require-any"、"verify-if-given"、"require-and-verify"
  # client-auth-cert: string # 证书 PEM 格式，或者 证书的路径
  # 如果填写则开启ech（可由 mihomo generate ech-keypair <明文域名> 生成）
  # ech-key: |
  #   -----BEGIN ECH KEYS-----
  #   ACATwY30o/RKgD6hgeQxwrSiApLaCgU+HKh7B6SUrAHaDwBD/g0APwAAIAAgHjzK
  #   madSJjYQIf9o1N5GXjkW4DEEeb17qMxHdwMdNnwADAABAAEAAQACAAEAAwAIdGVz
  #   dC5jb20AAA==
  #   -----END ECH KEYS-----
  # 如果填写reality-config则开启reality（注意不可与certificate和private-key同时填写）
  reality-config:
    dest: test.com:443
    private-key: jNXHt1yRo0vDuchQlIP6Z0ZvjT3KtzVI-T4E7RoLJS0 # 可由 mihomo generate reality-keypair 命令生成
    short-id:
      - 0123456789abcdef
    server-names:
      - test.com
    #下列两个 limit 为选填，可对未通过验证的回落连接限速，bytesPerSec 默认为 0 即不启用
    #回落限速是一种特征，不建议启用，如果您是面板/一键脚本开发者，务必让这些参数随机化
    limit-fallback-upload:
      after-bytes: 0 # 传输指定字节后开始限速
      bytes-per-sec: 0 # 基准速率（字节/秒）
      burst-bytes-per-sec: 0 # 突发速率（字节/秒），大于 bytesPerSec 时生效
    limit-fallback-download:
      after-bytes: 0 # 传输指定字节后开始限速
      bytes-per-sec: 0 # 基准速率（字节/秒）
      burst-bytes-per-sec: 0 # 突发速率（字节/秒），大于 bytesPerSec 时生效
  ### 注意，对于vless listener, 至少需要填写 “certificate和private-key” 或 “reality-config” 或 “decryption” 的其中一项 ###
```
