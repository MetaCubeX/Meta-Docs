# 语法

mihomo 使用 `yaml` 作为配置文件格式

`yaml` 大小写敏感，使用缩进表示层级关系，缩进不允许使用 tab, 只允许空格，缩进的空格数不重要，只要相同层级的元素左对齐即可

## 注释

在 `yaml` 格式的文件中，以"#"作为注释开头，行尾为结尾，"#"必须在行头或者必须在前方有空格，否则不视为注释

```{.yaml linenums="1"}
port: 7890 # http 代理端口
socks-port: 7891
# socks 代理端口
```

## 对象

对象键值对使用冒号结构表示`key: value`,冒号后面要加一个空格，使用缩进表示层级关系

因`yaml`格式为`json`的超集，所以可以直接使用`json`的写法

### 多行

```{.yaml linenums="1"}
tun:
  enable: true
  stack: system
  auto-route: true
  auto-detect-interface: true
```

### 多行 json

```{.yaml linenums="1"}
tun: { 
  enable: true,
  stack: system,
  auto-route: true,
  auto-detect-interface: true
  }
```

### 单行 json

```{.yaml linenums="1"}
tun: { enable: true, stack: system, auto-route: true, auto-detect-interface: true}
```

### 全 json

```{.json linenums="1"}
{
  "tun": {
    "enable": true,
    "stack": "system",
    "auto-route": true,
    "auto-detect-interface": true
  }
}
```

## 数组

以`-`开头的行表示构成一个数组，用于一个对象内的多个值

### 多行数组

```{.yaml linenums="1"}
a:
  - b
  - c
  - d
```

### 单行 json 数组

```{.yaml linenums="1"}
a: [b, c, d]
```

## 引用

`&` 锚点和 `*` 别名，可以用来引用，`&` 用来建立锚点，`<<`表示合并到当前数据，`*` 用来引用锚点

如下示例中，因`p`这个键在`Clash`中不存在，所以在`Clash`解析配置会被忽视

如合并时有重复的项，则不会去合并

```{.yaml linenums="1"}
p: &p
  type: http
  interval: 3600
  health-check:
    enable: true
    url: https://www.gstatic.com/generate_204
    interval: 300

proxy-providers:
  provider1:
    <<: *p
    url: ""
    path: ./proxy_providers/provider1.yaml

  provider2:
    <<: *p
    type: file
    path: ./proxy_providers/provider2.yaml
```

等同于

```{.yaml linenums="1"}
proxy-providers:
  provider1:
    type: http
    interval: 3600
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
    url: ""
    path: ./proxy_providers/provider1.yaml

  provider2:
    interval: 3600
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
    type: file
    path: ./proxy_providers/provider2.yaml
```

## **IPV6 地址**

在`Clash`配置中，应当使用`[]`来框选一个 IPV6 地址

```{.yaml linenums="1"}
[aaaa::a8aa:ff:fe09:57d8]
[aaaa::a8aa:ff:fe09:57d9]:853 # 带端口的 IPV6 地址
```

## 域名通配符

### 通配符 `*`

Clash 的通配符 \* 一次只能匹配一级域名

\*.baidu.com 只匹配 tieba.baidu.com 而不匹配 123.tieba.baidu.com 或者 baidu.com

`'*'`只匹配 localhost 等域

### 通配符 `+`

通配符 ＋ 类似 DOMAIN-SUFFIX, 可以一次性匹配多个级别

＋.baidu.com 匹配 tieba.baidu.com 和 123.tieba.baidu.com 或者 baidu.com

通配符 ＋ 只能用于域名前缀匹配

### 通配符 `.`

通配符 . 可以一次性匹配多个级别

.baidu.com 匹配 tieba.baidu.com 和 123.tieba.baidu.com, 但不能匹配 baidu.com

通配符 . 只能用于域名前缀匹配

### 使用示例

使用通配符时，应当使用引号 `''`或 `" "`将内容包裹起来，以免过度匹配

```{.yaml linenums="1"}
fake-ip-filter:
  - ".lan"
  - "xbox.*.microsoft.com"
  - "+.xboxlive.com"
  - localhost.ptlogin2.qq.com
```

## 端口范围

mihomo 可以使用 - 来匹配端口范围，使用 / 来区分多个端口/端口范围

### 示例

匹配 114 到 514 和 810 到 1919 端口，以及 65530 端口

```{.yaml linenums="1"}
114-514/810-1919/65530
```

<!--
## 时间格式

mihomo 支持两种时间格式，分别是整数和持续时间

=== "整数格式"
    ```{.yaml linenums="1"}
    interval: 3600
    ```

=== "持续时间格式"
    ```{.yaml linenums="1"}
    interval: 1h
    ``` 
-->
