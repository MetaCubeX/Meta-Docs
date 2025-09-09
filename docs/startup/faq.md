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
> * `v1/2/3`：仅适用于AMD64平台，用于标记[CPU指令集等级](https://en.wikipedia.org/wiki/X86-64#Microarchitecture_levels)
> * ~~`默认（无额外标识）`: 使用 GOAMD64=v3 标签编译的默认版本~~
> * ~~`compatible`: 使用 GOAMD64=v1 标签进行编译。该版本是为了兼容特定的操作系统或架构而编译的。~~
> * `go120`: 使用 Golang1.20 版本进行编译。该版本是为了兼容特定的操作系统或架构而编译的。
> * `abi1/2`: `loongarch64`的 abi 版本
>
* 分支（alpha）
* 提交的 git hash 值（如 f90066f）

可以根据这些信息选择你需要下载的可执行文件。

👉[在此了解](https://go.dev/wiki/MinimumRequirements#amd64)更多关于 GOAMD64 标签的信息

👉[在此了解](https://go.dev/doc/go1.20#ports)更多关于 Golang1.20 版本的系统兼容性信息

👉[在此了解](http://www.loongnix.cn/zh/toolchain/Golang/downloads-Go1.21/index.html)更多关于`loongarch64`abi 版本的信息

对于macos用户：根据[go wiki](https://go.dev/doc/go1.25#darwin)，go1.25开始不再支持macos11，请macos11用户下载带有`go124`标签的二进制文件，macos10.15用户下载带有`go122`标签的二进制文件，macoc10.13用户下载带有`go120`标签的二进制文件

对于windows用户：目前官方构建的所有版本均支持win7及更高版本的系统（非官方构建不保证这一点，我们通过维护[自己fork版本的golang](https://github.com/MetaCubeX/go)来编译）

对于linux用户：根据[go wiki](https://go.dev/doc/go1.24#linux)，go1.24开始仅支持3.2+版本内核，请2.6.32~3.1版本内核用户下载带有`go123`标签的二进制文件
