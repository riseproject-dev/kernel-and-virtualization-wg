# ACPI RQSC Proof of Concept

This page describes the software Proof of Concept (PoC) for the ACPI RQSC table which describes QoS controllers in the system that implement the [Capacity and Bandwidth QoS Register Interface (CBQRI)](https://github.com/riscv-non-isa/riscv-cbqri/blob/main/qos_intro.adoc).

This PoC builds upon the [original CBQRI PoC](https://lists.riscv.org/g/tech-cbqri/topic/cbqri_software/98372428). Linux now supports CBQRI controller information passed through Device Tree or ACPI. The choice depends on how Qemu is invoked - see below for details.

## Resources

### ACPI RQSC table spec

[https://github.com/riscv-non-isa/riscv-rqsc/tree/main/src](https://github.com/riscv-non-isa/riscv-rqsc/tree/main/src)

> The Capacity and Bandwidth QoS controllers in a system are described by the ACPI RQSC table. The actual register interface is specified in the RISC-V Capacity and Bandwidth Controller QoS Register Interface specification.
>
> The RQSC table describes the properties of the QoS controllers in the system. The table also describes the topological arrangement of the QoS controllers and resources in the system. The topology is expressed in terms of the location of the resources within the system and the relation between the QoS Controller and the resource it manages.

### RVI PRS meeting

[RQSC slides](https://docs.google.com/presentation/d/1YJvsXjx6AaTM4n2rTBrmLuW9xiktUaY7smdvb9cFwfY/edit#slide=id.g343d027d434_0_1) from RVI Platform Runtime Services (PRS) TG meeting on March 27, 2025.

## Linux

* Repo: [https://github.com/tt-fustini/linux](https://github.com/tt-fustini/linux)
* Branch: dfustini/cbqri-mpam-snapshot-v6.14-rc1

    * Based on the James Morse MPAM support snapshot for 6.14: [mpam/snapshot/v6.14-rc1](https://web.git.kernel.org/pub/scm/linux/kernel/git/morse/linux.git/log/?h=mpam/snapshot/v6.14-rc1)

        * Update: [Some of the patches](https://lore.kernel.org/all/20250311183715.16445-1-james.morse@arm.com/#t) to decouple resctrl from arch/x86 [got merged for 6.15](https://lore.kernel.org/all/20250325171320.GAZ-LkMAFoSM7ZhDqq@fat_crate.local/)

    * Note: riscv defconfig works okay but a smaller subset of drivers to reduce build time are enabled in the downstream `linux.config`

## Qemu

* Repo: [GitHub - tt-fustini/qemu](https://github.com/tt-fustini/qemu)
* Branch: riscv-cbqri-rqsc-pptt
* Build and run: `./build.sh && ./run-acpi.sh`

    * TODO: add note about dependencies to build Qemu and EDK2

`run-acpi.sh`

```bash
#!/bin/bash
export LX=$HOME/dev/linux
export BR=$HOME/dev/images
export EDK=$HOME/dev/edk2
build/qemu-system-riscv64 \
	-M virt,pflash0=pflash0,pflash1=pflash1,aia=aplic-imsic  \
	-nographic \
	-m 1G \
	-smp cpus=8,sockets=1,clusters=2,cores=4,threads=1 \
	-serial mon:stdio \
	-kernel ${LX}/arch/riscv/boot/Image \
	-append "root=/dev/vda ro loglevel=8 ro console=ttyS0 rootwait earlycon=uart8250,mmio,0x10000000 acpi.debug_layer=0xffffffff acpi.debug_level=0xffffffff" \
	-blockdev node-name=pflash0,driver=file,read-only=on,filename=$EDK/RISCV_VIRT_CODE.fd \
	-blockdev node-name=pflash1,driver=file,filename=$EDK/RISCV_VIRT_VARS.fd \
	-drive if=none,file=${BR}/rootfs.ext2,format=raw,id=hd0 \
	-device virtio-blk-device,drive=hd0 \
	-device qemu-xhci \
	-device usb-kbd \
	-device virtio-net-pci,netdev=net0 \
	-netdev user,id=net0 \
	-device riscv.cbqri.capacity,max_mcids=256,max_rcids=64,ncblks=12,alloc_op_flush_rcid=false,mon_op_config_event=false,mon_op_read_counter=false,mon_evt_id_none=false,mon_evt_id_occupancy=false,mmio_base=0x04820000 \
	-device riscv.cbqri.capacity,max_mcids=256,max_rcids=64,ncblks=12,alloc_op_flush_rcid=false,mon_op_config_event=false,mon_op_read_counter=false,mon_evt_id_none=false,mon_evt_id_occupancy=false,mmio_base=0x04821000 \
	-device riscv.cbqri.capacity,max_mcids=256,max_rcids=64,ncblks=16,mmio_base=0x0482B000 \
	-device riscv.cbqri.bandwidth,max_mcids=256,max_rcids=64,nbwblks=1024,mrbwb=819,mmio_base=0x04828000 \
	-device riscv.cbqri.bandwidth,max_mcids=256,max_rcids=64,nbwblks=1024,mrbwb=819,mmio_base=0x04829000 \
	-device riscv.cbqri.bandwidth,max_mcids=256,max_rcids=64,nbwblks=1024,mrbwb=819,mmio_base=0x0482a000
```

### PPTT table in Qemu

* Dump and disassembled PPTT table: [https://gist.github.com/tt-fustini/70ac54ed6ef13c8baf4462d7ca8f6fd4](https://gist.github.com/tt-fustini/70ac54ed6ef13c8baf4462d7ca8f6fd4)
* PoC based on last PPTT [series](https://lore.kernel.org/all/20240129104039.117671-1-jeeheng.sia@starfivetech.com/) from Sia Jee Heng (StarFive)
* Proper upstream solution would be to collaborate on ARM [patches](https://lore.kernel.org/all/20250310162337.844-1-alireza.sanaee@huawei.com/)

### RQSC table in Qemu

* Dump and disassembled RQSC table: [https://gist.github.com/tt-fustini/cd4b89a52c72debb4308bee68b580cce](https://gist.github.com/tt-fustini/cd4b89a52c72debb4308bee68b580cce)

#### Cache ID

Qemu uses the CBQRI controller's mmio_base address as the Cache ID in both the PPTT and RQSC tables for cache controllers. A real platform would probably do it differently but this is a simple way to ensure the Cache ID is unique.

The Linux PPTT driver is now able to populate the cache information for each cpu. Example of cpu0 (cluster 0) and cpu4 (cluster 1). They have different instances of the L2 cache controller (note: sysfs prints the Cache ID in decimal).

```
 for i in `find /sys/devices/system/cpu/cpu0/cache/index* -type f | grep -E '/(type|size|id|level)'`; do echo "$i: `cat $i`"; done
/sys/devices/system/cpu/cpu0/cache/index0/id: 29
/sys/devices/system/cpu/cpu0/cache/index0/type: Data
/sys/devices/system/cpu/cpu0/cache/index0/size: 64K
/sys/devices/system/cpu/cpu0/cache/index0/level: 1
/sys/devices/system/cpu/cpu0/cache/index1/id: 30
/sys/devices/system/cpu/cpu0/cache/index1/type: Instruction
/sys/devices/system/cpu/cpu0/cache/index1/size: 64K
/sys/devices/system/cpu/cpu0/cache/index1/level: 1
/sys/devices/system/cpu/cpu0/cache/index2/id: 75632640
/sys/devices/system/cpu/cpu0/cache/index2/type: Unified
/sys/devices/system/cpu/cpu0/cache/index2/size: 750K
/sys/devices/system/cpu/cpu0/cache/index2/level: 2
/sys/devices/system/cpu/cpu0/cache/index3/id: 75673600
/sys/devices/system/cpu/cpu0/cache/index3/type: Unified
/sys/devices/system/cpu/cpu0/cache/index3/size: 3072K
/sys/devices/system/cpu/cpu0/cache/index3/level: 3
```

### resctrl demo

The full resctrl user interface is documented in the kernel: [https://docs.kernel.org/arch/x86/resctrl.html](https://docs.kernel.org/arch/x86/resctrl.html)

Example commands to mount and modify the schemata:

```
mount -t resctrl resctrl /sys/fs/resctrl

ls -la /sys/fs/resctrl/

cd /sys/fs/resctrl/

cat /sys/fs/resctrl/schemata

echo 'L2:4=ff0' >  /sys/fs/resctrl/schemata

cat /sys/fs/resctrl/schemata
```
