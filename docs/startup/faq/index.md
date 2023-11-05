---
hide:
  # - navigation
  - toc
---

# 常见问题

### alpha 和 meta 分支的区别

alpha 分支为最新提交分支，meta 分支每隔一段时间合并 alpha 分支的代码，meta 分支不一定比 alpha 分支更稳定。

### 我应该下载哪一个文件？

release 中，包的文件名中包含了多个信息，包括

- 程序名称（clash.meta）
- 操作系统（如 android、darwin、freebsd、linux、windows 等）
- 架构（如 386、amd64、arm32v7、arm64 等）
- 编译方式
  > - `默认（无额外标识）`: 使用 GOAMD64=v3 标签编译的默认版本。👉[在此了解](https://github.com/golang/go/wiki/MinimumRequirements#amd64)更多关于 GOAMD64 标签的信息。
  > - `cgo`: 使用 GOAMD64=v1 标签进行编译。该版本具有与默认版本不同的功能和特性，包括支持 lwip tun 堆栈。
  > - `compatible`: 使用 GOAMD64=v1 标签进行编译。该版本是为了兼容特定的操作系统或架构而编译的。
- 分支（alpha）
- 提交的 git hash 值（如 f90066f）

可以根据这些信息或使用 `uname -m` 检查设备的 CPU 架构, 并在发布页面中找到相应的版本的可执行文件。

