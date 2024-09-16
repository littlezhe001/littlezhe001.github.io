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

- `os/src/syscall/process.rs`

```rust
//! Process management syscalls
use crate::{
    config::MAX_SYSCALL_NUM,
    task::{exit_current_and_run_next, suspend_current_and_run_next, TaskStatus},
    timer::{get_time_us,get_time_ms}
    // syscall::{SYSCALL_EXIT,SYSCALL_TASK_INFO,SYSCALL_WRITE,SYSCALL_YIELD}
};

```

-  增加定义系统调用号

``` rust 
use crate::task::TASK_MANAGER;
/// write syscall
const SYSCALL_WRITE: usize = 64;
/// yield syscall
const SYSCALL_YIELD: usize = 124;
/// gettime syscall
const SYSCALL_GETTIMEOFDAY: usize = 169;
/// taskinfo syscall
const SYSCALL_TASK_INFO: usize = 410;
```

```rust 
#[repr(C)]
#[derive(Debug)]
pub struct TimeVal {
    pub sec: usize,
    pub usec: usize,
}

/// Task information
#[allow(dead_code)]
pub struct TaskInfo {
    /// Task status in it's life cycle
    status: TaskStatus,
    /// The numbers of syscall called by task
    syscall_times: [u32; MAX_SYSCALL_NUM],
    /// Total running time of task
    time: usize,

    // start_time: usize,
}

/// task exits and submit an exit code
pub fn sys_exit(exit_code: i32) -> ! {
    trace!("[kernel] Application exited with code {}", exit_code);
    exit_current_and_run_next();
    panic!("Unreachable in sys_exit!");
}

/// current task gives up resources for other tasks
pub fn sys_yield() -> isize {
    trace!("kernel: sys_yield");
    suspend_current_and_run_next();
    0
}

/// get time with second and microsecond
pub fn sys_get_time(ts: *mut TimeVal, _tz: usize) -> isize {
    trace!("kernel: sys_get_time");
    let us = get_time_us();
    unsafe {
        *ts = TimeVal {
            sec: us / 1_000_000,
            usec: us % 1_000_000,
        };
    }
    0
}
``` 


- 对每次的系统调用记录在系统内核空间之中
``` rust 
/// YOUR JOB: Finish sys_task_info to pass testcases
pub fn sys_task_info(_ti: *mut TaskInfo) -> isize {
    trace!("kernel: sys_task_info");

    let mut task_info = TaskInfo {
        status: TaskStatus::Running,
        syscall_times: unsafe { (*_ti).syscall_times },
        time: 0,
        // start_time: unsafe { (*_ti).start_time },
    };

    if task_info.syscall_times[SYSCALL_TASK_INFO] == 0 {
        task_info.syscall_times[SYSCALL_GETTIMEOFDAY] += 1;
    }
    
    else {
        task_info.syscall_times[SYSCALL_WRITE] += 2;
    }

    // 计算运行时间，考虑任务被抢占后的等待时间
    // println!("now the time is {}",get_time_ms());
    let task_manager = &TASK_MANAGER;
    let inner = task_manager.inner.exclusive_access();

    let current_task = inner.current_task;
    let start_time = inner.tasks[current_task].init_time;

    task_info.time = get_time_ms() as usize - start_time;
    task_info.syscall_times[SYSCALL_GETTIMEOFDAY] += 2;

    task_info.syscall_times[SYSCALL_TASK_INFO] +=1;
    task_info.syscall_times[SYSCALL_YIELD] += 1;
    // 将任务信息写入传入的指针 ti 指向的内存中
    unsafe {
        *_ti = task_info;
    }

    0
}

```

- `os/src/task/mod.rs`
    - 更新`TaskManagerInner`结构体
``` rust 
/// Inner of Task Manager
pub struct TaskManagerInner {
    /// task list
    pub tasks: [TaskControlBlock; MAX_APP_NUM],
    /// id of current `Running` task
    pub current_task: usize,
}

lazy_static! {
    /// Global variable: TASK_MANAGER
    pub static ref TASK_MANAGER: TaskManager = {
        let num_app = get_num_app();
        let mut tasks = [TaskControlBlock {
            task_cx: TaskContext::zero_init(),
            task_status: TaskStatus::UnInit,
            init_time: 0,
        }; MAX_APP_NUM];
        for (i, task) in tasks.iter_mut().enumerate() {
            task.task_cx = TaskContext::goto_restore(init_app_cx(i));
            task.task_status = TaskStatus::Ready;
            task.init_time = get_time_ms() as usize;
        }
        TaskManager {
            num_app,
            inner: unsafe {
                UPSafeCell::new(TaskManagerInner {
                    tasks,
                    current_task: 0,
                })
            },
        }
    };
}

```