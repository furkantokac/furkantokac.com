---
title: "Raspberry Pi 3 Fastboot - Less Than 2 Seconds"
date: "2059-09-15T20:38:10+03:00"
thumbnail: "/img/0042-rpi3-fast-boot-2-saniyede-acilan-sistem.jpg"
categories: ["Gömülü|Embedded"]
tags: ["Embedded Linux", "Raspberry Pi", "Embedded"]
url: "rpi3-fast-boot-less-than-2-seconds"
summary: This post shows how to boot Raspberry Pi 3 (RPI) in 1.75 seconds, in details. In addition to that, we'll discuss optimizations that can be applied to Qt application. At the end, we will have a RPI that boots from power-up to Linux shell in 1.75 seconds, power-up to Qt GUI (QML) application in 2.82 seconds.
---

{{< goTrPost url="/rpi3-fast-boot-2-saniyede-acilan-sistem" >}} <br>

This post tells about the journey to fast-booting a Raspberry Pi 3 (RPI). At the end, we will have a RPI that boots from power-up to Linux shell in 1.75 seconds, power-up to Qt GUI (QML) application in 2.82 seconds.

You can download and test the demo image : [ftDev / RPI3 Demo Image][14]


## Outline

**1.** Introduction <br>
**2.** Project Requirements <br>
**3.** Raspberry Boot Files <br>
**4.** Raspberry Boot Optimization <br>
&emsp;&emsp;**K1 -** Raspberry boot stage <br>
&emsp;&emsp;**K2 -** Linux pre-boot stage <br>
&emsp;&emsp;**K3 -** Linux boot stage <br>
&emsp;&emsp;**K4 -** Init system <br>
&emsp;&emsp;**K5 -** Application <br>
**5.** More Optimization! <br>
**6.** In A Nutshell.. <br>
**7.** Result <br>
**8.** References <br>


## 1. Introduction

First of all, we should know the target device well since some critical stages of boot optimization process are low-level (hardware dependent). We need to be able to answers questions such as what is the boot sequence of the device, which files are running in which order to boot the device, which files are 100% required etc. Besides that, optimizations should be done and tested one by one, so that the optimizations' effect can be clearly observed.

The boot process of the RPI is kind of different than the other, traditional devices. RPI's boot process is based on GPU rather than CPU. I recommend that you dig into more on this topic on the internet. (see [1][1], [9][9])

RPI uses Qualcomm's closed-source chip as System on Chip (SoC). Therefore, SoC-related softwares are provided to us as binary by RPI. (see [2][2]) So we cannot customize them without reverse-engineering. That is why the most difficult parts of the RPI boot optimization process are SoC related parts.


## 2. Project Requirements

- RPI shall be used as a device.
- Buildroot shall be used for Linux customization.
- RPI's GPIO, UART shall be usable.
- GPIO, UART shall be usable on Qt.
- Qt (QML) application shall automatically be started.


## 3. Raspberry Boot Files

The files related to RPI's boot process and their purposes are briefly as the following;

* **bootcode.bin**: This is the 2nd Stage Bootloder file that is run by the 1st Stage Bootloader that is embedded on the RPI by the manufacturer. Runs on the GPU. Activates the RAM. Its purpose is to run start.elf, the 3rd Stage Bootloader, correctly.
* **config.txt**: This is the file that contains the GPU settings. Used by start.elf.
* **cmdline.txt**: Contains the parameters that will be passed to the kernel when executing it. Used by start.elf.
* **.dtb**: The compiled Device Tree file. It contains the descriptions of the hardwares of the device. It is used by start.elf and kernel.img.
* **start.elf**: This is the 3rd Stage Bootloader file run by bootcode.bin. It contains the GPU driver. Its purpose is to split RAM between the GPU and the CPU, apply the settings inside the config.txt file to the GPU, make the necessary adjustments by reading the corresponding .dtb file, and run the kernel.img with the parameters in the cmdline.txt file. After performing these operations, it'll keep running on the device as a GPU driver till the device is turned off.
* **kernel.img**: This is our kernel file. Run by start.elf. We have a full control on kernel.img.
* **Basic logic**: power-up the RPI -> embedded software inside RPI is run -> bootcode.bin is run -> start.elf is run -> read config.txt -> read .dtb -> read cmdline.txt -> kernel.img is running


## 4. Raspberry Boot Optimization

RPI boot process from power-up to Qt application is as the following; <br>
**K1** - Raspberry boot stage (1st & 2nd Stage Bootloader) (bootcode.bin) <br>
**K2** - Linux pre-boot stage (3rd Stage Bootloader) (start.elf, bcm2710-rpi-3-b.dtb) <br>
**K3** - Linux boot stage (kernel.img) <br>
**K4** - Init system (BusyBox) <br>
**K5** - Application (Qt QML)


#### K1 - Raspberry boot stage

In this part, the software embedded on the device by the manufacturer runs the bootcode.bin. Because bootcode.bin is a closed-source, we can't configure it directly so there are 2 things we can do. We either try different versions of the bootcode.bin files, or we'll try to change the files that are run by bootcode.bin. (we ignore the reverse-engineering).

We go to RPI's Git page (see [12][12]) and see that there is no different versions of bootcode.bin available. We go to the old Commits of bootcode.bin and try the old versions, we see that there is no change in speed. We can move on to the other option.

We can work on start.elf because it might have an effect on bootcode.bin. On the RPI’s Git page, we see that there are different versions of start.elf files: start_cd.elf, start_db.elf, start_x.elf. We check the differences of the files and see that start_cd.elf is a simplified version of start.elf. In the start_cd.elf, GPU features are cropped, this may cause problem but lets try it. When we change our start.elf by start_cd.elf and observe the difference, we can see boot process is 0.5sec faster than before. However, when we run the Qt app, it fails. So why it fails, can we fix it ? Our GUI application runs on OpenGL ES and start_cd.elf does not allocate enough memory for the GPU. Although we have tried to overcome this difficulty, we have not succeeded, but I believe that it can be solved if more time is spend on it.


#### K2 - Linux pre-boot stage

This part is handled by start.elf. Since start.elf is closed-source, we cannot directly work on it, but there are open-source files associated with start.elf: bcm2710-rpi-3-b.dtb, kernel.img

The first thing we can do is to check if any of these files slow down start.elf. When we remove the Device Tree, Kernel does not boot. There are 2 possibilities here; the problem is either start.elf or Kernel. We need to find a way to test it. An application that can run without Device Tree will do the trick, which is a barebone RPI application. If we make a small application and let start.elf run this application instead of Kernel, we can see if deleting Device Tree creates any speed change. As a second option, we can try to compile U-Boot and run U-Boot instead of Kernel, but we will implement the first option. When we write and run the LED burning application (see [13][13]), we see that deleting the Device Tree gave us 1.0sec. We also try to change the default name of the Device Tree (bcm2710-rpi-3-b.dtb). Acceleration works again. Here we make the following inferences; 1. Device Tree is processed by start.elf even if we do not boot the kernel. 2. By default, start.elf searches directly for the file “bcm2710-rpi-3-b.dtb”. As a result, in order to improve, we need to destroy the Device Tree file or use the changed name.

The first method to rename the Device Tree file is as follows; We can write software to run by start.elf. This software is a kind of software that can boot the Kernel with Device Tree instead of lighting a LED. start.elf runs the software we have written, and the software we have booted the Kernel with the renamed Device Tree file. There will be a slight loss of speed because we need to run code here. Therefore, before starting this method, it is better to review the other method and choose between.

The other way is to somehow cancel the Device Tree. We have seen in our test that the Device Tree is absolutely necessary for booting the Kernel and not necessarily for start.elf. If the Device Tree is related to the Kernel, we can somehow try to specify the Device Tree configurations in the Kernel as hardcoded. So we can embed the Device Tree in the Kernel. When we get detailed information about Device Tree, we see that such an option already exists for Kernel. (see [3][3] on page 11) When we make the necessary settings (K3 contains information about this setting), we see that the Kernel can boot. Now we need to do a situation analysis and see if everything's okay.

The situation after the tests is as follows; Reviews
- Qt application works without problems. Reviews
- When we tested the UART, it wasn't working. Reviews
- We see that the boot time of the kernel is extended by 0.8sec.

First, we need to find out what the UART problem is. We boot up a Kernel where UART is running trouble-free and save the boot logs. Then we boot and log our Kernel where UART is troubled and embedded in Device Tree. (These logs can be accessed with the command mes dmesg).) We examine the differences. (see [4][4]) Especially in the line starting with “Kernel command line:,, we see a difference in the system where UART is running and not in the system that is not running. In the system where UART is running, ”8250.nr_uarts = 1 parametre parameter is passed to Kernel. We put this parameter in the cmdline.txt file, we see that UART is now working when we boot the system where UART is not working. We solved this problem.

Our other work is to investigate the cause of the Kernel's boot time to increase by about 1.0sec. We will use the logs again. When we look at the logs, we see that there is a log that contains the word “random olan, which is not in the normal working system but in the system we developed, and the delay is there. When we try to close the "random" settings from the kernel one by one, we find the troublesome setting. (K3 has information about this setting) When we turn off the setting, we see that everything is back to normal. We solved this problem.

As a result, we achieved a profit of approximately 2.0 seconds thanks to our strategy. The total time spent for K2 was 0.25sec. Our improvements can continue here (in the simplest Device Tree can be optimized), but we'll spend more time in places where we have bigger troubles, and we'll have more time to go. So we go to the next step.


#### K3 - Linux boot stage

We explained some of the Kernel optimization in part K2. This part have the kernel settings that we've changed as a list. To see how the features affect the kernel, please visit the Git page of the project (see [5][5]) and, if necessary, do detailed research online about the feature.

**Enabled Features**
```
ARM_APPENDED_DTB
ARM_ATAG_DTB_COMPAT
```

**Disable Features**
```
NET
SOUND
HW_RANDOM
ALLOW_DEV_COREDUMP
STRICT_KERNEL_RWX
STRICT_MODULE_RWX
NAMESPACES
FTRACE
USB_SUPPORT
BLK_DEBUG_FS
DEBUG_BUGVERBOSE
DEBUG_FS
DEBUG_MEMORY_INIT
SLUB_DEBUG
PRINTK
BUG
DM_DELAY
ELF_CORE
KGDB
PRINT_QUOTA_WARNING
AUTOFS4_FS
MEDIA_DIGITAL_TV_SUPPORT
MEDIA_ANALOG_TV_SUPPORT
MEDIA_CAMERA_SUPPORT
MEDIA_RADIO_SUPPORT
INPUT_MOUSEDEV
INPUT_JOYDEV
INPUT_JOYSTICK
INPUT_TABLET
INPUT_TOUCHSCREEN
IIO
RC_CORE
HID_LOGITECH
HID_MAGICMOUSE
HID_A4TECH
HID_ACRUX
HID_APPLE
HID_ASUS
```


#### K4 - Init system

InitSystem doesn't take a lot of time, but the fastest-running code is non-running code. :) That's why we removed the BusyBox. The only process that is required for us from BusyBox is File System Mounting. We simply embedded this process into the application.

All we need to do is embedding the following code anywhere in the Qt program: <br>
`QProcess::execute("/bin/mount -a");`

Of course, putting it into the right place is important because this process may take time so we don't want our application is blocked by this process. After that, we put our application in “/sbin/” folder with the name “init" and the process is complete. Kernel automatically runs the "/sbin/init" after it loads the userspace so the first thing that run after userspace is loaded will be our application.


#### K5 - Application

Qt Creator offers detailed debugging tools. Using these tools, we can determine what is slowing down of Qt application starting process.

**Static Compilation** <br>
One of the most important improvements we have made in K5 is static compilation. Static compilation means that all the libraries required by application are contained in its binary file. (see [6][6], [7][7]) When we compile Qt normally, the application is compiled dynamically. In dynamic compilation, the application calls the required libraries one by one from the file system and this is a waste of time. Static compilation has no disadvantage in our scenario so it is safe to use. This development has allowed us to gain approximately 0.33sec.

**Stripping** <br>
Stripping reduces the file size by stripping unnecessary areas in the binary file. It is a very useful and essential step, especially after static compilation. Stripping can be done with the following command: `strip --strip-all QtAppam` After this process, the application size of 21mb decreased to 15mb.

**QML Lazy Load** <br>
This feature does not make a difference because the GUI of the application we are working on is not very complex, but in large QML files, we can hide our time-taking processes by showing the user some graphical contents like an animation.

**Embedding Source Files in the Application** <br>
Any resources that we add to the project through the .qrc file are embedded in the compiled program. This feature should be default after Qt version 9.0. Just try to keep everything the application needed in the binary file (like fonts, images etc), so this will faster the booting progress.


## 5. More Optimization!

Although there are infinitely different possibilities in this section, there are ones that will have a big impact in an acceptable amount of time. Besides that there are some advices.


**Code**: G1 <br>
**Related section**: K1 (start.elf) <br>
**Estimated effect**: 0.5sec <br>
**Available Tools / Methods**: ARM disassembler
**Description**: start_cd.elf can be used instead of start.elf. To do this, it is necessary to reverse engineer the start_cd.elf file to fully identify the problem. Basically, we first need to understand the structure of start.elf. The parts that are in start.elf, not in start_cd.elf, need to be identified and added to start_cd.elf.


**Code**: G2 <br>
**Related section**: K5 (Qt) <br>
**Estimated effect**: 0.9sec <br>
**Available Tools / Methods**: Cache <br>
**Description**: Userspace applications open slowly when first opened. Because they are cached by the Kernel the next time they boot up, they open quickly. (see [8][8]) The same situation is observed in our Qt application. The difference can be observed when you open the Qt application, then close and reopen it. If we can somehow copy the Cache of the cached application and pass them to the Kernel at boot time, our application will open quickly from the beginning. The source code “hibernete.c” (see [10][10]) and “drop_caches.c içindeki (see [11][11]) inside the kernel source code can be used for this purpose. In particular, I think that the Hibernete transaction should cover our transaction.


**Code**: G3 <br>
**Related section**: K3 (kernel.img), K5 (Qt) <br>
**Estimated effect**: 1.0sec <br>
**Available Tools / Methods**: Hibernete <br>
**Explanation**: Hibernete operation is the event that the system shuts down the system completely by copying the current temporary information (RAM) to the hard memory (it becomes the SD device for us). This can be a major gain in the K3 and K5 stages, as it ensures that many operations can be skipped without the need to do so as well as that the Cache is not lost. The K3 and K5 steps, which are now 1.32sec, are worth considering as they are one of the most time consuming parts of the boot process. We need to know exactly what this process is doing in the background, and we should be able to control it completely, because unconsciously, there may be a possibility of stability. For example, one of the measures that can be taken is to turn the system off and on completely at certain time intervals. This resets RAM and Cache. In addition, the Kernel already offers the Hibernete feature, but it is not available when you try to activate it directly.


**Code**: G4 <br>
**Related section**: K3 (kernel.img), K5 (Qt) <br>
**Estimated effect**: - <br>
**Available Tools / Methods**: initramfs, SquashFS <br>
**Description**: There are 2 Partitions on the SD device. Boot Partition and File System Partition (rootfs). Boot Partition contains the files necessary for Raspberry to boot. For this reason, Boot Partition is automatically read by Raspberry and the necessary files are executed automatically. It actually runs on Kernel, RAM without File System. We can try to use this situation briefly. If we put the whole system in Kernel.img, all the operations after copying Kernel.img to RAM will be faster because it will be on RAM. This will increase the size of Kernel.img, so the size of Kernel.img should be reduced as much as possible so that copying to RAM takes little time. One of the effects of this development is that our Qt application will run in a Read-Only environment if it runs in RAM. Of course, this problem can be overcome if necessary.


**Code**: G5 <br>
**Related section**: K3 (kernel.img), K5 (Qt) <br>
**Estimated effect**: - <br>
**Available Tools / Methods**: Btrfs, f2fs <br>
**Description**: These File System types are File System types with high read speed, support for read and write. For fast boot, read speed is more important than write speed. So these File Systems are worth a try.


**Code**: G100 <br>
**Explanation**: Debugging, which is the basis of the optimization process, needs to be more planned. Before starting the optimization process, you should not hesitate to prepare a debugging plan, collect the necessary materials for debugging, compile, run, automate the development processes and determine the strategy.


**Code**: G101 <br>
**Explanation**: If noticed, we have moved from optimization to the lowest level, that is, from Raspberry's self-boot to high level, that is, to the optimization of the Qt application. This makes debugging difficult, which is the basis of the optimization process. Instead, I think that optimization can be improved if Qt is started.


## 6. In A Nutshell..

If you want to just have a fast-boot image for your RPI without going into the details, follow these steps;

1. `git clone https://github.com/furkantokac/buildroot`
2. `cd buildroot`
3. `make ftdev_rpi3_fastboot_defconfig`
4. `make`
5. At this stage, the ready-to-run RPI image will be available in the `buildroot/output/images` folder. You can write it to the SD device and boot RPI directly. When the system is up, the Qt application should be displayed directly on the screen.


## 7. Result

"Normal" section has the measurements of the default Buildroot image. Only the boot delay time is reduced to 0.

"ftDev" section has the measurements of the optimized image.

|           |**K1** |**K2** |**K3** |**K4** |**K5** |**Toplam** |
|---:       |---    |---    |---    |---    |---    |---        |
|**Normal** |1.25sn |2.12sn |5.12sn |0.12sn |1.56sn |10.17sn    |
|**ftDev**  |1.25sn |0.25sn |0.25sn |0.00sn |1.07sn |2.82sn     |

Note: Measurements are done by recording the boot process with high-speed camera. Therefore, they are accurate.


## 8. References

**1.** [How the Raspberry Pi boots up][1]
**2.** [Raspberry / Firmware][2]
**3.** [Device Tree For Dummies][3]
**4.** [Mergely : Compare 2 Texts][4]
**5.** [ftDev / Buildroot Kernel Doc][5]
**6.** [RaspberryPi2EGLFS][6]
**7.** [Linking to Static Builds of Qt][7]
**8.** [Linux System Administrators Guide / Memory Management / The buffer cache][8]
**9.** [Raspberry Pi Boot][9]
**10.** [Raspberry / Linux / kernel / power / hibernate.c][10]
**11.** [Raspberry / Linux / Bootp][11]
**12.** [Raspberry / Firmware / Boot][12]
**13.** [ftDev / Raspberry Pi 3 Baremetal][13]
**14.** [ftDev / RPI3 Demo Image][14]
**15.** [ftDev / Buildroot][15]

[1]: https://thekandyancode.wordpress.com/2013/09/21/how-the-raspberry-pi-boots-up/ 
[2]: https://github.com/raspberrypi/firmware 
[3]: https://bootlin.com/pub/conferences/2014/elc/petazzoni-device-tree-dummies/petazzoni-device-tree-dummies.pdf
[4]: http://www.mergely.com/editor
[5]: https://github.com/furkantokac/buildroot/blob/ftdev/board/ftdev/rpi3/docs/distro_optimization/fcond04/README.config
[6]: https://wiki.qt.io/RaspberryPi2EGLFS 
[7]: http://doc.qt.io/QtForDeviceCreation/qtee-static-linking.html 
[8]: https://www.tldp.org/LDP/sag/html/buffer-cache.html 
[9]: http://exileinparadise.com/raspberry_pi_boot 
[10]: https://github.com/raspberrypi/linux/blob/46d8169547b49308c459707ab45b18339ff392a2/kernel/power/hibernate.c
[11]: https://github.com/raspberrypi/linux/blob/3667ae0605bfbed9e25bd48365457632cf660d78/fs/drop_caches.c
[12]: https://github.com/raspberrypi/firmware/tree/master/boot
[13]: https://github.com/furkantokac/raspberrypi3-tutorials
[14]: https://github.com/furkantokac/buildroot/releases/download/v1.0/rpi3-fastboot-sdcard.img
[15]: https://github.com/furkantokac/buildroot
