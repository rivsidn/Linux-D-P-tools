## 安装

### 源代码安装

* 代码下载[How do I get LMbench?](http://www.bitmover.com/lmbench/get_lmbench.html)

* 编译 `make`

* 问题整理

  * 问题一

    ```bash
    # 问题描述
    disk.c:(.text+0x37): undefined reference to `llseek'
    # 代码修改
    $ git diff src/disk.c
    diff --git a/src/disk.c b/src/disk.c
    index c3f1154..8aebb6d 100644
    --- a/src/disk.c
    +++ b/src/disk.c
    @@ -289,9 +289,9 @@ int
     seekto(int fd, uint64 off)
     {
     #ifdef __linux__
    -       extern  loff_t llseek(int, loff_t, int);
    +       extern  loff_t lseek(int, loff_t, int);
     
    -       if (llseek(fd, (loff_t)off, SEEK_SET) == (loff_t)-1) {
    +       if (lseek(fd, (loff_t)off, SEEK_SET) == (loff_t)-1) {
                    return(-1);
            }
            return (0);
    ```

  * 问题二

    ```bash
    # 问题描述
    gmake[2]: *** No rule to make target `../SCCS/s.ChangeSet', needed by `bk.ver'.  Stop.
    # 修改
    mkdir SCCS
    touch SCCS/s.ChangeSet
    ```

### 软件包安装

```bash
sudo apt-get install lmbench
```



## 使用

### 源代码使用

* `make results` 
  * 根据提示做一些配置选项
  * 生成测试结果
* `make see` 查看测试结果

### 软件包使用

```bash
sudo lmbench-run
```



## 附录

### 参考资料

* [LMbench 安装与使用](https://scoolor.github.io/2019/01/01/LMbench/)
* [How do I get LMbench?](http://www.bitmover.com/lmbench/get_lmbench.html)

