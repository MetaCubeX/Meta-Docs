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
    http-mask: true
    # http-mask-strategy: random
    enable-pure-downlink: false
```

[通用字段](./index.md)

## key

如果你使用sudoku生成的ED25519密钥对，请填写密钥对中的私钥，否则填入和服务端相同的uuid

## aead-method

可选值：`chacha20-poly1305`、`aes-128-gcm`、`none` 我们保证在none的情况下sudoku混淆层仍然确保安全

## padding-min

最小填充字节数

## padding-max

最大填充字节数

## table-type

可选值：prefer_ascii、prefer_entropy 前者全ascii映射，后者保证熵值（汉明1）低于3

## custom-table

可选，自定义字节布局，必须包含2个x、2个p、4个v，可随意组合。启用此处则需配置`table-type`为`prefer_entropy`

## custom-tables

可选，自定义字节布局列表（x/v/p），用于 xvp 模式轮换；非空时覆盖 custom-table

## http-mask

是否启用http掩码

## http-mask-strategy

可选：random（默认）、post、websocket；仅在 http-mask=true 时生效

## enable-pure-downlink

是否启用混淆下行，false的情况下能在保证数据安全的前提下极大提升下行速度，与服务端端保持相同(如果此处为false，则要求aead不可为none)
