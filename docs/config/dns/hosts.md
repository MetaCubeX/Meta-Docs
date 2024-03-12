# hosts

```{.yaml linenums="1"}
hosts:
  '*.clash.dev': 127.0.0.1
  'alpha.clash.dev': '::1'
  test.com: [1.1.1.1, 2.2.2.2]
  baidu.com: google.com
  home.lan
```

hosts 域名支持通配，例如 `*.clash.dev`或 `+.example.com`,别名 (示例最后两个) 不支持通配

支持单域名多 ip，格式为数组

!!! note
    完整的的域名优先级高于使用通配符的域名，例如：foo.example.com > \*.example.com > .example.com
