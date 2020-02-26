---
title: "Raspberry Pi 3 Fastboot - Less Than 2 Seconds"
date: "2020-02-26T03:11:09+03:00"
thumbnail: "/img/0042-rpi3-fast-boot-2-saniyede-acilan-sistem.jpg"
categories: ["Gömülü|Embedded"]
tags: ["Embedded Linux", "Raspberry Pi", "Embedded", "English"]
url: "rpi3-fast-boot-less-than-2-seconds"
summary: This post shows how to boot Raspberry Pi 3 (RPI) in 1.75 seconds, in details. In addition to that, it is discussed optimizations that can be applied to a Qt application. At the end, we will have a RPI that boots from power-up to Linux shell in 1.75 seconds, power-up to Qt GUI (QML) application in 2.82 seconds.
---

{{< goTrPost url="/rpi3-fast-boot-2-saniyede-acilan-sistem" >}} <br>

This post tells about my journey on fast-booting a Raspberry Pi 3 (RPI). At the end, we will have a RPI that boots from power-up to Linux shell in 1.75 seconds, power-up to Qt GUI (QML) application in 2.82 seconds.

**Download demo image** : [github.com/furkantokac/rpi3-fastboot-sdcard.img][14]
You can see the details about the demo image on part 6.

{{< youtube eQW0QNUPb2o >}}

>

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

First of all, we should know the target device well since some critical stages of boot optimization process are low-level (hardware dependent). We need to be able to answers questions such as what is the boot sequence of the device, which files are running in which order to boot the device, which files are 100% required etc. Besides that, optimizations should be done and tested one by one, so that the effect can be clearly observed.

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

In this part, the software embedded to the device by the manufacturer runs the bootcode.bin. Because bootcode.bin is a closed-source, we can't configure it directly so there are 2 things we can do. We either try different versions of the bootcode.bin files, or we'll try to change the files that are run by bootcode.bin. (we ignore the reverse-engineering)

We go to RPI's Git page (see [12][12]) and see that there is no different versions of bootcode.bin available. We go to the old Commits of bootcode.bin and try the old versions, we see that there is no change in speed. We can move on to the other option.

Lets check for start.elf. On the RPI’s Git page, we see that there are different versions of start.elf files: start_cd.elf, start_db.elf, start_x.elf. We check the differences of the files and see that start_cd.elf is a simplified version of start.elf. In the start_cd.elf, GPU features are cropped, this may cause problem but lets try it. When we change our start.elf by start_cd.elf, the boot process is 0.5sec faster than before. However, when we run the Qt app, it fails. So why it fails, can we fix it ? Our GUI application runs on OpenGL ES and start_cd.elf does not allocate enough memory for the GPU. Although we have tried to overcome this difficulty, we have not succeeded, but I believe that it can be solved if more time is spend on it.


#### K2 - Linux pre-boot stage

This part is handled by start.elf. Since start.elf is closed-source, we cannot directly work on it, but there are open-source files associated with start.elf: bcm2710-rpi-3-b.dtb, kernel.img

The first thing we can do is to check if any of these files slow down the start.elf. When we remove the Device Tree, Kernel does not boot. There are 2 possibilities here; the problem is either start.elf or Kernel. We need to find a way to test it. An application that can run without Device Tree will do the trick, which is a barebone RPI application. If we make a small application and let start.elf run this application instead of Kernel, we can see if deleting Device Tree creates any speed change. As a second option, we can try to compile U-Boot and run U-Boot instead of Kernel, but the first option is cool. When we write and run the LED blink application (see [13][13]), we see that deleting the Device Tree faster the boot process for 1.0sec. We also try to change the default name of the Device Tree (bcm2710-rpi-3-b.dtb). Acceleration works again. So here is the conclusion: Device Tree is processed by start.elf even if we do not boot the kernel, and start.elf specifically searches for the name "bcm2710-rpi-3-b.dtb". To sum up, either we get rid of the Device Tree or use it by changing its name.

Renaming the Device Tree option can be handled as the following; We can write a barebone software thats gonna be run by start.elf and handle the Kernel booting process by using renamed Device Tree. There will be time loss because we need to run an extra code here. Therefore, lets check the other option, which is cancelling the Device Tree.

We have seen in our test that the Device Tree is absolutely necessary for booting the Kernel and not necessary for start.elf. If the Device Tree is related to the Kernel, we can somehow try to hardcode the Device Tree configurations into the Kernel. When we get detailed information about Device Tree, we see that similar option already exists for the Kernel. (see [3][3] on page 11) When we make the necessary settings (K3 contains information about this setting), we see that the Kernel can boot successfully. Lets test if everything works OK.

After the tests, we observe that;
- Qt application works OK.
- UART stop working.
- We see that the boot time of the Kernel is slower by 0.8sec.

Lets check what is the problem with UART. We save the boot logs of the Kernel where UART has problem. Then, we save the boot logs of the Kernel where UART is running. (By boot logs, I mean "dmesg" command). When we check the differences between the logs, there is a difference in the line starting with "Kernel command line:". In the system where UART is running, "8250.nr_uarts = 1" parameter is passed to the Kernel. We put this parameter in the cmdline.txt file of the problematic Kernel and it works like a charm. Lets move on the other problem.

We should check what slow down the boot process about 1.0sec. We will use the same logs again. When we compare the problematic system's logs and non-problematic system's logs, we see that there is an extra log in the problematic system that contains the word "random", and the delay is there. When we try to close the "random" settings from the Kernel one by one, we find the problematic setting of the Kernel. (K3 has information about this setting) When we turn off the setting, we see that everything is back to normal. Mission completed.

As a result, boot process is faster about 2.0 seconds. The total time spent for K2 is 0.25sec. Our improvements can continue here, like we can optimize the Device Tree, but I think that we can spend the time more efficient by moving to the next step so lets move.


#### K3 - Linux boot stage

We explained some of the Kernel optimization in part K2. This part has a list of Kernel feature that we've played on. To see how a specific feature affects the Kernel booting process, please visit the Git page of the project (see [5][5]) and, do a detailed research online about the setting if it is required.

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

InitSystem does not take a lot of time, but the fastest-running code is non-running code. :) That's why we removed the BusyBox. The only process that is required for us from BusyBox is File System Mounting. We can simply embed this process into the application.

All we need to do is embedding the following code anywhere in the Qt program: <br>
`QProcess::execute("/bin/mount -a");`

Of course, this piece of code it should be put into the right place because this process may take time so we don't want our application is blocked by this process. After that, we put our application in "/sbin/" folder with the name "init" and the process is completed. Kernel automatically runs the "/sbin/init" after it loads the userspace so the first thing that run after userspace is loaded will be our application.


#### K5 - Application

Qt Creator offers detailed debugging tools. Using these tools, we can determine what is slowing down of Qt application starting process.

**Static Compilation** <br>
One of the most important improvement we have done in K5 is static compilation. Static compilation means that all the libraries required by application are contained in its binary file. (see [6][6], [7][7]) When we compile such a Qt application by default settings, the application is compiled dynamically. In dynamic compilation, the application calls the required libraries one by one from the file system and this is a waste of time. Static compilation has no disadvantage in our scenario so it is safe to use. This has allowed us to gain approximately 0.33sec.

**Stripping** <br>
Stripping reduces the file size by stripping unnecessary areas in the binary file. It is a very useful and essential step, especially after static compilation. Stripping can be done with the following command: `strip --strip-all QtApp` After this process, the application size of 21mb decreased to 15mb.

**QML Lazy Load** <br>
This feature does not make a big impact in our case because the GUI of the application we are working on is not very complex, but in large QML files, we can hide our time-taking processes by showing the user some graphical contents like an animation.

**Embedding Source Files in the Application**
Any resources that we add to the project through the .qrc file are embedded in the compiled program. This feature should be default after Qt 9.0. Just try to keep everything in the binary file (like fonts, images etc.).


## 5. More Optimization!

Although there are infinitely different possibilities in this section, there are ones that will have a big impact in an acceptable amount of time. Besides that there are some advices.


**Code**: G1
**Related section**: K1 (start.elf)
**Estimated effect**: 0.5sec
**Available Tools / Methods**: ARM disassembler
**Description**: start_cd.elf can be used instead of start.elf. To do this, it is necessary to reverse engineer the start_cd.elf file to identify the problem. Basically, we first need to understand the structure of start.elf. Then we can hack start_cd.elf to solve the problem.


**Code**: G2
**Related section**: K5 (Qt)
**Estimated effect**: 0.9sec
**Available Tools / Methods**: Cache
**Description**: When a userspace application runs for the first time, it starts slow since it is not cached. After they are cached by the Kernel, they starts much faster. (see [8][8]) The same situation is observed in our Qt application. The difference can be observed by running the Qt application, then close and re-run it. If we can somehow copy the cache and pass it to the Kernel at boot time, our application will start faster. The source code "hibernete.c" (see [10][10]) and "drop_caches.c" (see [11][11]) inside the Kernel source code can be used for this purpose.


**Code**: G3
**Related section**: K3 (kernel.img), K5 (Qt)
**Estimated effect**: 1.0sec
**Available Tools / Methods**: Hibernete
**Explanation**: By hibernete, we can have a major gain for the K3 and K5 stages. To implement hibernate, we need a total control on it since it may cause an unstable system if it is not implemented correctly.


**Code**: G4
**Related section**: K3 (kernel.img), K5 (Qt)
**Estimated effect**: -
**Available Tools / Methods**: initramfs, SquashFS
**Description**: There are 2 partitions on the SD card: Boot Partition and File System Partition (rootfs). Boot Partition contains the files necessary for Raspberry to boot. For this reason, Boot Partition is automatically read by Raspberry. The Kernel actually runs on the RAM without a File System at first. So if we put the whole rootfs in Kernel.img, all the operations after copying Kernel.img to RAM will be faster because it will be on the RAM. This will increase the size of Kernel.img, so the size of Kernel.img should be reduced as much as possible.


**Code**: G5
**Related section**: K3 (kernel.img), K5 (Qt)
**Estimated effect**: -
**Available Tools / Methods**: Btrfs, f2fs
**Description**: These File System types have high read-speed, while they support both reading and writing. For fast boot, read-speed is more important than write-speed. So these File Systems are worth a try.


**Code**: G100
**Explanation**: Debugging, which is the basis of the optimization process, needs to be planned. Before starting the optimization process, you should not hesitate to share time for a debugging plan, collect the necessary materials for debugging, compile, run, automate the development processes.


**Code**: G101
**Explanation**: If noticed, we start optimizations from the lowest level, that is, from Raspberry's self-boot to the highest level, that is, the optimization of the Qt application. This makes debugging difficult, which is the basis of the optimization process. Instead, I think that starting from the highest level may make the debugging easier.


## 6. In A Nutshell..

If you want to just have a fast-boot image for your RPI without going into the details, follow these steps;

1. `git clone https://github.com/furkantokac/buildroot`
2. `cd buildroot`
3. `make ftdev_rpi3_fastboot_defconfig`
4. `make`
5. At this stage, the ready-to-run RPI image will be available in the `buildroot/output/images` folder. You can write it to the SD device and boot RPI directly. When the system is up, the terminal should be displayed directly on the screen.

The image you gonna have has no overclock so you can faster the booting process by overclocking your RPI. Also you can compile your own Qt application statically and replace it with `sbin/init` to make it start automatically on startup. USB drivers are removed so you can not control the RPI by a USB keyboard or mouse.


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
**16.** [BMS Demo Video][16]

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
[16]: https://www.youtube.com/watch?v=eQW0QNUPb2o
