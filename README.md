# Meta-Docs

[Clash.Meta](https://github.com/MetaCubeX/Clash.Meta/tree/Alpha) 的文档

## 使用方法

### 设置虚拟环境

python3.11以上依赖虚拟环境运行,不能直接运行

##### 安装virtualenv

* debian
  ```shell
  apt update && apt install virtualenv
  ```
* archlinux
  ```shell
  pacman -Syu python-virtualenv
  ```

##### 创建虚拟环境

```shell
virtualenv venv
```

##### 使用虚拟环境

```shell
source venv/bin/activate
```

##### 退出虚拟环境

```shell
deactivate
```

##### 确认是否处于虚拟环境

```shell
which python3
```

### 设置源

```shell
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

### 安装依赖

```shell
pip install -r requirements.txt
```

### 预览修改

```shell
mkdocs serve
```

### 参考文档

https://squidfunk.github.io/mkdocs-material/setup/
https://squidfunk.github.io/mkdocs-material/reference/

推荐使用 [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) 对文档进行格式化。
