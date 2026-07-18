# Sudoku

```{.yaml linenums="1"}
listeners:
- name: sudoku-in-1
  type: sudoku
  port: 8443 # 仅支持单端口
  listen: 0.0.0.0
  # routing-mark: 0 # 为监听socket设置routing-mark（仅支持linux）
  key: "<server_key>" # 如果你使用sudoku生成的ED25519密钥对，此处是密钥对中的公钥，当然，你也可以仅仅使用任意uuid充当key
  aead-method: chacha20-poly1305 # 可选：chacha20-poly1305、aes-128-gcm、none（不建议；none 不提供 AEAD 保护）
  padding-min: 1 # 最小填充率（0-100）
  padding-max: 15 # 最大填充率（0-100，必须 >= padding-min）
  table-type: prefer_ascii # 可选值：prefer_ascii、prefer_entropy、up_ascii_down_entropy、up_entropy_down_ascii
  # custom-table: xpxvvpvv # 可选，自定义字节布局，必须包含2个x、2个p、4个v，可随意组合；只对 entropy 方向生效
  # custom-tables: ["xpxvvpvv", "vxpvxvvp"] # 可选，自定义字节布局列表（x/v/p），用于多表轮换；非空时覆盖 custom-table
  handshake-timeout: 5 # 可选（秒）
  enable-pure-downlink: false # 可选：false=带宽优化下行；true=纯 Sudoku 下行
  httpmask:
    disable: false # true 禁用所有 HTTP 伪装/隧道
    mode: legacy # 可选：legacy（默认）、stream（split-stream）、poll、auto（先 stream 再 poll）、ws（WebSocket 隧道）
    # path-root: "" # 可选：HTTP 隧道端点一级路径前缀（双方需一致），例如 "aabbcc" 或 "/aabbcc/" => /aabbcc/session、/aabbcc/stream、/aabbcc/api/v1/upload、/aabbcc/ws
  #
  # 可选：当启用 HTTPMask 且识别到“像 HTTP 但不符合 tunnel/auth”的请求时，将原始字节透传给 fallback（常用于与其他服务共端口）：
  # fallback: "127.0.0.1:80"
  # 以下为 httpmask 对象的兼容写法：
  # disable-http-mask: false
  # http-mask-mode: legacy
  # path-root: ""
  # mux-option:
  #   padding: true
  #   brutal:
  #     enabled: true
  #     up: 1000 # 默认 Mbps
  #     down: 1000

```

[通用字段](./index.md)
