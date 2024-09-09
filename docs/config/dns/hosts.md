# hosts

```{.yaml linenums="1"}
hosts:
  '*.clash.dev': 127.0.0.1
  'alpha.clash.dev': '::1'
  test.com: [1.1.1.1, 2.2.2.2]
  baidu.com: google.com
```

键支持[域名通配](../../handbook/syntax.md#_8)

值支持字符串/数组，域名重定向不支持数组

!!! note
    完整的的域名优先级高于使用通配符的域名，例如：foo.example.com > \*.example.com > .example.com
