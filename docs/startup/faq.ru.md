---
hide:
  # - navigation
  - toc
---
# Часто задаваемые вопросы

## Разница между ветками alpha и meta

Ветка alpha - это ветка с последними коммитами. Ветка meta периодически объединяет код из ветки alpha. Ветка meta не обязательно стабильнее, чем ветка alpha.

## Какой файл мне скачать?

В релизах имена пакетов содержат несколько параметров, включая:

* Название программы (`mihomo`)
* Операционная система (например, android, darwin, freebsd, linux, windows и т.д.)
* Архитектура (например, 386, amd64, arm32v7, arm64 и т.д.)
* Метод компиляции
>
> * `По умолчанию (без дополнительных отметок)`: версия по умолчанию, скомпилированная с тегом GOAMD64=v3
> * `compatible`: скомпилирована с тегом GOAMD64=v1. Эта версия предназначена для совместимости с определенными операционными системами или архитектурами.
> * `go120`: скомпилирована с использованием Golang1.20. Эта версия предназначена для совместимости с определенными операционными системами или архитектурами.
> * `abi1/2`: версия abi для `loongarch64`
>
* Ветка (alpha)
* Значение git hash коммита (например, f90066f)

Вы можете выбрать нужный исполняемый файл на основе этой информации.

👉[Узнайте больше](https://github.com/golang/go/wiki/MinimumRequirements#amd64) о тегах GOAMD64

👉[Узнайте больше](https://go.dev/doc/go1.20#ports) о системной совместимости версии Golang1.20

👉[Узнайте больше](http://www.loongnix.cn/zh/toolchain/Golang/downloads-Go1.21/index.html) о версиях abi для `loongarch64` 