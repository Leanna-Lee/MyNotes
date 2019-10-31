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
- Uses a modified guest operation system.
- The guest communicates with the hypervisor. The hypervisor passes the unmodified calls from the guest to the CPU and other interfaces, both real and virtual.
- Because the calls are routed through the hypervisor, this method is slower than full virtualization.
#### Software virtualization (or emulation)
- Uses binary translation and other emulation techniques to run unmodified operating systems.
- The hypervisor translates the guest calls to a format that can be used by the host system.
- Because all calls are translated, this method is slower than paravirtualization.
#### Containerization
While KVM virtualization creates a separate instance of OS kernel, operating-system-level virtualization, also known as containerization, operates on top of an existing OS kernel and creates isolated instances of the host OS, known as containers.   
Containers do not have the versatility of KVM virtualization, but are more lightweight and flexible to handle
## Virtualization Solutions