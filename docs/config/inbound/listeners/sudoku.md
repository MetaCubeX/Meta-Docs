# Sudoku

```{.yaml linenums="1"}
listeners:
- name: sudoku-in-1
  type: sudoku
  port: 8443 # 仅支持单端口
  listen: 0.0.0.0
  key: "<server_key>" # 如果你使用sudoku生成的ED25519密钥对，此处是密钥对中的公钥，当然，你也可以仅仅使用任意uuid充当key
  aead-method: chacha20-poly1305 # 支持chacha20-poly1305或者aes-128-gcm以及none，sudoku的混淆层可以确保none情况下数据安全
  padding-min: 1 # 填充最小长度
  padding-max: 15 # 填充最大长度，均不建议过大
  table-type: prefer_ascii # 可选值：prefer_ascii、prefer_entropy 前者全ascii映射，后者保证熵值（汉明1）低于3
  # custom-table: xpxvvpvv # 可选，自定义字节布局，必须包含2个x、2个p、4个v，可随意组合。启用此处则需配置`table-type`为`prefer_entropy`
  # custom-tables: ["xpxvvpvv", "vxpvxvvp"] # 可选，自定义字节布局列表（x/v/p），用于 xvp 模式轮换；非空时覆盖 custom-table
  handshake-timeout: 5   # optional
  enable-pure-downlink: false # 是否启用混淆下行，false的情况下能在保证数据安全的前提下极大提升下行速度，与客户端保持相同(如果此处为false，则要求aead不可为none)
```

[通用字段](./index.md)
