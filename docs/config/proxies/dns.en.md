# DNS

```{.yaml linenums="1"}
proxies:
- name: "dns-out"
  type: dns
```

The `dns` outbound hijacks requests to the internal [DNS](../dns/index.md) module, where all requests are handled internally.
