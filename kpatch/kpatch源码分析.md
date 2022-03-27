希望实现的功能是，能够通过该脚本生成自己本地的`patch` 文件，组要包括及部分。

* 自己指定代码源文件
* 使用自己的编译器



## 核心目录

| 文件         | 作用 |
| ------------ | ---- |
| kpatch-build |      |
| kpatch       |      |
| kmod         |      |
|              |      |
|              |      |

## kpatch-build目录

`kpatch-build` 通过`patch` 文件生成内核补丁模块。

### 工作原理

* 首先编译一个没有裁剪符号表的内核

* 给源代码打`patch`

* 重新编译内核，查看哪些文件重新编译之后改变了

* 添加 `-ffunction-sections -fdata-sections`选项，重新编译每个改变了的`object`文件

* 取消`patch`

* 添加 `-ffcuntion-sections -fdata-sections` 选项，重新编译改变了的`object` 文件

* 对每个改变了的文件，用`create-diff-object` 执行下列操作：

  * 分析每个源问进/修改文件是否可以生成`patch`

  * 输出`object` 中生成`.kpatch.funcs` 和 `.rela.kpatch.funcs` 段。

    `kpatch core`通过该信息获取需要重定向的函数列表。

  * 输出`object` 文件中生成`.kpatch.dynrelas` 和 `.rela.kpatch.dynrelas` 段。

    `kpatch core` 通过这些信息获取解析本地符号表。

  * 生成新的`object` 文件

* 链接所有的`object` 文件到一个大的文件中

* 生成模块



### 脚本参数

| 参数                  | 说明                                    | 补充解释       |
| --------------------- | --------------------------------------- | -------------- |
| patchN                | 输入补丁列表                            |                |
| -h, --help            | 显示使用方法                            |                |
| -a, --archversion     | 指定内核版本信息                        |                |
| -r, --sourcerpm       | 指定内核源码rpm包                       |                |
| -s, --sourcedir       | 指定内核源码路径                        |                |
| -c, --config          | 指定内核配置文件                        |                |
| -v, --vmlinux         | 指定内核之前的`vmlinux` 文件            | 此时不需要指定 |
| -j, --jobs            | 指定`make` 运行的线程数                 |                |
| -t, --target          | 指定需要重新编译的部分(vmlinux或module) |                |
| -n, --name            | 指定生成模块名称                        |                |
| -o, --output          | 指定文件输出路径                        |                |
| -d, --debug           | 使能`xtrace` 功能                       |                |
| --oot-modules         | out-of-tree 模块编译                    |                |
| --oot-module-src      | out-of-tree模块编译，模块代码路径       |                |
| -R, --non-replace     | 关闭内核`livepatch` 功能，默认是开启    |                |
| --skip-cleanup        | 跳过编译后清空                          |                |
| --skip-compiler-check | 跳过编译器版本匹配                      |                |









## kpatch目录

