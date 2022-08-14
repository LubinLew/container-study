# namepsace

轻量级进程虚拟化

namepsace 没有名字, 使用时是通过 inode 号来指定 namespace

/proc/<pid>/ns/

Each namespace has a unique inode number

This inode number of a each namespace is created when the namespace is created.

```c
/*
 * A structure to contain pointers to all per-process
 * namespaces - fs (mount), uts, network, sysvipc, etc.
 *
 * The pid namespace is an exception -- it's accessed using
 * task_active_pid_ns.  The pid namespace here is the
 * namespace that children will use.
 *
 * 'count' is the number of tasks holding a reference.
 * The count for each namespace, then, will be the number
 * of nsproxies pointing to it, not the number of tasks.
 *
 * The nsproxy is shared by tasks which share all namespaces.
 * As soon as a single namespace is cloned or unshared, the
 * nsproxy is copied.
 */
struct nsproxy {
	atomic_t count;
	struct uts_namespace *uts_ns;
	struct ipc_namespace *ipc_ns;
	struct mnt_namespace *mnt_ns;
	struct pid_namespace *pid_ns_for_children;
	struct net 	     *net_ns;
	struct time_namespace *time_ns;
	struct time_namespace *time_ns_for_children;
	struct cgroup_namespace *cgroup_ns;
};
extern struct nsproxy init_nsproxy;
```


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


- [lsns](cmds/lsns.md)
- [unshare](cmds/unshare.md) - util with support for all the 6 namespaces
- [nsenter](cmds/nsenter.md) - a wrapper around setns().
- [nsexec](cmds/nsexec.md)
- [ip netns](cmds/ipnetns.md)

## 参考文档

https://en.wikipedia.org/wiki/Linux_namespaces

https://www.kernel.org/doc/html/latest/admin-guide/namespaces/index.html
