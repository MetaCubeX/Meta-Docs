# rule-provider

```{.yaml linenums="1"}
rule-providers:
  google:
    type: http
    behavior: classical
    format: yaml
    path: ./rule1.yaml 
    url: "https://raw.githubusercontent.com/../Google.yaml"
    interval: 600
```

## rule-set文件格式

`behavior`参数有三种可选项：`domain` / `ipcidr` / `classical`，对应不同格式的rule-set文件格式。请按实际格式填写
