***[Migration](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/Migration.md#migration)***
- [Migration Types](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/Migration.md#migration-types)
- [Benefits of Migrating Virtual Machines](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/Migration.md#benefits-of-migrating-virtual-machine)
- [Virtualized to Virtualized Migratio](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/Migration.md#virtualized-to-virtualized-migratio)
# Migration
**原文请参考：**  
[https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/virtualization_getting_started_guide/index](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/virtualization_getting_started_guide/index)
## Migration Types
Migration describes the process of moving a guest virtual machine from one host to another.
- **Live migration**
Live migration is the process of migrating an active virtual machine from one physical host to another.
- **Offline migration**
An offline migration suspends the guest virtual machine, and then moves an image of the virtual machine's memory to the destination host. The virtual machine is then resumed on the destination host and the memory used by the virtual machine on the source host is freed.
## Benefits of Migrating Virtual Machine
- Load balancing
- Upgrading or making changes to the host
- Energy saving
- Geographic migration
## Virtualized to Virtualized Migration
As a special type of migration, Red Hat Enterprise Linux 7 provides tools for converting virtual machines from other types of hypervisors to KVM. The virt-v2v tool converts and imports virtual machines from Xen, other versions of KVM, and VMware ESX.