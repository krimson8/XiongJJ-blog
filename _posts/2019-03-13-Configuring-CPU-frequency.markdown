---
layout: post
title:  "CPU Frequency Scaling On Linux With cpupower"
date:   2019-03-13 15:30:04 +0800
author: XiongJJ
comments: true
categories: linux
---

CPU frequency scaling enable OS to scale the frequency of the CPU for several purpose, including power saving and for bench-marking purpose. This tutorial shows how to scale the frequency of CPU manually using `cpupower`.

Several notes: 
+   This is tutorial is written using Ubuntu, but also tested in Arch, should works well with other linux distro too.
+   Most of the commands used below requires root permission.
<br>
<br>


## Installing `cpupower`
Enter the command below to install the needed tools
```
$ sudo apt install linux-tools-common linux-tools-generic linux-tools-`uname -r`
```
<br>

## Steps before configuring the frequency
<br>

### Changing your CPU driver
Run the command to attain CPU information:
```shell
$ cpupower frequency-info | grep driver
```
It will show up something like this:
```shell
driver: intel_pstate
```
which indicates that the current driver is called `intel_pstate`. For more information regarding the driver, please refer to the document at [kernel.org](https://www.kernel.org/doc/html/v4.12/admin-guide/pm/intel_pstate.html). This driver provides 2 choices of CPU governor which are `performance` and `powersave`. However, it doesn't have the governor for us user to make customization to the frequency. 

Hence, we have to replace the driver with other alternatives.

First, open the file `/etc/default/grub` and add the phrase `intel_pstate=disable` to the line `GRUB_CMDLINE_LINUX_DEFAULT`. After adding the phrase it should looks like 
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash intel_pstate=disable"
```

Then, execute the command:
```
$ update-grub
```

Upon the next boot, check again the driver in use. If it shows `acpi-cpufreq` as result then we have successfully replaced the driver. To switch it back simply remove the phrase `intel_pstate=disable` from the line and run `update-grub` again.
<br>
<br>
### Changing CPU governor
Run the command:
```
$ cpupower -c 0 frequency-info
```
The result are shown as below:
```
analyzing CPU 0:
  driver: acpi-cpufreq
  CPUs which run at the same hardware frequency: 0
  CPUs which need to have their frequency coordinated by software: 0
  maximum transition latency: 10.0 us
  hardware limits: 400 MHz - 1.80 GHz
  available frequency steps:  1.80 GHz, 1.80 GHz, 1.70 GHz, 1.60 GHz, 1.50 GHz, 1.40 GHz, 1.30 GHz, 1.20 GHz, 1.10 GHz, 1000 MHz, 900 MHz, 800 MHz, 700 MHz, 600 MHz, 500 MHz, 400 MHz
  available cpufreq governors: userspace performance schedutil
  current policy: frequency should be within 400 MHz and 1.80 GHz.
                  The governor "userspace" may decide which speed to use
                  within this range.
  current CPU frequency: Unable to call hardware
  current CPU frequency: 1.40 GHz (asserted by call to kernel)
  boost state support:
    Supported: yes
    Active: yes
```

Within the options at `available cpufreq governors`, we are going to use the governor `userspace` which allows us to do modification. If you don't have `userspace` in your list then run the command
```
modprobe cpufreq_userspace
```

To choose `userspace` as the CPU governor and scale the frequency, run the following commands:
```
$ sudo cpupower frequency-set -g userspace
$ sudo cpupower frequency-set -f 1400MHz
```
The input unit for the frequency can be Hz, MHz and GHz.

Your CPU should now running at your desired speed.
```
$ grep MHz /proc/cpuinfo
cpu MHz		: 1400.082
cpu MHz		: 1400.100
cpu MHz		: 1400.009
cpu MHz		: 1400.000
cpu MHz		: 1400.026
cpu MHz		: 1400.123
cpu MHz		: 1400.026
cpu MHz		: 1400.002
```

