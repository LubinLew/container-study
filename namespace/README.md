# namepsace

轻量级进程虚拟化

## namespace 种类

| 命令空间 | Kernel版本 | 对应宏                                                                                           |说明       |
|----------|------------|------------------------------------------------------------------------------------------------- |-----------|
| mnt      | 2.4.19     | [CLONE_NEWNS](https://elixir.bootlin.com/linux/latest/source/include/uapi/linux/sched.h#L20)     |           |
| pid      | 2.6.24     | [CLONE_NEWPID](https://elixir.bootlin.com/linux/latest/source/include/uapi/linux/sched.h#L32)    |           |
| ipc      | 2.6.19     | [CLONE_NEWIPC](https://elixir.bootlin.com/linux/latest/source/include/uapi/linux/sched.h#L32)    |           |
| uts      | 2.6.19     | [CLONE_NEWUTS](https://elixir.bootlin.com/linux/latest/source/include/uapi/linux/sched.h#L29)    |           |
| net      | 2.6.29     | [CLONE_NEWNET](https://elixir.bootlin.com/linux/latest/source/include/uapi/linux/sched.h#L33)    |           |
| user     | 3.8        | [CLONE_NEWUSER](https://elixir.bootlin.com/linux/latest/source/include/uapi/linux/sched.h#L31)   |           |
| cgroup   | 4.6        | [CLONE_NEWCGROUP](https://elixir.bootlin.com/linux/latest/source/include/uapi/linux/sched.h#L28) |           |
| time     | 5.6        | [CLONE_NEWTIME](https://elixir.bootlin.com/linux/latest/source/include/uapi/linux/sched.h#L44)   |           |

----

## 相关函数

| 函数     | 新进程 | 新namespace | 说明                                                                 |
|----------|--------|-------------|----------------------------------------------------------------------|
| clone    | V      | V           | 创建一个新的进程和一个新的 namespace, 并将进程 attach 到新 namespace |
| unshare  | X      | V           | 创建一个新的 namespace, 并将当前进程 attach 到新 namespace           |
| setns    | X      | X           | 并将当前进程 attach 到一个已经存在的 namespace                       |

### 函数声明

```c
#define _GNU_SOURCE
#include <sched.h>

int clone(int (*fn)(void *), void *stack, int flags, void *arg, ...
          /* pid_t *parent_tid, void *tls, pid_t *child_tid */ );

int setns(int fd, int nstype);

int unshare(int flags);
```

`clone()` 、`unshare()` 系统调用函数可以通过设置 `CLONE_NEW*` 系列宏创建新的 namespace。

> `setns()` 通过参数 `nstype` 设置 `CLONE_NEW*`, 这是可选的。
>
> `clone()` 、`unshare()` 通过参数 `flags` 设置 `CLONE_NEW*`。

----

## 相关命令

- [lsns]](cmds/lsns.md)
- [unshare](cmds/unshare.md)
- [nsenter](cmds/nsenter.md)
- [nsexec](cmds/nsexec.md)

## 参考文档

https://en.wikipedia.org/wiki/Linux_namespaces

https://www.kernel.org/doc/html/latest/admin-guide/namespaces/index.html
