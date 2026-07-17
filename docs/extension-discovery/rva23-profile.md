# RVA23 Profile

## About

The RVA23 profiles are intended to be used for 64-bit application processors that will run rich OS stacks from standard binary OS distributions and with a substantial number of third-party binary user applications that will be supported over a considerable length of time in the field. The approach is to provide a large guaranteed set of features that can safely be exploited by third-party developers to ship binaries that will provide a better experience across a large number of alternative implementations over time. It is explicitly a non-goal of RVA23 to allow more hardware implementation flexibility by supporting only a minimal set of features.

Only user-mode (RVA23U64) and supervisor-mode (RVA23S64) profiles are specified in this family.

Refer: [RVA23 Profile :: RISC-V Ratified Specifications Library](https://docs.riscv.org/reference/rva23/index.html)

## Status

| **Profile Ext. Name** | **Supervisor Detection** | **Kernel HWPROBE** | **KVM ONE_REG** | **Kernel Version** | **KVM Version** | **Comments** |
| --- | --- | --- | --- | --- | --- | --- |
| **RVA22U64 Mandatory** |
| M | ISA String | COMPLETED | COMPLETED | Linux-6.4 | Linux-5.19 | |
| A | ISA String | COMPLETED | COMPLETED | Linux-6.4 | Linux-5.19 | |
| F | ISA String | COMPLETED | COMPLETED | Linux-6.4 | Linux-5.19 | |
| D | ISA String | COMPLETED | COMPLETED | Linux-6.4 | Linux-5.19 | |
| C | ISA String | COMPLETED | COMPLETED | Linux-6.4 | Linux-5.19 | |
| B | ISA String | NA | NA | | | Bundle of extensions: Zba, Zbb, and Zbs |
| Zicsr | ISA String | NA | COMPLETED | | Linux-6.6 | Implied by F |
| Zicntr | ISA String | COMPLETED | COMPLETED | Linux-6.15 | Linux-6.6 | Mandatory for Linux RISC-V |
| Zihpm | ISA String | COMPLETED | COMPLETED | Linux-6.15 | Linux-6.6 | Separate ioctl() available for user-space HPM access |
| Ziccif | ISA String | NA | TBD | | | Does this need HWPROBE support? |
| Ziccrse | ISA String | NA | COMPLETED | | Linux-6.14 | Does this need HWPROBE support? |
| Ziccamoa | ISA String | NA | TBD | | | Does this need HWPROBE support? |
| Zicclsm | ISA String | NA | TBD | | | Does this need HWPROBE support? |
| Za64rs | ISA String | NA | NA | | | |
| Zihintpause | ISA String | COMPLETED | COMPLETED | Linux-6.10 | Linux-6.1 | |
| Zba | ISA String | COMPLETED | COMPLETED | Linux-6.5 | Linux-6.6 | Implied by B |
| Zbb | ISA String | COMPLETED | COMPLETED | Linux-6.5 | Linux-6.4 | Implied by B |
| Zbs | ISA String | COMPLETED | COMPLETED | Linux-6.5 | Linux-6.6 | Implied by B |
| Zic64b | ISA String | NA | NA | | | HWPROBE and KVM ONE_REG will have separate attributes for Zicbom, Zicbop, and Zicboz block size |
| Zicbom | ISA String | COMPLETED | COMPLETED | Linux-6.15 | Linux-6.1 | |
| Zicbop | ISA String | COMPLETED | COMPLETED | Linux-6.19 | Linux-6.18 | |
| Zicboz | ISA String | COMPLETED | COMPLETED | Linux-6.7 | Linux-6.4 | |
| Zfhmin | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | |
| Zkt | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | |
| **RVA23U64 Mandatory** |
| V | ISA String | COMPLETED | COMPLETED | Linux-6.5 | Linux-6.5 | |
| Zvfhmin | ISA String | COMPLETED | COMPLETED | Linux-6.9 | Linux-6.8 | |
| Zvbb | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | |
| Zvkt | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | |
| Zihintntl | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | |
| Zicond | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.7 | |
| Zimop | ISA String | COMPLETED | COMPLETED | Linux-6.11 | Linux-6.11 | |
| Zcmop | ISA String | COMPLETED | COMPLETED | Linux-6.11 | Linux-6.11 | |
| Zcb | ISA String | COMPLETED | COMPLETED | Linux-6.11 | Linux-6.11 | |
| Zfa | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | |
| Zawrs | ISA String | COMPLETED | COMPLETED | Linux-6.11 | Linux-6.11 | |
| Supm | ISA String | COMPLETED | NA | Linux-6.13 | | |
| **RVA23U64 Optional (Localized)** |
| Zvkng | ISA String | COMPLETED | NA | | | Bundle of extensions: Zvkn and Zvkg |
| Zvksg | ISA String | COMPLETED | NA | | | Bundle of extensions: Zvks and Zvkg |
| Zvkn | ISA String | COMPLETED | NA | | | Implied by Zvkng. Bundle of extensions: Zvkned, Zvknhb, Zvkb and Zvkt |
| Zvks | ISA String | COMPLETED | NA | | | Implied by Zvksg. Bundle of extensions: Zvksed, Zvksh, Zvkb, and Zvkt |
| Zvkned | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | Implied by Zvkn |
| Zvknhb | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | Implied by Zvkn |
| Zvkb | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | Implied by Zvkn |
| Zvkt | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | Implied by Zvkn and Zvks |
| Zvkg | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | Implied by Zvkng and Zvksg |
| Zvksed | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | Implied by Zvks |
| Zvksh | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | Implied by Zvks |
| **RVA23U64 Optional (Development)** |
| Zabha | ISA String | COMPLETED | COMPLETED | Linux-6.16 | Linux-6.14 | |
| Zacas | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.9 | |
| Ziccamoc | ISA String | NA | NA | | | |
| Zvbc | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | |
| Zama16b | ISA String | NA | NA | | | |
| **RVA23U64 Optional (Expansion)** |
| Zfh | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | |
| Zbc | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | |
| Zicfilp | ISA String | COMPLETED | TBD | Linux-7.0 | | |
| Zicfiss | ISA String | COMPLETED | TBD | Linux-7.0 | | |
| Zvfh | ISA String | COMPLETED | COMPLETED | Linux-6.8 | Linux-6.8 | |
| Zfbfmin | ISA String | COMPLETED | COMPLETED | Linux-6.15 | Linux-6.18 | |
| Zvfbfmin | ISA String | COMPLETED | COMPLETED | Linux-6.15 | Linux-6.18 | |
| Zvfbfwma | ISA String | COMPLETED | COMPLETED | Linux-6.15 | Linux-6.18 | |
| **RVA22S64 Mandatory** |
| Svbare | DeviceTree or ACPI | NA | COMPLETED | | Linux-6.6 | |
| Sv39 | DeviceTree or ACPI | COMPLETED | COMPLETED | Linux-6.11 | Linux-6.6 | HWPROBE Key HIGHEST_VIRT_ADDRESS |
| Svade | ISA String | NA | COMPLETED | Linux-6.13 | Linux-6.13 | |
| Ssccptr | ISA String | NA | NA | | | Does this need HWPROBE and KVM ONE_REG support? |
| Sstvecd | ISA String | NA | TBD | | | |
| Sstvala | ISA String | NA | TBD | | | |
| Sscounterenw | ISA String | NA | TBD | | | |
| Svpbmt | ISA String | NA | COMPLETED | | Linux-6.0 | |
| Svinval | ISA String | NA | COMPLETED | | Linux-6.1 | |
| **RVA23S64 Mandatory** |
| Zifencei | ISA String | NA | COMPLETED | | Linux-6.6 | HWPROBE not required because mandatory for Linux |
| Ss1p13 | ISA String | NA | NA | | | Does this need HWPROBE and KVM ONE_REG support? |
| Svnapot | ISA String | NA | COMPLETED | | Linux-6.5 | |
| Sstc | ISA String | NA | COMPLETED | | Linux-6.0 | |
| Sscofpmf | ISA String | NA | COMPLETED | | Linux-6.10 | |
| Ssnpm | ISA String | NA | COMPLETED | | Linux-6.13 | |
| Ssu64xl | ISA String | NA | TBD | | | |
| Sha | ISA String | NA | NA | | | The augmented hypervisor extension. Bundle of extensions: H, Sstateen, Shcounterenw, Shvstvala, Shtvala, Shvstvecd, Shvsatpa, and Shgatpa |
| H | ISA String | NA | COMPLETED | | Linux-5.19 | Implied by Sha |
| Sstateen | ISA String | NA | TBD | | | Implied by Sha |
| Shcounterenw | ISA String | NA | TBD | | | Implied by Sha |
| Shvstvala | ISA String | NA | TBD | | | Implied by Sha |
| Shtvala | ISA String | NA | TBD | | | Implied by Sha |
| Shvstvecd | ISA String | NA | TBD | | | Implied by Sha |
| Shvsatpa | ISA String | NA | TBD | | | Implied by Sha |
| Shgatpa | ISA String | NA | TBD | | | Implied by Sha |
| **RVA22S64 Optional (Expansion)** |
| Sv48 | DeviceTree or ACPI | COMPLETED | COMPLETED | Linux-6.11 | Linux-6.6 | HWPROBE Key HIGHEST_VIRT_ADDRESS |
| Sv57 | DeviceTree or ACPI | COMPLETED | COMPLETED | Linux-6.11 | Linux-6.6 | HWPROBE Key HIGHEST_VIRT_ADDRESS |
| Zkr | ISA String | TBD | COMPLETED | | Linux-6.8 | |
| **RVA23S64 Optional (Expansion)** |
| Svadu | ISA String | NA | COMPLETED | | Linux-6.13 | |
| Sdtrig | ISA String | NA | TBD | | | |
| Ssstrict | ISA String | NA | TBD | | | |
| Svvptc | ISA String | NA | COMPLETED | | Linux-6.14 | |
| Sspm | ISA String | NA | TBD | | | |
