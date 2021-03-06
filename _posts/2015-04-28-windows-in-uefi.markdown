---
layout: post
title: "UEFI 环境下的 Windows"
author: vjudge1
date:  2015-04-28 20:23:38
categories: 系统维护
tags: Windows UEFI
---

* content
{:toc}

*以下内容很长……*

*既然是 Linux 协会，为什么还讨论 Windows 的问题呢？*

*毕竟多数人还是对 Windows 的需求更大一些，而且愿意学习如何重装 Windows。本文就是为了使这些人与时俱进，掌握更高水平的技能。在装系统的问题上面，有些原理是相通的。如果基础扎实，读完本文在两种环境下装 Linux 也不会有太大问题。*

*如果你能坚持到最后，你会发现，文章已经考虑到 Linux 的问题了。*

本文主要讨论的是 UEFI 和 Legacy 两种环境。

本文要求你有装系统的经历，否则阅读起来可能会有一些困难。此外，本文是由实践总结出来的经验，某些地方并不一定完全准确。





## 问题的由来

过去装系统是件很容易的事儿，直到某一天，有一个名叫 Windows 8 的操作系统普及开来，问题就来了。有了 Windows 8，很多笔记本已经采用了全新的 UEFI 启动模式。由于新启动模式和传统模式有很大的不同，而且目前两种模式还同时广泛存在，所以需要了解新旧模式和区别，才能顺利地在不同电脑上安装系统。

## UEFI 和 Legacy

过去的电脑用的是 [BIOS](https://zh.wikipedia.org/wiki/BIOS)。BIOS 名为“基本输入输出系统”。过去的电脑在开机时会首先加载 BIOS 程序，进行开机自检，加载引导程序并引导操作系统。

由于 BIOS 年代久远，难以适应计算机硬件的发展 (例如寻址能力只有 16 位) 。而且基于 BIOS 的扩展程序也难以开发，因此诞生了新的规范——EFI。2002 年 EFI 由英特尔开发，2005 年交给 UEFI 论坛发展并更名为 [UEFI](https://zh.wikipedia.org/wiki/%E7%B5%B1%E4%B8%80%E5%8F%AF%E5%BB%B6%E4%BC%B8%E9%9F%8C%E9%AB%94%E4%BB%8B%E9%9D%A2)。

UEFI 名为“统一可扩展固件接口”。它相当于一个小型操作系统。有些电脑内置 EFI Shell，这样可以在没有任何工具的前提下进行简单的系统维护和修复。

### 经典 (Legacy) 的启动过程

* 开机的时候，首先要读取 `0 扇区`的信息。0 扇区只有 512 字节大小，其中有一段程序，使BIOS可以定位到活动分区。
* 寻找`活动分区`。读取分区信息。
* 按照分区内的代码，引导合适的`启动器`。
* 由启动器引导操作系统。

DOS 及基于 DOS 的 Windows 系统 (一直到 Windows Me) 不通过专门的启动管理程序。因此在装完 Windows NT 之后再装这些系统，会发现 NT 因引导被改写而无法启动。

Windows NT 系列 (从 NT 3.5 到 2003) 通过名为 `NTLDR` 的引导器引导。配置文件为 Boot.ini。

新系统 (从 Vista 开始) 通过名为 `Windows Boot Manager` 的引导器来引导。配置文件名为 BCD。相当于 Windows 注册表的格式，是个二进制文件，可用 bcdedit 命令编辑。此引导器可以直接引导 WIM 格式的映像。

### UEFI 模式的启动过程

目前笔记本的 UEFI 都是 64 位 UEFI。在某些平板电脑上是 32 位的。

开机的时候，UEFI 会在启动磁盘 (硬盘、光盘、U 盘等) 寻找 `EFI\Boot\bootx64.efi`。如果找到该文件，会执行该文件。接下来该文件会寻找和引导启动器，从而引导操作系统。

由于这个原因。在 UEFI 模式下使用 U 盘安装系统，安装盘制作过程非常简单——只要找到安装盘，直接往 U 盘根目录解压就可以了。

需要注意的是，对于目前的大多数笔记本而言，并不能在 UEFI 模式下安装 32 位操作系统。但某些平板型电脑可以。

### Secure Boot

Secure Boot 是 UEFI 支持的一个功能。如果该功能开启，在引导时，固件会验证启动程序的签名。只有启动器具有合法签名，固件才会继续引导，否则会拒绝引导。

此功能在 Windows 8 诞生之后被普及。诞生之后，该功能受到了极大的争议，因为一般电脑只有 Windows 8 的签名，这样的话，用户无法在保留 Windows 8 的前提下安装其他操作系统 (甚至包括 Windows 7。但实际上是可以关闭的，因此没有太大影响) 。

按照微软的规定，其他安装 Windows 的设备 (如 Windows RT 或手机) 禁止关闭此功能。因此这些设备基本上无法安装其他系统。

最近，微软规定“预装 Windows 10的电脑必须开启 Secure Boot。至于能否关闭，由 OEM 厂商决定”。因此，目前还无法准确判断未来能否顺利地更换系统。

目前笔记本电脑的 Secure Boot 可以关闭，而且关闭之后对 Windows 8/8.1没有影响。如果关闭之后进入系统，屏幕右下角出现“Secure Boot 未正确配置”，可安装最新的系统补丁。

部分笔记本电脑无法直接关闭 Secure Boot。这种情况下，可尝试给 BIOS 设置一个密码。

### UEFI 与 Legacy 的切换
目前，不太旧的笔记本电脑已经全部采用 EFI 模式。不过，有些笔记本并不能对模式进行更改 (没有切换的选项，但是确实是 EFI) 。

生产 BIOS 的厂家并不多，但是由于各电脑品牌会进行一定程度的定制，所以接下来的操作是思路相同、细节不同。

进入 BIOS 设置，找到 `Boot` 页面。

* 如果看到类似于 `Boot Mode`、`UEFI Boot Option` 的选项，说明可以切换这两种启动模式。
* 有些电脑带有类似于 `UEFI Priority Order` 的选项，内容为 `UEFI First` 或 `Legacy First`。这也是改变启动模式的方法。它的作用是调整优先级。
* 还有些电脑可能没有“Boot Mode”，但是有类似于 `CSM Support` 的选项。开启此选项，相当于支持 Legacy 模式 (但仍然是 UEFI 优先) 。
* 有些电脑没有类似选项，那么不外乎两种情况：
  1. 电脑根本不支持 EFI。
  2. 电脑支持 EFI，并且优先从 EFI 启动。也就是说，不管启动盘本身有没有引导信息，只要有 EFI 的引导程序，电脑就会通过 EFI 引导系统。

证明方法也很简单，找一个干净的、没有引导信息的 U 盘，把 Windows 8 安装盘 (或者小一点的系统，如 64 位 Ubuntu 14.04) 直接解压进去。如果能够直接引导，说明支持 EFI。

### 预装 Windows 8 的电脑进入 BIOS 设置
有些电脑开机时并不会显示类似“Press F2 to setup”的提示，甚至按下F2等按键也不会进入BIOS设置，而是直接进入Windows 8。可以按照以下步骤来进入BIOS设置：
1. 进入Windows 8
2. `按住 Shift 键`，重新启动计算机
3. 之后会进入诊断画面。选择`疑难解答`、`高级设置`、`UEFI固件设置`
4. 屏幕会提示重启，重启之后即进入 BIOS 设置。

## GPT 和 MBR

本文只在“外观”上探讨二者的区别，不研究二者的具体结构。由于几乎没有谁会在自己笔记本电脑上搞 SCSI、RAID 或者动态分区，所以本文不讨论这些特殊情况。

### GPT 的优势

* GPT 支持容量大于 2TB 的分区。
* GPT 支持大于 4 个主分区。而 MBR 磁盘只支持最多 4 个主分区。

### MBR 磁盘的外观

一般情况，人们习惯上把一个 MBR 磁盘划分为一个 `主分区` (C: 或者叫 /dev/sda1) 和一个 `扩展分区` (/dev/sda2) 。其他分区都是被包在扩展分区之内的 (从 /dev/sda5 开始) 。

![mbr](/images/mbr.png)

### GPT 磁盘的外观

GPT 磁盘的分区全部是主分区。一般 GPT 磁盘前面有一个数百 MB 的采用 FAT32 文件系统的分区，即 ESP 分区。

![gpt1](/images/gpt1.png)

### 品牌机的 GPT 磁盘

一些品牌机有恢复功能，因此会多数一些分区来保存“恢复程序”和/或原始系统镜像。并且这些分区在一般情况下是不能被直接分配盘符的。

![gpt2](/images/gpt2.png)

## Windows 对两种模式的支持

### 旧系统 (2003及以下) 对 EFI/GPT 的支持

Windows XP 及更低版本系统只支持 Legacy+MBR。无法读取 GPT 磁盘。
Windows 2003 也不支持 UEFI+GPT，但是可以读取 GPT 磁盘。不过无法从 GPT 磁盘启动。

因此，很多 PE 工具箱是用 Windows 2003 而不是 XP 制作的。

### 新系统 (Vista 及以上) 对 EFI/GPT 的支持

UEFI & Legacy、GPT & MBR 是可以自由组合的。

32 位的 Windows 不支持 64 位 UEFI 安装，也就是说，在我们的笔记本电脑上只能通过 Legacy+MBR 启动。 (实际上是 UEFI 也是可以启动的) 。

在某些平板电脑上，由于 UEFI 是 32 位的，这时 64 位系统反倒无法安装。

64 位的 Windows Vista 或更高版本只支持以下两种组合：

1. UEFI + GPT
2. Legacy + MBR

错误的组合会使 Windows 无法启动。

后面只讨论 64 位新版 Windows 的安装。

Windows 安装程序也有 EFI 和 Legacy 两种启动方式。在安装的时候应该根据硬盘的实际情况来选择合适的启动方式。否则，在选择分区的时候会发现，无论选择哪个分区 (甚至格式化) ，都无法安装，并且会看到类似于下面的错误提示：

1. Windows 无法安装到这个分区上。选中的分区具有 MBR 分区表。在 EFI 系统上，Windows 只能安装到 GPT 分区上。
2. Windows 无法安装到这个分区上。选中的分区具有 GPT 分区表。

遇到此情况，只需重启，并选择正确的启动方式即可解决。

对于全新的电脑，也可以考虑采用分区工具 (或系统自带的 diskpart) 重建分区表。

### 无损切换

**注意，操作有风险！请不要在存有重要数据的计算机上试验。**

如果因为某些原因，需要对 GPT/MBR 进行转换，可以按照以下思路来进行。

在操作之前，需要制作一个 Windows PE 启动盘。可以采用网上流行的大白菜、老毛桃等制作工具。

(1) UEFI + GPT 转换成 Legacy + MBR

* 使用 DiskGenius 或傲梅分区助手可以对磁盘进行无损转换。注意！由于 MBR 分区表只支持最多四个主分区。因此在转换时可能会丢失分区。
* 将 Windows 系统所在分区设置为活动分区。
* 修复引导。可采用
  
{% highlight text %}
bcdboot C:\Windows /l zh-CN
{% endhighlight %}

* 在 BIOS 选项中将启动模式改成 Legacy，或者改成支持 Legacy 的选项。

(2) Legacy + MBR 转换成 UEFI + GPT

* 使用 DiskGenius 或傲梅分区助手可以对磁盘进行无损转换。
* 在分区开头建立一个 200MB 左右和一个 128MB 的分区。
那个 200MB 左右的分区是将来的 ESP 分区，必须是 FAT32 文件系统。而且要分配个盘符 (假如是X:) ，后续操作要用。
* 进入“命令提示符”，输入：

{% highlight text %}
diskpart
list disk
sel disk 0 (0表示系统所在硬盘。应根据list的实际结果来选择) 
list part
sel part 0 (0表示准备作为ESP的分区。需要根据实际情况来修改。) 
set id={C12A7328-F81F-11D2-BA4B-00A0C93EC93B}
sel part 1 (1表示那个128MB分区。需要根据实际情况来修改。) 
set id={E3C9E316-0B5C-4DB8-817D-F92DF00215AE}
{% endhighlight %}
  
* 修复引导。输入：

{% highlight text %}
bcdboot C:\Windows /s X: /f uefi /l zh-cn
{% endhighlight %}

其中X:是ESP分区的盘符。

* 在 BIOS 选项中将启动模式改成 UEFI。

## 安装 Windows 的注意事项

1. Windows 7 可以通过 EFI 启动和安装，但是不建议你这样做。使用花钱买来的正版可以无视此条。
2. 如果要组建双系统或多系统，而且其中有一个是 Windows 8 或更高的系统。这样的话最好关闭“快速启动”功能，否则其他系统无法正常访问硬盘。
3. 如果安装 Windows 时出现有关 GPT/MBR 的问题，需要 Legacy 启动，但笔记本电脑无法切换 EFI/Legacy 时，可以将启动盘的 `EFI` 文件夹删除或更名。

## Linux 对两种模式的支持

### Linux 对两种模式的支持

Linux 系统支持 GPT 和 MBR 磁盘。而它对 UEFI 的支持程度基本上由启动器决定。在个人电脑上，Linux 系统的启动器是可以更换的 (嵌入式系统请无视本节) 。也就是说，只要有合适的启动器，并设置正确的内核启动参数，引导系统就不会有太大问题。

比较古老的系统采用 LILO 或 GRUB，这两种不支持 UEFI 启动。很多 PE 工具箱的启动菜单是基于 GRUB 制作的，同样不支持 UEFI 启动。

幸运地是，目前绝大多数发行版已经采用 GRUB2 (系统安装盘用 syslinux) 。这两种启动器都支持 UEFI 启动。但是，和 Windows 一样，也得是 64 位系统。少数发行版 (如 Ubuntu) 由于购买了 Secure Boot 的证书，因此甚至支持在 Secure Boot 开启的情况下引导。

因此现在 Linux 的启动器引导 Linux 并没有什么问题，甚至不需要更改引导参数。

但是，由于 GRUB2 引导其他操作系统 (例如 Windows) 是通过`链式引导`来完成的。所以要根据具体情况 (UEFI/Legacy) 来调整引导参数。

如果需要更换模式，只需用对应的引导方式重新安装一遍 GRUB2。并不需要重装系统，也不需要更换内核参数。

在 UEFI 下模式安装某些 Linux 发行版 (如 Ubuntu) ，除了要挂载 `/boot` (实际上可以和主分区合并) ，还要挂载 ESP 分区到要求的某一目录，如 `/boot/efi`。

### Linux 与 Windows 共存

与 Windows 不同，UEFI/Legacy + GPT/MBR 的四种组合对于 Linux 来说都可以引导。所以，在不希望抛弃 Windows 的情况下，应该按照 Windows 的引导方式来安装 Linux。

(1) GRUB2 引导 Windows

GRUB2 可以通过链式引导来引导进入 Windows。但是 UEFI 和 Legacy 模式的引导命令并不相同：

* UEFI 模式下引导命令类似于

{% highlight bash %}
set root='hd0,gpt1'
chainloader /EFI/Microsoft/Boot/bootmgfw.efi
{% endhighlight %}

* Legacy 模式下引导命令类似于

{% highlight bash %}
chainloader (hd0,1)+1
{% endhighlight %}

(2) Windows (BCD) 引导 GRUB

可以使用 EasyBCD 软件把 Linux 系统加入 Windows 的“选择操作系统”菜单中。此时的引导是先引导进入 BCD，然后 BCD 将引导权交给 GRUB/GRUB2，从而启动 Linux 系统。

BCD 并不能直接引导 Linux 或 Linux 安装盘。

## OS X 对两种模式的支持

### 苹果电脑

苹果电脑一律采用 Apple EFI。因此苹果电脑不支持 Legacy 模式。

苹果电脑采用 GPT 分区表，如果未安装其他操作系统，则苹果电脑有三个分区：ESP、苹果系统 (HFS+ 文件系统)、Recovery HD (HFS+ 文件系统)。

如果希望通过 U 盘引导，U 盘里面的系统必须支持 UEFI 引导。如果支持，需要在开机之前插入 U 盘，并按住 Option 键开机，这样就可以从启动选项中看到自己的 U 盘。

如果希望在苹果电脑中安装 Windows 操作系统，建议使用系统自带的“Boot Camp助理”。如果只是偶尔用用 Windows 或其他系统，可以考虑安装专门为苹果电脑设计的 `Parallels Desktop` 虚拟机软件，这样对电脑的影响更小。

### “黑苹果”

“黑苹果”指在普通电脑上安装的 OS X (10.5 或更高版本)。苹果官方并不允许在普通电脑上安装 OS X。不过，OS X 从理论和实践上又可以装到普通电脑上，所以仍然有很多人去尝试安装。

黑苹果的安装非常复杂。这里只讨论启动和分区的问题。

黑苹果无法直接引导，因此必须要用启动器来启动。目前流行的启动器有 Clover (四叶草) 和 Chamaleon (变色龙)。前者既支持UEFI启动又支持Legacy，而后者只支持Legacy。

由于 OS X 只支持 GPT 分区，所以在 MBR 分区上安装系统时需要对系统内核进行破解。这意味着每次系统升级都需要重新破解内核。如果有条件，建议把自己的电脑换成 UEFI + GPT 模式。

### Linux 与 OS X 共存

GRUB2 支持引导苹果系统。不过有一个条件：原生的苹果电脑。

如果用的是“黑苹果”，那么仍然可以通过 GRUB2 引导黑苹果的启动器，间接地引导进入苹果系统。
