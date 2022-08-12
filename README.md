# 容器知识学习

- Linux 内核特性
   - [namespace](namespace/)
   - [cgroups](cgroups/)
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



