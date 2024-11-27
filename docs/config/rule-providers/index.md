# rule-provider

```{.yaml linenums="1"}
rule-providers:
  google:
    type: http
    path: ./rule1.yaml 
    url: "https://raw.githubusercontent.com/../Google.yaml"
    interval: 600
    proxy: DIRECT
    behavior: classical
    format: yaml
    size-limit: 0
```

## name

必须，如`google`,不能重复

## type

必须，`provider`类型，可选`http` / `file`

## url

类型为`http`则必须配置

## path

可选，文件路径，不可重复，不填写时会使用 url 的 MD5 作为此文件的文件名

由于安全问题，此路径将限制只允许在 `HomeDir`(有启动参数 -d 配置) 中，如果想存储到任意位置配置环境变量 `SKIP_SAFE_PATH_CHECK=1`

## interval

更新`provider`的时间，单位为秒

## proxy

经过指定代理进行下载/更新

## behavior

行为，可选`domain`/`ipcidr`/`classical`，对应不同格式的 rule-provider 文件格式，请按实际格式填写

## format

格式，可选 `yaml`/`text`/`mrs`，默认 `yaml`

`mrs`目前 `behavior` 仅支持 `domain`/`ipcidr`，可以通过`mihomo convert-ruleset domain/ipcidr yaml/text XXX.yaml XXX.mrs`转换得到

## size-limit

限制下载文件的最大大小，默认为 0 即不限制文件大小，单位为字节 (`b`)
