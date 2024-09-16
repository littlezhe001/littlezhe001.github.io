# Lun Zhe

## Basic Information

- Age: 21
- Gender: Male
- Place of Origin: Jiaozuo City, Henan Province
- Active Applicant for Party Membership
- Date of Birth: February 18, 2003
- Contact Number: 13569109525
- WeChat: lz2673664288
- Email: lz2673664288@gmail.com

## Education Background

### China University of Mining and Technology

- School of Mathematics, Major in Mathematics, Sep 2021 - Jun 2022
- School of Computer Science and Technology, Major in Information Security, Sep 2022 - Present
- Weighted Average Score: 82.7
- Class Ranking: 32
- GPA: 3.41
- CET-4 Score: 499

## Honors and Awards

- Chunhui Inspirational Scholarship, School of Mathematics, May 2022
- Third-Class Scholarship, School of Mathematics, September 2022
- Third-Class Scholarship, School of Computer Science and Technology, September 2023
- Successful Participant Award, National Finals of China College Computer Contest, May 2023
- Excellence Award, May 1 Mathematical Modeling Competition, May 2022

## Professional Skills

- Programming Languages: C++/Python/Rust/Java/JavaScript/PHP
- Skills:
  - Familiar with RSA/Hash encryption principles and operations, capable of implementing encryption and decryption in multiple languages;
  - Experience in learning Crypto problems in CTF, proficient in using Python to script and crack RSA-type CTF cryptographic problems;
  - Familiar with the imgui desktop application framework based on OPENGL, capable of creating simple high-performance desktop applications, and able to control the program's resolution and refresh rate using imgui tools;
  - Internship experience in a network company, over 2 months at Henan Championship Group, capable of building web pages and WeChat mini-programs based on Vue front-end and Laravel back-end frameworks, and familiar with simple optimization methods for SQL databases.

## Club and Organization Experience

- Class Committee of Class 2, Grade 2021, School of Mathematics, Sep 2021 - Jun 2022
  - Study Committee Member
  - Managed daily study organization tasks for the class, organized collective study activities and peer-to-peer Q&A sessions during exam periods. During my tenure, the class achieved a 90% pass rate for the CET-4 English test and an 80% pass rate for major courses, and I was honored as an outstanding class cadre at the end of the academic year.
- Member of the China University of Mining and Technology's Calligraphy Club, Sep 2021 - Dec 2021
  - Member
  - Studied calligraphy with club members and created calligraphy works, participated in amateur calligraphy competitions with a passion for writing calligraphic art pieces.

## Hobbies and Interests

- Optimistic and cheerful personality, fond of sports, passionate about fitness, good at playing basketball and table tennis, with experience in participating in competitions and winning awards.

## Research Experience

### Rust-based RISC-V Kernel Source Code Writing (Demonstrable Live), Sep 2023 - Nov 2023

- Project Source: [Tsinghua University Rust Open Source Operating System Training Camp](https://github.com/LearningOS/2023-littlezhe001)
- Based on the Ubuntu system and QEMU simulator, simulate the risc64 system environment to run the system and directly modify the kernel source code.
  1. For the system's Task Manager module, added system call task information (TaskInfo) to increase the information of the Task Manager, so that every time the Task Manager is called, such as read, write, and other operations, and obtaining the current time, can be recorded in the kernel space array syscall_times.
  2. For the system call corresponding to the task information (TaskInfo), added elements to the structure (taskinfo), defined the running time element (time), and started recording the running time from the kernel space from the start of the call, in order to accurately measure the system call time.

### Linux Kernel Source Code Memory Protection and NX Bit, Mar 2023 - May 2023

- Advisor: Li Yonggang
- Identified how the Linux kernel protects memory management through system calls and functions using debugging tools like gdb or kernel observation tools like ebpf.
  1. Wrote memory overflow vulnerability code, ran it on the Centos7 operating system, and used the gdb tool to debug the vulnerability code step by step, observing the kernel functions and system calls (syscall) when the program was forcibly terminated by the Linux kernel due to a segmentation fault.
  2. Read papers related to address randomization (ASLR), reproduced the source code from the papers in Linux kernel versions above 4.0, and used the Python code (benchmark) from the paper to compare the addresses that have been randomized with those that have not been randomized.
  3. Learned how the Linux kernel protects system memory through hardware (NX bit) and used the ebpf tool to lock specified system calls, tracking how system call functions operate at the kernel level.

### Rust Learning under the Rustlings Framework, Mar 2024 - May 2024

- Project Source: [Tsinghua University Rust Open Source Operating System](https://github.com/LearningOS/rust-rustlings-2024-spring-littlezhe001)
- Conducted basic data structure construction with safety features in Rust, using Rust's characteristics to protect linked storage structures to prevent memory leaks.
  1. Learned the basic syntax of the Rust language and its basic data structures, set up the Rust compilation environment on the Ubuntu system.
  2. Engaged in multithreading programming in Rust and learned to implement basic semaphore mechanisms using atomic statements, writing simple programs to achieve mutual exclusion effects.
  3. Wrote basic data structures such as linked lists and binary trees, using Rust's lifetime features to prohibit memory management leaks (null pointers).
