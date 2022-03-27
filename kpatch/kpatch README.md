## 安装

```bash
sudo apt update
sudo apt install kpatch
sudo apt install kpatch-build
```



## 组成部分

| 工具               | 解释                              |
| ------------------ | --------------------------------- |
| kpatch-build       | 通过diff文件生成patch模块的工具   |
| patch module       | 包含替换函数和原函数数据的模块    |
| kpatch core module | 为提供patch module 注册的函数接口 |
| kpatch utility     | 管理patch的命令行工具             |



## 快速开始

### 生成patch

### 生成patch模块

```bash
kpatch-build -t vmlinux meminfo-string.patch
```

生成 `kpatch-meminfo-string.ko` 模块。

### 模块加载

```bash
sudo kpatch load kpatch-meminfo-string.ko
```





## 参考资料

* [kpatch github](https://github.com/dynup/kpatch)

