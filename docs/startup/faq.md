---
hide:
  # - navigation
  - toc
---
# 常见问题

## alpha 和 meta 分支的区别

alpha 分支为最新提交分支，meta 分支每隔一段时间合并 alpha 分支的代码，meta 分支不一定比 alpha 分支更稳定。

## 我应该下载哪一个文件？

release 中，包的文件名中包含了多个信息，包括

* 程序名称 (`mihomo`)
* 操作系统（如 android、darwin、freebsd、linux、windows 等）
* 架构（如 386、amd64、arm32v7、arm64 等）
* 编译方式
>
> * `默认（无额外标识）`: 使用 GOAMD64=v3 标签编译的默认版本
> * `compatible`: 使用 GOAMD64=v1 标签进行编译。该版本是为了兼容特定的操作系统或架构而编译的。
> * `go120`: 使用 Golang1.20 版本进行编译。该版本是为了兼容特定的操作系统或架构而编译的。
> * `abi1/2`: `loongarch64`的 abi 版本
>
* 分支（alpha）
* 提交的 git hash 值（如 f90066f）

可以根据这些信息选择你需要下载的可执行文件。

👉[在此了解](https://github.com/golang/go/wiki/MinimumRequirements#amd64)更多关于 GOAMD64 标签的信息

👉[在此了解](https://go.dev/doc/go1.20#ports)更多关于 Golang1.20 版本的系统兼容性信息

👉[在此了解](http://www.loongnix.cn/zh/toolchain/Golang/downloads-Go1.21/index.html)更多关于`loongarch64`abi 版本的信息
