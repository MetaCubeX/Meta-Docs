# DNS

```{.yaml linenums="1"}
proxies:
- name: "dns-out"
  type: dns
```

Исходящее соединение `dns` перехватывает запросы во внутренний модуль [dns](../dns/index.md), где все запросы обрабатываются внутренне 