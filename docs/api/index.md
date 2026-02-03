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

## 流量信息

### `/traffic`

!!! info ""
    获取实时流量，单位 kbps

- 请求方法：`GET` / `WS`

## 内存信息

### `/memory`

!!! info ""
    获取实时内存占用，单位 kb

- 请求方法：`GET` / `WS`

## 版本信息

### `/version`

!!! info ""
    获取 mihomo 版本信息

- 请求方法：`GET`

## 缓存

### `/cache/fakeip/flush`

!!! info ""
    清除 fakeip 缓存

- 请求方法：`POST`

### `/cache/dns/flush`

!!! info ""
    清除 dns 缓存

- 请求方法：`POST`

## 运行配置

### `/configs`

!!! info ""
    获取基本配置

- 请求方法：`GET`

!!! info ""
    重新加载基本配置

- 请求方法：`PUT`
- 携带参数：`?force=true`

!!! info ""
    更新基本配置

- 请求方法：`PATCH`
- 携带数据：`'{"mixed-port": 7890}'`

### `/configs/geo`

!!! info ""
    更新 GEO 数据库

- 请求方法：`POST`
- 发送数据：`'{"path": "", "payload": ""}'`

### `/restart`

!!! info ""
    重启内核

- 请求方法：`POST`
- 发送数据：`'{"path": "", "payload": ""}'`

## 更新

### `/upgrade`

!!! info ""
    更新内核

- 请求方法：`POST`
- 发送数据：`'{"path": "", "payload": ""}'`

### `/upgrade/ui`

!!! info ""
    更新面板，须设置 [external-ui](../config/general.md#_7)

- 请求方法：`POST`

### `/upgrade/geo`

!!! info ""
    更新 GEO 数据库

- 请求方法：`POST`
- 发送数据：`'{"path": "", "payload": ""}'`

## 策略组

### `/group`

!!! info ""
    获取策略组信息

- 请求方法：`GET`

### `/group/group_name`

!!! info ""
    获取具体的策略组信息

- 请求方法：`GET`

!!! info ""
    清除自动策略组 fixed 选择

- 请求方法：`DELETE`

### `/group/group_name/delay`

!!! info ""
    对指定策略组内的节点/策略组进行测试，返回新的延迟信息，并清除自动策略组的 fixed 选择

- 请求方法：`GET`
- 携带参数：`?url=xxx&timeout=5000`

## 代理

### `/proxies`

!!! info ""
    获取代理信息

- 请求方法：`GET`

### `/proxies/proxies_name`

!!! info ""
    获取具体的代理信息

- 请求方法：`GET`

!!! info ""
    选择特定的代理

- 请求方法：`PUT`
- 携带数据：`'{"name":"日本"}'`

### `/proxies/proxies_name/delay`

!!! info ""
    对指定代理进行测试，并返回新的延迟信息

- 请求方法：`GET`
- 携带参数：`?url=xxx&timeout=5000`

## 代理集合

### `/providers/proxies`

!!! info ""
    获取所有代理集合的所有信息

- 请求方法：`GET`

### `/providers/proxies/providers_name`

!!! info ""
    获取特定代理集合的信息

- 请求方法：`GET`

!!! info ""
    更新代理集合

- 请求方法：`PUT`

### `/providers/proxies/providers_name/healthcheck`

!!! info ""
    触发特定代理集合的健康检查

- 请求方法：`GET`

### `/providers/proxies/providers_name/proxies_name/healthcheck`

!!! info ""
    对代理集合内的指定代理进行测试，并返回新的延迟信息

- 请求方法：`GET`
- 携带参数：`?url=xxx&timeout=5000`

## 规则

### `/rules`

!!! info ""
    获取规则信息

- 请求方法：`GET`

### `/rules/disable`

!!! info ""
    禁用规则，其中 key 为规则的索引，value 为是否禁用该规则，为临时操作，重启后失效

- 请求方法：`PATCH`
- 携带数据：`'{"0": false,"1": true}'`

## 规则集合

### `/providers/rules`

!!! info ""
    获取所有规则集合的所有信息

- 请求方法：`GET`

### `/providers/rules/providers_name`

!!! info ""
    更新规则集合

- 请求方法：`PUT`

## 连接

### `/connections`

!!! info ""
    获取连接信息

- 请求方法：`GET` / `WS`
- 可选参数：`?interval=milliseconds`, 其中 `milliseconds` 为刷新间隔，默认值为 1000 毫秒

!!! info ""
    关闭所有连接

- 请求方法：`DELETE`

### `/connections/:id`

!!! info ""
    关闭特定连接

- 请求方法：`DELETE`

## 域名查询

### `/dns/query`

!!! info ""
    获取指定名称和类型的 DNS 查询数据

- 请求方法：`GET`
- 携带参数：`?name=example.com&type=A`

## DEBUG

`/debug` 需要内核启动时 [日志级别](../config/general.md#_5) 为 `debug`

### `/debug/gc`

!!! info ""
    进行主动 GC

- 请求方法：`PUT`

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
