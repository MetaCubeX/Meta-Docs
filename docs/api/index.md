---
hide:
  - navigation
#   - toc
---

# API

## 请求示例

curl 示例 `curl -H 'Authorization: Bearer ${secret}'  http://${controller-api}/configs?force=true -d '{"path": "", "payload": ""}' -X PUT`

此请求附带 `'Authorization: Bearer ${secret}'` 请求头，其中：

- `${secret}` 为配置文件设置的[api](../config/general.md#api)密钥
- `${controller-api}` 为配置文件中设置的[api](../config/general.md#api)监听地址
- `?force=true` 为携带参数，部分请求需携带
- `'{"path": "", "payload": ""}'` 为要更新的资源的数据

!!! note
    如果需要传入路径，注意，如果路径不在 mihomo 工作目录，请手动设置`SAFE_PATHS`环境变量将其加入安全路径，该环境变量的语法同本操作系统的 PATH 环境变量解析规则（即 Windows 下以分号分割，其他系统下以冒号分割）。

## 日志

### `/logs`

!!! info ""
    获取实时日志

- 请求方法：`GET` / `WS`
- 可选参数：`?level=log_level`, 其中 `log_level` 可选值为 `info`, `warning`, `error`, `debug`
- 可选参数：`?format=structured`, 携带后输出结构化日志（包含 `time`、`level`、`message`、`fields` 字段）
- 返回字段（标准模式，每行推送一个 JSON）：
    - `type`：日志级别，可选值 `info` / `warning` / `error` / `debug`
    - `payload`：日志内容
- 返回字段（结构化模式 `?format=structured`）：
    - `time`：时间字符串（格式 `HH:MM:SS`）
    - `level`：日志级别
    - `message`：日志内容
    - `fields`：附加字段数组

## 流量信息

### `/traffic`

!!! info ""
    获取实时流量，单位 kbps

- 请求方法：`GET` / `WS`
- 返回字段（每秒推送一次）：
    - `up`：当前上传速率（字节/秒）
    - `down`：当前下载速率（字节/秒）
    - `upTotal`：累计上传字节数
    - `downTotal`：累计下载字节数

## 内存信息

### `/memory`

!!! info ""
    获取实时内存占用，单位 kb

- 请求方法：`GET` / `WS`
- 返回字段（每秒推送一次）：
    - `inuse`：当前内存使用量（字节）
    - `oslimit`：系统内存限制（字节，固定为 `0`）

## 版本信息

### `/version`

!!! info ""
    获取 mihomo 版本信息

- 请求方法：`GET`
- 返回字段：
    - `meta`：是否为 Meta 版本（`true` / `false`）
    - `version`：版本号字符串

## 缓存

### `/cache/fakeip/flush`

!!! info ""
    清除 fakeip 缓存

- 请求方法：`POST`
- 返回：无（HTTP 204）

### `/cache/dns/flush`

!!! info ""
    清除 dns 缓存

- 请求方法：`POST`
- 返回：无（HTTP 204）

## 运行配置

### `/configs`

!!! info ""
    获取基本配置

- 请求方法：`GET`
- 返回字段：当前运行配置的 JSON 对象，包含 `port`、`socks-port`、`mixed-port`、`mode`、`log-level`、`allow-lan`、`ipv6`、`tun` 等字段

!!! info ""
    重新加载基本配置

- 请求方法：`PUT`
- 携带参数：`?force=true`
- 返回：无（HTTP 204）

!!! info ""
    更新基本配置

- 请求方法：`PATCH`
- 携带数据：`'{"mixed-port": 7890}'`
- 返回：无（HTTP 204）

### `/configs/geo`

!!! info ""
    更新 GEO 数据库

- 请求方法：`POST`
- 发送数据：`'{"path": "", "payload": ""}'`
- 返回：无（HTTP 204）

### `/restart`

!!! info ""
    重启内核

- 请求方法：`POST`
- 发送数据：`'{"path": "", "payload": ""}'`
- 返回：无（HTTP 204）

## 更新

### `/upgrade`

!!! info ""
    更新内核

- 请求方法：`POST`
- 可选参数：`?channel=xxx` 指定更新通道，`?force=true` 强制更新
- 发送数据：`'{"path": "", "payload": ""}'`
- 返回字段：
    - `status`：固定为 `"ok"`

### `/upgrade/ui`

!!! info ""
    更新面板，须设置 [external-ui](../config/general.md#_7)

- 请求方法：`POST`
- 返回字段：
    - `status`：固定为 `"ok"`

### `/upgrade/geo`

!!! info ""
    更新 GEO 数据库

- 请求方法：`POST`
- 发送数据：`'{"path": "", "payload": ""}'`
- 返回：无（HTTP 204）

## 策略组

### `/group`

!!! info ""
    获取策略组信息

- 请求方法：`GET`
- 返回字段：
    - `proxies`：策略组对象数组，每个对象格式同 `/proxies/proxies_name`

### `/group/group_name`

!!! info ""
    获取具体的策略组信息

- 请求方法：`GET`
- 返回字段：策略组对象，格式同 `/proxies/proxies_name`

### `/group/group_name/delay`

!!! info ""
    对指定策略组内的节点/策略组进行测试，返回新的延迟信息，并清除自动策略组的 fixed 选择

- 请求方法：`GET`
- 携带参数：`?url=xxx&timeout=5000`
- 可选参数：`?expected=xxx`, 期望的 HTTP 响应状态码，支持范围写法（如 `200/204`、`200-299`）
- 返回字段：以节点名称为键、延迟毫秒数（`uint16`）为值的 JSON 对象，例如 `{"节点A": 120, "节点B": 350}`

## 代理

### `/proxies`

!!! info ""
    获取代理信息

- 请求方法：`GET`
- 返回字段：
    - `proxies`：以代理名称为键的对象，每个代理包含以下字段：
        - `name`：代理名称
        - `type`：代理类型（如 `Shadowsocks`、`VMess`、`DIRECT`、`Selector` 等）
        - `udp`：是否支持 UDP
        - `uot`：是否支持 UDP over TCP
        - `xudp`：是否支持 XUDP
        - `tfo`：是否启用 TCP Fast Open
        - `mptcp`：是否启用 MPTCP
        - `smux`：是否启用多路复用
        - `alive`：当前是否存活
        - `history`：延迟历史数组，每项含 `time`（时间）和 `delay`（毫秒）
        - `extra`：额外延迟历史（按测试 URL 分组）
        - `interface`：绑定的网卡名称
        - `routing-mark`：路由标记
        - `provider-name`：所属代理集合名称
        - `dialer-proxy`：底层拨号代理名称

### `/proxies/proxies_name`

!!! info ""
    获取具体的代理信息

- 请求方法：`GET`
- 返回字段：代理对象，字段同 `/proxies` 中的单个代理条目

!!! info ""
    选择特定的代理

- 请求方法：`PUT`
- 携带数据：`'{"name":"日本"}'`
- 返回：无（HTTP 204）

!!! info ""
    清除该代理/策略组的 fixed 固定选择（`Selector` 类型除外）

- 请求方法：`DELETE`
- 返回：无（HTTP 204）

### `/proxies/proxies_name/delay`

!!! info ""
    对指定代理进行测试，并返回新的延迟信息

- 请求方法：`GET`
- 携带参数：`?url=xxx&timeout=5000`
- 可选参数：`?expected=xxx`, 期望的 HTTP 响应状态码，支持范围写法（如 `200/204`、`200-299`）
- 返回字段：
    - `delay`：测试延迟（毫秒，`uint16`）

## 代理集合

### `/providers/proxies`

!!! info ""
    获取所有代理集合的所有信息

- 请求方法：`GET`
- 返回字段：
    - `providers`：以集合名称为键的对象，每个集合包含其元信息及代理列表

### `/providers/proxies/providers_name`

!!! info ""
    获取特定代理集合的信息

- 请求方法：`GET`
- 返回字段：代理集合对象，包含集合配置信息及 `proxies` 代理列表

!!! info ""
    更新代理集合

- 请求方法：`PUT`
- 返回：无（HTTP 204）

### `/providers/proxies/providers_name/healthcheck`

!!! info ""
    触发特定代理集合的健康检查

- 请求方法：`GET`
- 返回：无（HTTP 204）

### `/providers/proxies/providers_name/proxies_name`

!!! info ""
    获取代理集合内指定代理的信息

- 请求方法：`GET`
- 返回字段：代理对象，字段同 `/proxies/proxies_name`

### `/providers/proxies/providers_name/proxies_name/healthcheck`

!!! info ""
    对代理集合内的指定代理进行测试，并返回新的延迟信息

- 请求方法：`GET`
- 携带参数：`?url=xxx&timeout=5000`
- 返回字段：
    - `delay`：测试延迟（毫秒，`uint16`）

## 规则

### `/rules`

!!! info ""
    获取规则信息

- 请求方法：`GET`
- 返回字段：
    - `rules`：规则数组，每个元素包含：
        - `index`：规则索引（从 0 开始）
        - `type`：规则类型（如 `DOMAIN`、`IP-CIDR`、`GEOIP` 等）
        - `payload`：规则匹配内容
        - `proxy`：目标代理/策略组名称
        - `size`：规则条目数量（仅 `GEOIP` / `GEOSITE` 类型有效，其他为 `-1`）
        - `extra`（可选，存在时包含以下字段）：
            - `disabled`：是否已禁用
            - `hitCount`：命中次数
            - `hitAt`：最后命中时间
            - `missCount`：未命中次数
            - `missAt`：最后未命中时间

### `/rules/disable`

!!! info ""
    禁用规则，其中 key 为规则的索引，value 为是否禁用该规则，为临时操作，重启后失效

- 请求方法：`PATCH`
- 携带数据：`'{"0": false,"1": true}'`
- 返回：无（HTTP 204）

## 规则集合

### `/providers/rules`

!!! info ""
    获取所有规则集合的所有信息

- 请求方法：`GET`
- 返回字段：
    - `providers`：以规则集合名称为键的对象

### `/providers/rules/providers_name`

!!! info ""
    更新规则集合

- 请求方法：`PUT`
- 返回：无（HTTP 204）

## 连接

### `/connections`

!!! info ""
    获取连接信息

- 请求方法：`GET` / `WS`
- 可选参数：`?interval=milliseconds`, 其中 `milliseconds` 为刷新间隔，默认值为 1000 毫秒
- 返回字段：
    - `downloadTotal`：累计下载字节数
    - `uploadTotal`：累计上传字节数
    - `memory`：当前内存使用量（字节）
    - `connections`：活跃连接数组，每项包含：
        - `id`：连接唯一 ID
        - `metadata`：连接元数据（源/目标地址、协议、进程名称等）
        - `upload`：该连接已上传字节数
        - `download`：该连接已下载字节数
        - `start`：连接建立时间
        - `chains`：代理链路数组
        - `providerChains`：provider 代理链路数组
        - `rule`：命中规则类型
        - `rulePayload`：命中规则内容

!!! info ""
    关闭所有连接

- 请求方法：`DELETE`
- 返回：无（HTTP 204）

### `/connections/:id`

!!! info ""
    关闭特定连接

- 请求方法：`DELETE`
- 返回：无（HTTP 204）

## 域名查询

### `/dns/query`

!!! info ""
    获取指定名称和类型的 DNS 查询数据

- 请求方法：`GET`
- 携带参数：`?name=example.com&type=A`
- 返回字段：
    - `Status`：DNS 响应码（Rcode）
    - `Question`：查询问题数组
    - `TC`：是否截断（Truncated）
    - `RD`：是否期望递归（RecursionDesired）
    - `RA`：是否支持递归（RecursionAvailable）
    - `AD`：数据是否已认证（AuthenticatedData）
    - `CD`：是否禁止检查（CheckingDisabled）
    - `Answer`（可选）：答案记录数组，每项含 `name`、`type`、`TTL`、`data`
    - `Authority`（可选）：权威记录数组，格式同 `Answer`
    - `Additional`（可选）：附加记录数组，格式同 `Answer`

## 存储

### `/storage/key`

!!! info ""
    获取指定 key 的存储值，不存在时返回 `null`

- 请求方法：`GET`
- 返回字段：存储的任意合法 JSON 值，不存在时返回 `null`

!!! info ""
    写入指定 key 的存储值，数据须为合法 JSON，最大 1MB

- 请求方法：`PUT`
- 携带数据：`'{"foo": "bar"}'`
- 返回：无（HTTP 204）

!!! info ""
    删除指定 key 的存储值

- 请求方法：`DELETE`
- 返回：无（HTTP 204）

## DEBUG

`/debug` 需要内核启动时 [日志级别](../config/general.md#_5) 为 `debug`

### `/debug/gc`

!!! info ""
    进行主动 GC

- 请求方法：`PUT`
- 返回：无（HTTP 204）

### `/debug/pprof`

浏览器打开 `http://${controller-api}/debug/pprof` 可查看原始 DEBUG 信息，其中：

- allocs 表示每个函数调用的内存分配情况，包括在堆栈上和堆上分配的内存大小以及内存分配次数。这个报告主要是为了帮助我们找到代码中存在的内存泄漏、内存频繁申请等问题。
- heap 报告则给出了程序在堆上使用的内存的详细信息，其中包括被分配的内存块的大小、数量和地址，并且按照大小排序。这个报告主要是为了搜寻内存使用过高的地方，我们可以在 heap 报告中查看对象的大小，从而找到内存使用过高的地方。

#### 安装 [Graphviz](https://graphviz.org/download/),可查看图形化的 debug 信息

##### 查看图形化 Heap 报告

```shell
go tool pprof -http=:8080 http://127.0.0.1:xxxx/debug/pprof/heap
```

[Full image](../assets/image/api/heap.svg)

##### 查看图形化 Allocs 报告

```shell
go tool pprof -http=:8080 http://127.0.0.1:xxxx/debug/pprof/allocs
```

[示例输出](../assets/image/api/allocs.svg)

##### 提交输出报告

浏览器访问 `http://${controller-api}/debug/pprof/heap?raw=true` 即可下载这个文件，通过上传到 [issues](https://github.com/MetaCubeX/mihomo/issues) 提交你遇到的问题。
