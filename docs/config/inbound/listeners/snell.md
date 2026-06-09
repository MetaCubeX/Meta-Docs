# Snell

```{.yaml linenums="1"}
listeners:
- name: snell-in-1
  type: snell
  port: 10815 # 支持使用ports格式，例如200,302 or 200,204,401-429,501-503
  listen: 0.0.0.0
  psk: your-password
  version: 4 # 仅支持 4/5
  udp: true # UDP over TCP tunnel，默认 true
  # obfs-opts:
  #   mode: http # 可选：http / tls
  #   host: bing.com
  # rule: sub-rule-name1 # 默认使用 rules，如果未找到 sub-rule 则直接使用 rules
  # proxy: proxy # 如果不为空则直接将该入站流量交由指定 proxy 处理 (当 proxy 不为空时，这里的 proxy 名称必须合法，否则会出错)

```
