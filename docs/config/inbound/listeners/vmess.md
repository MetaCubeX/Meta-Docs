# VMESS

```{.yaml linenums="1"}
listeners:
- name: vmess-in-1
  type: vmess
  port: 10814 # 支持使用ports格式，例如200,302 or 200,204,401-429,501-503
  listen: 0.0.0.0
  # routing-mark: 0 # 为监听socket设置routing-mark（仅支持linux）
  # rule: sub-rule-name1 # 默认使用 rules，如果未找到 sub-rule 则直接使用 rules
  # proxy: proxy # 如果不为空则直接将该入站流量交由指定 proxy 处理 (当 proxy 不为空时，这里的 proxy 名称必须合法，否则会出错)
  users:
    - username: 1
      uuid: 9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68
      alterId: 1
  # ws-path: "/" # 如果不为空则开启 websocket 传输层
  # grpc-service-name: "GunService" # 如果不为空则开启 grpc 传输层
  # 如果填写 mekya-config 并设置 enable: true 则启用 v2ray 兼容的 Mekya 入站监听（不可与 mkcp/ws/grpc 同时使用）
  # mekya-config:
  #   enable: true
  #   max-write-size: 10485760 # 单个响应写回的最大负载大小，单位字节
  #   max-write-duration-ms: 5000 # 单个响应写回的最大持续时间，单位毫秒
  #   max-simultaneous-write-connection: 128 # 同一会话允许同时等待写回的请求数
  #   packet-writing-buffer: 65536 # 写包缓冲区大小
  #   kcp:
  #     mtu: 1350 # 最大传输单元
  #     tti: 15 # 传输时间间隔，单位毫秒
  #     uplink-capacity: 40 # 上行容量，单位 MB/s
  #     downlink-capacity: 2000 # 下行容量，单位 MB/s
  #     congestion: false # 是否启用拥塞控制
  #     write-buffer: 67108864 # 写缓冲区大小，单位字节
  #     read-buffer: 67108864 # 读缓冲区大小，单位字节
  #     seed: "" # 启用 AES-GCM 认证时使用的种子，留空使用默认认证
  #     header: "" # 伪装包头，可选：none/srtp/utp/wechat-video/dtls/wireguard
  # 如果填写 mkcp-config 并设置 enable: true 则启用 v2ray 兼容的 mKCP 入站监听
  # mkcp-config:
  #   enable: true
  #   mtu: 1350 # 最大传输单元
  #   tti: 50 # 传输时间间隔，单位毫秒
  #   uplink-capacity: 5 # 上行容量，单位 MB/s
  #   downlink-capacity: 20 # 下行容量，单位 MB/s
  #   congestion: false # 是否启用拥塞控制
  #   write-buffer: 2097152 # 写缓冲区大小，单位字节
  #   read-buffer: 2097152 # 读缓冲区大小，单位字节
  #   seed: "" # 启用 AES-GCM 认证时使用的种子，留空使用默认认证
  #   header: "" # 伪装包头，可选：none/srtp/utp/wechat-video/dtls/wireguard
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
  # 如果填写tlsmirror-config则开启tlsmirror（注意不可与certificate和private-key同时填写）
  # tlsmirror-config:
  #   dest: test.com:443
  #   primary-key: MDEyMzQ1Njc4OWFiY2RlZjAxMjM0NTY3ODlhYmNkZWY= # 必填，32 字节主密钥的 base64 编码
  #   proxy: ""
  #   explicit-nonce-ciphersuites: [156, 157, 158, 159, 160, 161, 162, 163, 164, 165, 166, 167, 168, 169, 170, 171, 172, 173, 49195, 49196, 49197, 49198, 49199, 49200, 49201, 49202, 49290, 49291, 49293, 49316, 49317, 49318, 49319, 49320, 49321, 49322, 49323, 49324, 49325, 49326, 49327, 52392, 52393, 52394, 52395, 52396, 52397, 52398] # TLS 1.2 载体使用显式 nonce 的加密套件
  #   defer-instance-derived-write-time: # 首次写入前的延迟
  #     base-nanoseconds: 0 # 固定延迟，单位纳秒
  #     uniform-random-multiplier-nanoseconds: 0 # 额外随机延迟上限，单位纳秒
  #   transport-layer-padding: # 启用传输层填充
  #     enabled: false
  #   connection-enrolment: # 启用 v2ray 兼容的连接登记确认
  #     primary-ingress-outbound: "" # v2ray 兼容字段，建议和对端路由中的控制出站 tag 一致
  #   sequence-watermarking-enabled: false # 启用序列水印
  # mux-option:
  #   padding: true
  #   brutal:
  #     enabled: true
  #     up: 1000 # 默认 Mbps
  #     down: 1000
```
