## 内核编译

| 选项                         | 功能                                    |
| ---------------------------- | --------------------------------------- |
| CONFIG_FUNCTION_TRACER       | ftrace功能                              |
| CONFIG_FUNCTION_GRAPH_TRACER | graph ftrace功能                        |
| CONFIG_DYNAMIC_FTRACE        | 动态ftrace 功能，livepatch 依赖于改功能 |
| CONFIG_LIVEPATCH             | 使能livepatch                           |



`livepatch` 功能实现依赖于动态`ftrace` 功能。



## 代码示例

代码实现可以查看`samples/livepatch`。

**Tips：针对同一个函数可以添加多个livepatch，最后一个生效**



## 用户态接口

可以在  `sys/kernel/livepatch` 下关闭对应的`livepatch`，只有关闭之后的模块才可以卸载。

