`ubuntu 18.04` 中通过`apt` 安装的`kpatch` 使用存在问题，本文中直接使用的是`github` 最新的代码。



### 内核代码编译

```
CONFIG_LIVEPATCH=y
CONFIG_DEBUG_INFO=y
CONFIG_FUNCTION_TRACER=y
CONFIG_DYNAMIC_FTRACE=y
```

使能编译选项，编译内核。

### 代码下载

```bash
git clone https://github.com/dynup/kpatch.git
```

### kpatch代码修改

```diff
diff --git a/kpatch-build/kpatch-build b/kpatch-build/kpatch-build
index d0397d4..c79f503 100755
--- a/kpatch-build/kpatch-build
+++ b/kpatch-build/kpatch-build
@@ -132,8 +132,8 @@ apply_patches() {
 	local patch
 
 	for patch in "${PATCH_LIST[@]}"; do
-		patch -N -p1 --dry-run < "$patch" 2>&1 | logger || die "$patch file failed to apply"
-		patch -N -p1 < "$patch" 2>&1 | logger || die "$patch file failed to apply"
+		patch -N -p0 --dry-run < "$patch" 2>&1 | logger || die "$patch file failed to apply"
+		patch -N -p0 < "$patch" 2>&1 | logger || die "$patch file failed to apply"
 		(( APPLIED_PATCHES++ ))
 	done
 }
@@ -145,7 +145,7 @@ remove_patches() {
 	for (( ; APPLIED_PATCHES>0; APPLIED_PATCHES-- )); do
 		idx=$(( APPLIED_PATCHES - 1))
 		patch="${PATCH_LIST[$idx]}"
-		patch -p1 -R -d "$BUILDDIR" < "$patch" &> /dev/null
+		patch -p0 -R -d "$BUILDDIR" < "$patch" &> /dev/null
 	done
 
 	# If $BUILDDIR was a git repo, make sure git actually sees that
@@ -258,7 +259,9 @@ find_dirs() {
 
 find_core_symvers() {
 	SYMVERSFILE=""
-	if [[ -e "$SCRIPTDIR/create-diff-object" ]]; then
+	if [[ -n "${USERSRCDIR}" ]] && [[ -f "${USERSRCDIR}/Module.symvers" ]]; then
+		SYMVERSFILE="${USERSRCDIR}/Module.symvers"
+	elif [[ -e "$SCRIPTDIR/create-diff-object" ]]; then
 		# git repo
 		SYMVERSFILE="$DATADIR/core/Module.symvers"
 	elif [[ -e "$SCRIPTDIR/../libexec/kpatch/create-diff-object" ]]; then

```

修改之后，编译安装。

### 准备内核`patch`

```diff
$ cat svn_diff.patch 
Index: net/ipv4/ip_input.c
===================================================================
--- net/ipv4/ip_input.c	(revision 8579)
+++ net/ipv4/ip_input.c	(working copy)
@@ -464,6 +464,9 @@
 		goto inhdr_error;
 
 	iph = ip_hdr(skb);
+	{
+		printk("livepatch %pI4-->%pI4\n", &iph->saddr, &iph->daddr);
+	}
 
 	/*
 	 *	RFC1122: 3.2.1.2 MUST silently discard any IP frame that fails the checksum.
```

### kpatch生成模块补丁模块

```bash
kpatch-build --sourcedir /home/yuchao/workspace/blade/trunk_newkernel/platform/kernel.NEW-x86 --skip-compiler-check --name ip_rcv svn_diff.patch
```

命令执行后会生成`ip_rcv.ko` 文件



### 补丁模块文件加载

```
# insmod ip_rcv.ko
# dmesg
livepatch 127.0.0.1-->127.0.0.1
livepatch 127.0.0.1-->127.0.0.1
livepatch 1.2.0.9-->1.2.0.255
livepatch 172.31.3.140-->172.31.3.136
livepatch 172.31.3.140-->172.31.3.136
livepatch 172.31.3.140-->172.31.3.136
livepatch 172.31.3.140-->172.31.3.136
```





## 附录

### 参考资料

* [kpatch github](https://github.com/dynup/kpatch)

### 内核config配置文件

不能确保开启上边的配置选项就一定有效，保存当前配置文件与之前文件`diff`。

```
$ diff .config .config.old
243d242
< CONFIG_KPROBES_ON_FTRACE=y
533d531
< CONFIG_LIVEPATCH=y
3221d3218
< # CONFIG_PSTORE_FTRACE is not set
3346,3350c3343
< CONFIG_DEBUG_INFO=y
< # CONFIG_DEBUG_INFO_REDUCED is not set
< # CONFIG_DEBUG_INFO_SPLIT is not set
< # CONFIG_DEBUG_INFO_DWARF4 is not set
< # CONFIG_GDB_SCRIPTS is not set
---
> # CONFIG_DEBUG_INFO is not set
3385a3379
> # CONFIG_KMEMCHECK is not set
3460d3453
< CONFIG_GENERIC_TRACER=y
3463,3464c3456
< CONFIG_FUNCTION_TRACER=y
< CONFIG_FUNCTION_GRAPH_TRACER=y
---
> # CONFIG_FUNCTION_TRACER is not set
3466a3459
> # CONFIG_ENABLE_DEFAULT_TRACERS is not set
3478,3482d3470
< CONFIG_DYNAMIC_FTRACE=y
< CONFIG_DYNAMIC_FTRACE_WITH_REGS=y
< # CONFIG_FUNCTION_PROFILER is not set
< CONFIG_FTRACE_MCOUNT_RECORD=y
< # CONFIG_FTRACE_STARTUP_TEST is not set
```



