# Sudoku

```{.yaml linenums="1"}
proxies:
  - name: sudoku
    type: sudoku
    server: 1.2.3.4
    port: 443 
    key: "<client_key>"
    aead-method: chacha20-poly1305
    padding-min: 2
    padding-max: 7
    table-type: prefer_ascii
    # custom-table: xpxvvpvv
    # custom-tables: ["xpxvvpvv", "vxpvxvvp"]
    # multiplex: "off"
    httpmask:
      disable: false
      mode: legacy
      tls: true
      host: ""
      path-root: ""
      multiplex: "off"
    enable-pure-downlink: false
```

[通用字段](./index.md)

## key

如果你使用 sudoku 生成的 ED25519 密钥对，请填写密钥对中的私钥，否则填入和服务端相同的 uuid

## aead-method

可选值：`chacha20-poly1305`、`aes-128-gcm`、`none`。不建议使用 `none`，因为它不提供 AEAD 保护。

## padding-min

最小填充率，范围为 0-100。

## padding-max

最大填充率，范围为 0-100，且必须大于或等于 `padding-min`。

## table-type

可选值：prefer_ascii、prefer_entropy、up_ascii_down_entropy、up_entropy_down_ascii

## custom-table

可选，自定义字节布局，必须包含 2 个 x、2 个 p、4 个 v，可随意组合；只对 entropy 方向生效

## custom-tables

可选，自定义字节布局列表（x/v/p），非空时覆盖 custom-table

## multiplex

可选：`off`（默认）、`auto`（仅复用 HTTPMask 底层连接）、`on`（在原始 TCP 或 HTTPMask 上启用 Sudoku 单会话多目标 mux）

## httpmask.disable

是否禁用所有 HTTP 伪装/隧道

## httpmask.mode

可选：legacy（默认）、stream、poll、auto、ws；stream/poll/auto/ws 支持走 CDN/反代

## httpmask.tls

可选：仅在 mode 为 stream/poll/auto/ws 时生效；true 强制 https；false 强制 http（不会根据端口自动推断）

## httpmask.host

可选：覆盖 Host/SNI（支持 example.com 或 example.com:443）；仅在 mode 为 stream/poll/auto/ws 时生效

## httpmask.path-root

可选：HTTP 隧道端点一级路径前缀（双方需一致），例如 "aabbcc" => /aabbcc/session、/aabbcc/stream、/aabbcc/api/v1/upload、/aabbcc/ws

## httpmask.multiplex

兼容旧配置；设置时优先于顶层 `multiplex`

## enable-pure-downlink

选择下行模式：`false` 为带宽优化下行，`true` 为纯 Sudoku 下行。该配置需要与服务端保持一致。
