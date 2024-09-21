# hosts

```{.yaml linenums="1"}
hosts:
  '*.clash.dev': 127.0.0.1
  'alpha.clash.dev': '::1'
  test.com: [1.1.1.1, 2.2.2.2]
  baidu.com: google.com
```

Keys support [domain wildcards](../../handbook/syntax.md#domain-wildcards).

Values support strings/arrays; domain redirection does not support arrays.

!!! note
    A complete domain takes precedence over domains using wildcards, for example: foo.example.com > *.example.com > .example.com.