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

大多数情况传入的数据都为`'{"path": "", "payload": ""}'`，可以附带新的配置文件路径

## 日志

### `/logs`

请求方法：`GET`

- 获取实时日志

## 流量信息

### `/traffic`

请求方法：`GET`

- 获取实时流量，单位 kbps

## 内存信息

### `/memory`

请求方法：`GET`

- 获取实时内存占用，单位 kb

## 版本信息

### `/version`

请求方法：`GET`

- 获取 Clash 版本

## 缓存

### `/cache/fakeip/flush`

请求方法：`POST`

- 清除 fakeip 缓存

## 运行配置

### `/configs`

请求方法：`GET`

- 获取基本配置

请求方法：`PUT`

- 重新加载基本配置，必须发送数据，URL 需携带 `?force=true` 强制执行

请求方法：`PATCH`

- 更新基本配置，必须发送数据，格式为`'{"mixed-port": 7890}'`，按需修改为需要更新的配置项

### `/configs/geo`

请求方法：`POST`

- 更新 GEO 数据库，必须发送数据

### `/restart`

请求方法：`POST`

- 重启内核，必须发送数据

## 更新

### `/upgrade`

请求方法：`POST`

- 更新内核，必须发送数据

### `/upgrade/ui`

请求方法：`POST`

- 更新面板，须设置 [external-ui](../config/general.md#_7)

### `/upgrade/geo`

请求方法：`POST`

- 更新 GEO 数据库，必须发送数据

## 策略组

### `/group`

请求方法：`GET`

- 获取策略组信息

### `/group/group_name`

请求方法：`GET`

- 获取具体的策略组信息

### `/group/group_name/delay`

请求方法：`GET`

- 对指定策略组内的节点/策略组进行测试，并返回新的延迟信息，URL 需携带`?url=xxx&timeout=5000`，按需修改

## 代理

### `/proxies`

请求方法：`GET`

- 获取代理信息

### `/proxies/proxies_name`

请求方法：`GET`

- 获取具体的代理信息

请求方法：`PUT`

- 选择特定的代理，需携带数据，格式为`'{"name":"日本"}'`

### `/proxies/proxies_name/delay`

请求方法：`GET`

- 对指定代理进行测试，并返回新的延迟信息，URL 需携带`?url=xxx&timeout=5000`，按需修改

## 代理集合

### `/providers/proxies`

请求方法：`GET`

- 获取所有代理集合的所有信息

### `/providers/proxies/providers_name`

请求方法：`GET`

- 获取特定代理集合的信息

请求方法：`PUT`

- 更新代理集合

### `/providers/proxies/providers_name/healthcheck`

请求方法：`GET`

- 触发特定代理集合的健康检查

### `/providers/proxies/providers_name/proxies_name/healthcheck`

- 对代理集合内的指定代理进行测试，并返回新的延迟信息，URL 需携带`?url=xxx&timeout=5000`，按需修改

## 规则

### `/rules`

请求方法：`GET`

- 获取规则信息

## 规则集合

### `/providers/rules`

请求方法：`GET`

- 获取所有规则集合的所有信息

### `/providers/rules/providers_name`

请求方法：`PUT`

- 更新规则集合

## 连接

### `/connections`

请求方法：`GET`

- 获取连接信息

请求方法：`DELETE`

- 关闭所有连接

### `/connections/:id`

请求方法：`DELETE`

- 关闭特定连接

## 域名查询

### `/dns/query`

请求方法：`GET`

- 获取指定名称和类型的 DNS 查询数据，URL 需携带`?name=example.com&type=A`，按需修改

## DEBUG

`/debug` 需要内核启动时 [日志级别](../config/general.md#_5) 为 `debug`

### `/debug/gc`

请求方法：`PUT`

- 进行主动 GC

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

浏览器访问 `http://${controller-api}/debug/pprof/heap?raw=true` 即可下载这个文件，通过上传到 [issues](https://github.com/MetaCubeX/Clash.Meta/issues) 提交你遇到的问题。
