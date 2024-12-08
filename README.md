# ARCHIVED

<details>
  <summary><i>写在前面</i></summary>
原来自己东拼西凑弄的，休眠问题一直没有解决，需要对DSDT修复，太麻烦了。后来看到Git上韩国小哥找人做了aml文件，就直接拿过来试用了一下，发现已修复休眠问题，但他的kext文件可能由于版本问题，一进桌面就重启，就把最新版本无线网卡和蓝牙相关的文件替换，形成最终的版本。目前除了单向空投外，其它声卡网卡显卡等都完美黑化。

韩国小哥Git仓库地址：(https://github.com/awesometic/hackintosh-gigabyte-b760m-aorus-elite)

为照顾其他水友装黑，特共享出来，我的硬件配置在下节。
</details>

# Raptor Lake Hackintosh EFI

![image](https://github.com/user-attachments/assets/a8894bcf-e9d4-49d0-96e7-fae14bcf15ad)


## What is this

This repository contains the EFI directory of Intel RaptorLake i5-13490F and Gigabyte B760M Aorus Elite AX5 combo, with somewhat diverse additional components.

## Specification of my computer

| Component    | Product Name                                     | Note                                           |
|--------------|--------------------------------------------------|------------------------------------------------|
| CPU          | Intel 13th Gen Core i5-13490F                    |                        |
| Mainboard    | Gigabyte B760M Aorus Elite AX5                      |                                    |
| Memory       | KingSton DDR5 4800MHz 16GB 2EA                    |                                                |
| Graphics     | AMD Radeon RX 5500 XT 8GB GDDR6 RAW II Ultra |       |
| NVMe 1       | SanDisk 240G                              | macOS 14 installed                             |
| NVMe 2       | Fanxiang S790 1TB                              | Windows 11 installed                             |
| BT/WIFI      | Fenvi AXE3000 Pro                                | Intel AX210NGW                                 |


### Geekbench 6 benchmark results

> Performed only one time right after it boots. The score isn't important for me 🙂

- [CPU](https://browser.geekbench.com/v6/cpu/9297937)
  - Single-core **2205**
  - Multi-core **12228**
- GPU
  - [OpenCL](https://browser.geekbench.com/v6/compute/3278140) **38502**
  - [Metal](https://browser.geekbench.com/v6/compute/3278132) **58712**

## EFI structure

### WARNING

- I recommend you use this as only a reference resource
  - Obviously, this build may not be the best one
  - This EFI contains additional kexts in **config.plist** rather than only the essential things for B760M + RaptorLake CPU. You should remove them before using this on your PC

### Check this before you use

#### Generate your platform information

In the [config.plist](EFI/OC/config.plist) file, I've replaced the private serial codes into the `EDIT_HERE` words because to keep my personal information safe.

So if you are going to use this, you have to make sure that the `EDIT_HERE` texts are changed to yours. To generate the serial key, please refer to the [Dortania's OpenCore Guide](https://dortania.github.io/OpenCore-Install-Guide/AMD/zen.html#platforminfo). When you are about to generate one, you should select `iMacPro1,1` to properly use your machine.

> - If your CPU has less than 8 cores, go to `config.plist` file and find **PlatformInfo->Generic** and change the `ProcessorType` from `3841` to `1537`
> - If you want to change the CPU name, check this link and edit your `config.plist` file using [CPU-Name](https://github.com/corpnewt/CPU-Name)

#### About the Above 4G Decoding option in BIOS

In my setup, I disabled **Above 4G Decoding** in my BIOS and added `npci=0x2000` to the boot args because it doesn't boot with that option enabled.

If you want to use my EFI setup, you have to check whether this option enabled or not, if you want to enable **Above 4G Decoding** then you should remove that `npci=0x2000` arguments from the `boot-args`.


### OpenCore

- Version: 0.9.5

### ACPI

- MaLd0n.aml - Such like AIO method to fix everything including IRQ, PM, RTC, EC, ...
  - [Olarila.com by MaLd0n](https://www.olarila.com/topic/25111-hackintosh-efi-folders-with-opencore-mod/)

### Drivers

- OpenCanopy.efi
- OpenHfsPlus.efi
- OpenRuntime.efi
- ResetNvramEntry.efi

### Kexts

#### Generic kext patcher

> https://github.com/acidanthera/Lilu

- Lilu.kext

#### Improve overall stability and patch CPU name

> https://github.com/acidanthera/RestrictEvents

- RestrictEvents.kext

#### Optimize RaptorLake heterogeneous core configuration

> https://github.com/b00t0x/CpuTopologyRebuild

- CpuTopologyRebuild.kext

#### Apple SMC emulator

> https://github.com/acidanthera/VirtualSMC

- VirtualSMC.kext
- SMCProcessor.kext
- SMCSuperIO.kext

#### Improve NVMe stability

> https://github.com/acidanthera/NVMeFix

- NVMeFix.kext

#### Fix graphics

> https://github.com/acidanthera/WhateverGreen

- WhateverGreen.kext

#### Fix audio ports

> https://github.com/acidanthera/AppleALC

- AppleALC.kext

#### Fix RTL8125 Ethernet

> https://github.com/Mieze/LucyRTL8125Ethernet

- LucyRTL8125Ethernet.kext

#### Fix WiFi/Bluetooth

> https://github.com/OpenIntelWireless

- Airportltlwm.kext
- BlueToolFixup.kext
- IntelBluetoothFirmware.kext
- IntelBTPatcher.kext

#### Sensor for Radeon GPU

> https://github.com/ChefKissInc/RadeonSensor

- RadeonSensor.kext
- SMCRadeonSensor.kext

#### Correct USB port mapping using Hackintool

> https://github.com/benbaker76/Hackintool

- USBPorts.kext

### Tools

- OpenShell.efi

## What works and what doesn't work

### Works

- Everything excepts ...

### Doesn't work

- Apple continuity
  - Currently AX210 WiFi supports on macOS is immature yet
  - Sadly, Fenvi T919 (BCM94360) doesn't work on Sonoma
    - Seems like there is workaround but not stable, [check this post](https://www.reddit.com/r/hackintosh/comments/170q5wu/enable_wifi_in_sonoma_with_fenvi_t919/)

## Refer to this as much as you want

But if you want to post your EFI somewhere else, please leave this repository link as a reference.

Thank you :smile:
