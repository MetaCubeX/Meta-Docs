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
    http-mask: true
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

## http-mask

是否启用http掩码
