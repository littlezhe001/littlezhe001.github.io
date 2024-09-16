---
layout: archive
title: "Rust Risc-V 系统内核训练"
author_profile: true
redirect_from:
  - /rust/
---

我们希望引入一个新的系统调用 `sys_task_info` 以获取当前任务的信息，定义如下：

```rust
fn sys_task_info(ti: *mut TaskInfo) -> isize

```

- `syscall ID: 410`


- 查询当前正在执行的任务信息，任务信息包括任务控制块相关信息（任务状态）、任务使用的系统调用及调用次数、系统调用时刻距离任务第一次被调度时刻的时长（单位 ms）。


```rust
struct TaskInfo {
    status: TaskStatus,
    syscall_times: [u32; MAX_SYSCALL_NUM],
    time: usize
}
```

- 参数：
    - ti: 待查询任务信息
- 返回值：执行成功返回0，错误返回-1
- 说明：
    - 相关结构已在框架中给出，只需添加逻辑实现功能需求即可。

    - 在我们的实验中，系统调用号一定小于 500，所以直接使用一个长为 `MAX_SYSCALL_NUM=500` 的数组做桶计数。

    - 运行时间 time 返回系统调用时刻距离任务第一次被调度时刻的时长，也就是说这个时长可能包含该任务被其他任务抢占后的等待重新调度的时间。

    - 由于查询的是当前任务的状态，因此 TaskStatus 一定是 Running。（助教起初想设计根据任务 id 查询，但是既不好定义任务 id 也不好写测例，遂放弃 QAQ）

    - 调用 `sys_task_info` 也会对本次调用计数。

- 实验目录要求

``` 
├── os (内核实现)
│   ├── Cargo.toml (配置文件)
│   └── src (所有内核的源代码放在 os/src 目录下)
│       ├── main.rs (内核主函数)
│       └── ...
├── reports (不是 report)
│   ├── lab1.md/pdf
│   └── ...
├── ...

```

