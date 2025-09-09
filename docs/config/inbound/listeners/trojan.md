# Trojan

```{.yaml linenums="1"}
listeners:
- name: trojan-in-1
  type: trojan
  port: 10819 # 支持使用ports格式，例如200,302 or 200,204,401-429,501-503
  listen: 0.0.0.0
  # rule: sub-rule-name1 # 默认使用 rules，如果未找到 sub-rule 则直接使用 rules
  # proxy: proxy # 如果不为空则直接将该入站流量交由指定 proxy 处理 (当 proxy 不为空时，这里的 proxy 名称必须合法，否则会出错)
  users:
    - username: 1
      password: 9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68
  # ws-path: "/" # 如果不为空则开启 websocket 传输层
  # grpc-service-name: "GunService" # 如果不为空则开启 grpc 传输层
  # 下面两项如果填写则开启 tls（需要同时填写）
  certificate: ./server.crt
  private-key: ./server.key
  # 如果填写则开启ech（可由 mihomo generate ech-keypair <明文域名> 生成）
  # ech-key: |
  #   -----BEGIN ECH KEYS-----
  #   ACATwY30o/RKgD6hgeQxwrSiApLaCgU+HKh7B6SUrAHaDwBD/g0APwAAIAAgHjzK
  #   madSJjYQIf9o1N5GXjkW4DEEeb17qMxHdwMdNnwADAABAAEAAQACAAEAAwAIdGVz
  #   dC5jb20AAA==
  #   -----END ECH KEYS-----
  # 如果填写reality-config则开启reality（注意不可与certificate和private-key同时填写）
  # reality-config:
  #   dest: test.com:443
  #   private-key: jNXHt1yRo0vDuchQlIP6Z0ZvjT3KtzVI-T4E7RoLJS0 # 可由 mihomo generate reality-keypair 命令生成
  #   short-id:
  #     - 0123456789abcdef
  #   server-names:
  #     - test.com
  #   #下列两个 limit 为选填，可对未通过验证的回落连接限速，bytesPerSec 默认为 0 即不启用
  #   #回落限速是一种特征，不建议启用，如果您是面板/一键脚本开发者，务必让这些参数随机化
  #   limit-fallback-upload:
  #     after-bytes: 0 # 传输指定字节后开始限速
  #     bytes-per-sec: 0 # 基准速率（字节/秒）
  #     burst-bytes-per-sec: 0 # 突发速率（字节/秒），大于 bytesPerSec 时生效
  #   limit-fallback-download:
  #     after-bytes: 0 # 传输指定字节后开始限速
  #     bytes-per-sec: 0 # 基准速率（字节/秒）
  #     burst-bytes-per-sec: 0 # 突发速率（字节/秒），大于 bytesPerSec 时生效
  # ss-option: # like trojan-go's `shadowsocks` config
  #   enabled: false
  #   method: aes-128-gcm # aes-128-gcm/aes-256-gcm/chacha20-ietf-poly1305
  #   password: "example"
  ### 注意，对于trojan listener, 至少需要填写 “certificate和private-key” 或 “reality-config” 或 “ss-option” 的其中一项 ###
```
