***[Getting Started Guide](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/GettingStartedGuide.md#getting-started-guide)***
- [What is Virtualization?](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/GettingStartedGuide.md#what-is-virtualization)
  - [Full virtualization](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/GettingStartedGuide.md#full-virtualization) 
  - [Paravirtualization](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/GettingStartedGuide.md#paravirtualization)
  - [Software virtualization](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/GettingStartedGuide.md#software-virtualization-or-emulation)
  - [Containerization](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/GettingStartedGuide.md#containerization)
- [Virtualization Solutions](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/GettingStartedGuide.md#virtualization-solutions)
# Getting Started Guide
**原文请参考：**  
[https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/virtualization_getting_started_guide/index](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/virtualization_getting_started_guide/index)
## What is Virtualization?
Virtualization is a broad computing term used for running software, usually multiple operating systems, concurrently and in isolation from other programs on a single system. Virtualization is accomplished by using a hypervisor. This is a software layer or subsystem that controls hardware and enables running multiple operating systems, called virtual machines (VMs) or guests, on a single (usually physical) machine. This machine with its operating system is called a host.

There are several virtualization methods:  
#### Full virtualization
- Uses an unmodified version of the guest operating system.
- The guest addresses the host's CPU via a channel created by the hypervisor.
- Because the guest communicates directly with the CPUs, this is the fastest virtualization method.
#### Paravirtualization
The guest communicates with the hypervisor. The hypervisor passes the unmodified calls from the guest to the CPU and other interfaces, both real and virtual. Because the calls are routed through the hypervisor, this method is slower than full virtualization.
- Uses a modified guest operation system.
- The guest communicates with the hypervisor. The hypervisor passes the unmodified calls from the guest to the CPU and other interfaces, both real and virtual.
- Because the calls are routed through the hypervisor, this method is slower than full virtualization.
#### Software virtualization (or emulation)
#### Containerization
## Virtualization Solutions