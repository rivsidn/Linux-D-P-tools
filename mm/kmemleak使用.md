

| 操作                                      | 说明                             |
| ----------------------------------------- | -------------------------------- |
| echo 'scan' > /sys/kernel/debug/kmemleak  | 触发一次扫描                     |
| echo 'clear' > /sys/kernel/debug/kmemleak | 清除当前`kmemleak`记录的泄漏信息 |
|                                           |                                  |
|                                           |                                  |











## 附录

### 参考资料

* [内核检查内存泄漏的工具 --- kmemleak](https://blog.csdn.net/weixin_41944449/article/details/123441820)
* [深入理解内存泄漏检查kmemleak](https://www.eet-china.com/mp/a113356.html#:~:text=kmemleak的实现原理非常,被视为内存泄漏%E3%80%82)
* [详细讲解Linux内存泄漏检测实现原理与实现](https://zhuanlan.zhihu.com/p/585372167)

