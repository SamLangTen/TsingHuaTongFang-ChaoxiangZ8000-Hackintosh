# Tsing Hua Tongfang Chaoxiang Z8000 Hackintosh
## 电脑配置
|规格|详细信息|
|:-: |:-:|
|电脑型号|清华同方 超翔Z8000-61992 (Q170-4S A420AP0)|
|操作系统|macOS Catalina 10.15.7|
|处理器|英特尔 酷睿 i7 6700|
|内存|8G 2133MHz + 4G 2133MHz|
|硬盘|WD SN550 256G（加装）|
|显卡|Nvidia GT640 戴尔拆机卡（原机附带AMD R7 350x无法驱动）|
|声卡|Realtek ALC887|
|网卡|WTXUP博通BCM94360（加装）|

原版机器不含SSD和网卡，原机附带AMD R7 350x独显但不被支持。因此原机装Hackintosh最简单的方法是将附带的独显移除使用核显。

## 启用核显
该主板在插有独显时，会自动禁用核显，且主板BIOS设置项缺少核显启用的设置，经提取固件可以找到相关设置项，修改方法参见[OpenCore配置教程](https://github.com/acidanthera/OpenCorePkg/raw/master/Docs/Configuration.pdf)中CFG Lock值修改的部分（修改前请注意是否为Q170-4S A420AP0主板，61992型号有不同配置的产品，若主板型号不同请自行使用AMI BIOS Flasher utility提取）：
```
Internal Graphics, VarStoreInfo (VarOffset/VarName): 0x480
 	One Of Option: Auto, Value (8 bit): 0x2 (default) 
	One Of Option: Disabled, Value (8 bit): 0x0
 	One Of Option: Enabled, Value (8 bit): 0x1
```
因此只要通过GRUB UEFI Shell将0x480处的值修改为0x1即可启动核显。在插有独立显卡时，无需修改config.plist即可启用硬件加速，且Hackintool识别正常。

如果未插有独显，主板会自动启用核显，无需对BIOS设置项进行。但需要将该config.plist内Device Properies中有关核显的设置项启用。

## BIOS设置

* Boot->Launch CSM: Disabled
* Advanced->System Configuration->Serial Port: Disabled
* Advanced->CPU Configuration->Intel(R) Virtualization Technology: Enabled

该主板的BIOS设置项缺少如下影响启动的设置项，欲修改请参见[OpenCore配置教程](https://github.com/acidanthera/OpenCorePkg/raw/master/Docs/Configuration.pdf)中CFG Lock值修改的部分：

* CFG Lock。默认为Disabled，应为Disabled。默认设置不影响启动。
* XHCI Hand-off。默认为Disabled，应为Enabled。默认设置会影响启动，应该设置为Enabled或启动OpenCore的ReleaseUSBOwnership功能。该设置项偏移值为0x1B，可取值：0x1(Enabled)或0x0(Disabled)。

* VT-d:。默认为Disabled，应为Disabled。
* Secure Boot。默认为Disabled，应为Disabled。
* Fast Boot。默认为Disabled，应为Disabled。

