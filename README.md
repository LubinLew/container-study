# 容器知识学习

## 发展历史

> https://zhuanlan.zhihu.com/p/358548091

1979年，第一个 Unix 发布版本 v7，就集成了 `chroot` 命令，
## 背景简介

商业服务器通常性能十分强大(几十个核心，几百GB的内存，万兆网卡等)，传统的应用程序无法有效的利用其强大的性能，这时虚拟化产品 VMware、KVM、OpenStack 等应运而生，目前我们熟知的各种云厂商提供的云主机服务基本都是基于 OpenStack 进行二次开发的。借助虚拟化技术，能够对物理资源进行更合理的使用，从而真正发挥硬件的价值。

随着软件行业的发展，很多软件功能越来越多，越来越复杂，软件的架构经历了 单体架构、SOA(面向服务架构)、微服务架构的变革。

容器和虚拟机的目的都是隔离资源，保证系统安全，然后是尽量提高资源的利用率。

容器并不直接运行在 Docker 上，Docker 只是辅助建立隔离环境，让容器基于 Linux 操作系统运行。

- Linux 内核特性
   - [namespace](namespace/)
   - [cgroups](cgroups/)
   - [chroot/pivot_root/LXC]
   - [UnionFS](unionfs/)

```c
// https://elixir.bootlin.com/linux/latest/source/include/linux/sched.h
struct task_struct {
...
  /* namespace */
  struct nsproxy  *nsproxy;
...
  /* cgroups */
#ifdef CONFIG_CGROUPS
  /* Control Group info protected by css_set_lock: */
  struct css_set __rcu  *cgroups;
  /* cg_list protected by css_set_lock and tsk->alloc_lock: */
  struct list_head  cg_list;
#endif
....
}
```

-----

- 容器
   - OCI 标准
      - RunTime Spec
      - Image Spec
   - [overlay 文件系统](overlay/)
   - 容器运行时(RunTime)
      - runC
      - Kata
   - 容器工具
      - docker/podman
      - cri-o
      - containterd

   - 容器镜像
      - [镜像标准]
      - [镜像构建]
      - [镜像安全](security/)



