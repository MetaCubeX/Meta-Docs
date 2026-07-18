# 代理集合

```{.yaml linenums="1"}
proxy-providers:
  provider1:
    type: http
    url: "http://test.com"
    path: ./proxy_providers/provider1.yaml
    interval: 3600
    proxy: DIRECT
    size-limit: 0
    age-secret-key: AGE-SECRET-KEY-1ZTQLLN0A4U3ZTT3DCZKYN0CGZEZQLWX2DFTXUWMT4ZHR0N2UG6LSW9NT0N
    header:
      User-Agent:
      - "mihomo/1.18.3"
      Authorization:
      - 'token 1231231'
      # X-Age-Public-Key:
      # - 'age1xh86kh9v23vattr58yedspm3f57sxvnswu9krr6ns438amekx5gsd09uma'
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
      timeout: 5000
      lazy: true
      expected-status: 204
    override:
      tfo: false
      mptcp: false
      udp: true
      udp-over-tcp: false
      down: "50 Mbps"
      up: "10 Mbps"
      skip-cert-verify: true
      name-cert-verify: example.com
      dialer-proxy: proxy
      interface-name: tailscale0
      routing-mark: 233
      ip-version: ipv4-prefer
      additional-prefix: "provider1 prefix |"
      additional-suffix: "| provider1 suffix"
      proxy-name:
      - pattern: "IPLC-(.*?)倍"
        target: "iplc x $1"
      # override-expr:
      #   - '.name = "[provider1] " + .name'                   
      #   - '.plugin-opts.mode = "tls"'                        
      #   - '.alpn[] |= upcase'                                
      #   - 'del(.skip-cert-verify)'                           
      #   - '.name = (.name | trim | upcase)'                  
      #   - '.name = "[\(.type)] \(.name):\(.port)"'           
      #   - '(select(.port == 443) | .tls) = true'             
      #   - '.tags |= (unique | sort)'                         
      #   - '.names = [.servers[] | select(.enabled) | .name]' 
      #   - '.servers |= map(select(.enabled))'                
      #   - '.options |= with_entries(.key |= upcase)'         
      #   - '. | with_entries(.key |= upcase)'                 
    filter: "(?i)港|hk|hongkong|hong kong"
    exclude-filter: "xxx"
    exclude-type: "ss|http"
    payload:
      - name: "ss1"
        type: ss
        server: server
        port: 443
        cipher: chacha20-ietf-poly1305
        password: "password"
```

## name

必须，如`provider1`，不能重复，建议不要和[策略组](../proxy-groups/index.md#name)名称重复

## type

必须，`provider`类型，可选`http` / `file` / `inline`

## url

类型为`http`是则需要配置

## path

可选，文件路径，不可重复，不填写时会使用 url 的 MD5 作为此文件的文件名

由于安全问题，此路径将限制只允许在 `HomeDir`(有启动参数 -d 配置) 中，如果想存储到其他位置，请通过设置 `SAFE_PATHS` 环境变量指定额外的安全路径。该环境变量的语法同本操作系统的 PATH 环境变量解析规则（即 Windows 下以分号分割，其他系统下以冒号分割）

## interval

更新`provider`的时间，单位为秒

## proxy

经过指定代理进行下载/更新

## size-limit

限制下载文件的最大大小，默认为 0 即不限制文件大小，单位为字节 (`b`)

## age-secret-key

如果设置会age-secret-key尝试通过此secret解密age armor格式加密的配置文件

注意：

  * 对于加密内容，目前仅支持 [age-encryption.org/v1](https://age-encryption.org/v1) 的 official ASCII "armor" format
  * 对于key格式，目前仅支持 [age-encryption.org/v1](https://age-encryption.org/v1) 的 x25519 recipient type 和 The mlkem768-x25519 hybrid post-quantum recipient type
  * 目前核心不会主动将公钥发送给服务器，用户需要自行设置header里`X-Age-Public-Key`或者通过其他方式将公钥上传
  * 核心同样支持命令行参数`-age-secret-key`或`CLASH_AGE_SECRET_KEY`环境变量来加载加密配置文件

实用工具：

  * 您可以通过 `mihomo age keygen` 生成符合要求的 x25519 key
  * 您可以通过 `mihomo age keygen-pq` 生成符合要求的 mlkem768-x25519 key
  * 您可以通过 `mihomo age convert <secret_key>` 从 age-secret-key 导出 age-public-key
  * 您可以通过 `mihomo age decrypt <secret_key> <source_file> <target_file>` 将已加密文件解密，<source_file> 为 - 时会从标准输入读取，<target_file> 为 - 时会往标准输出写入
  * 您可以通过 `mihomo age encrypt <public_key> <source_file> <target_file>` 将未加密文件加密，<source_file> 为 - 时会从标准输入读取，<target_file> 为 - 时会往标准输出写入

参考实现：

  * Golang: [FiloSottile/age](https://github.com/FiloSottile/age)
  * Rust: [str4d/rage](https://github.com/str4d/rage)
  * Typescript: [FiloSottile/typage](https://github.com/FiloSottile/typage)

## header

自定义 http 请求头

## health-check

健康检查 (延迟测试)

### health-check.enable

是否启用，可选 `true/false`

### health-check.url

健康检查地址，推荐使用以下地址之一

=== "Cloudflare"
    ```yaml
    https://cp.cloudflare.com
    ```

=== "Google"
    ```yaml
    https://www.gstatic.com/generate_204
    ```

### health-check.interval

健康检查间隔时间，单位为秒

### health-check.timeout

健康检查超时时间，单位为毫秒

### health-check.lazy

懒惰状态，默认为`true`,不使用该集合节点时，不进行测试

### health-check.expected-status

参阅 [期望状态](../proxy-groups/index.md#expected-status)

## override

覆写节点内容，以下为支持的字段

### override.additional-prefix

为节点名称添加固定前缀

### override.additional-suffix

为节点名称添加固定后缀

### override.proxy-name

对节点名称内容进行替换，支持正则表达式，pattern 为替换内容，target 为替换目标

### override.其余配置项

参阅通用字段  [tfo](../proxies/index.md#tfo)

参阅通用字段  [mptcp](../proxies/index.md#mptcp)

参阅通用字段  [udp](../proxies/index.md#udp)

参阅`Shadowsocks`  [udp-over-tcp](../proxies/ss.md#udp-over-tcp)

参阅`Hysteria`/`Hysteria2`  [up](../proxies/hysteria2.md#updown)

参阅`Hysteria`/`Hysteria2`  [down](../proxies/hysteria2.md#updown)

参阅通用字段  [skip-cert-verify](../proxies/tls.md#skip-cert-verify)

参阅通用字段  [name-cert-verify](../proxies/tls.md#name-cert-verify)

参阅通用字段  [dialer-proxy](../proxies/index.md#dialer-proxy)

参阅通用字段  [interface-name](../proxies/index.md#interface-name)

参阅通用字段  [routing-mark](../proxies/index.md#routing-mark)

参阅通用字段  [ip-version](../proxies/index.md#ip-version)

## override.override-expr

这是一个基于 yq v4 风格的表达式配置覆盖子集。数组项按顺序执行，且生效时间晚于上面介绍的固定字段覆盖（如 `udp: true` 等）。

支持的路径形式包括 `.`, `.name`, `."a.b"`, `.["a.b"]`, `.items[0]`, `.items[-1]`，`[]` 可遍历数组或 mapping。赋值会创建缺失的 mapping 和非负数组索引，但不会穿过标量更新。

支持的操作包括 `=`、`|=`、`+=`、`-=`、`*=`、`del(.field)`、管道 `|`、逗号 union、`select`、`//` 默认值运算，以及常见比较和布尔运算。值支持 `null` / `~`、布尔值、十进制/十六进制整数、浮点数、双引号字符串、数组和双引号键的 mapping。

支持的函数包括 `length`、`keys`、`has`、`contains`、`select`、`reverse`、`sort`、`unique`、`flatten`、`any`、`all`、`map`、`map_values`、`filter`、`to_entries`、`from_entries`、`with_entries`、`any_c`、`all_c`、`test`、`sub`、`split`、`join`、`upcase`、`downcase`、`trim`、`tostring`、`tonumber`、`type` 和 `not`。

!!! warning "限制与注意事项"
    * 每一项最终必须恰好产生一个 mapping。未收集的多结果及 del(.) 不受支持。
    * 赋值右侧使用管道或 and/or 时必须加括号。from_entries 只接受字符串 key。
    * sort 只支持标量数组。any/all 只支持无参数形式。
    * ==/!= 按 yq 的标量文本值比较，不对数组或 mapping 做结构化比较。
    * <、<=、>、>= 只支持数字、字符串和 null。只有 false 和 null 按假值处理。
    * 普通函数参数不得产生多个结果；map/filter 等接收表达式的函数允许参数产生结果流。
    * 函数参数可使用逗号或分号分隔。
    * 求值采用值语义。越界读取不会扩展源数组，右侧的 flatten/map_values 也不会隐式修改源路径。
    * 需要修改源路径时请使用 |=。右侧没有结果时保留目标原值，不会自动创建 null。
    * mapping 流、keys 和 entries 转换按 key 排序，因为 map[string]any 不保留 YAML key 顺序。
    * 不支持的语言功能：变量（as/$x）、reduce、动态路径（.[$key]）、递归下降（..）。
    * 不支持的外部功能：多文档、文件/环境访问、YAML tag/样式/注释处理、yq CLI 参数。
    * 未在上面列出的 yq 语法、运算符和函数也不受支持。条件分支可改用 select 后串联多个赋值。

## filter

筛选满足关键词或[正则表达式](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)的节点，可以使用 `|` 区分多个正则表达式

## exclude-filter

排除满足关键词或[正则表达式](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)的节点，可以使用 `|` 区分多个正则表达式

## exclude-type

不支持正则表达式，通过 `|` 分割，根据节点类型排除

provider 的 `exclude-type` 使用配置文件中的 `type` 类型进行排除

## payload

内容，仅 `type` 为 `inline` 时生效

当 `http` 或 `file` 解析失败时，也可以使用 payload 作为备用代理
