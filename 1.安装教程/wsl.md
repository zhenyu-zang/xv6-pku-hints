WSL(Windows Subsystem for Linux)是Windows10系统级支持的运行Linux内核的方法，相比于虚拟机，其启动更快，访问windows文件也更加方便，缺点是没有图形界面

如果您的win10系统已经安装过WSL，那么很可能由于Hyper-V开启而无法兼容其他虚拟机。那为什么不用WSL来完成lab呢

步骤0到步骤5和[微软官方帮助](https://docs.microsoft.com/zh-cn/windows/wsl/install-win10)手动安装WSL2的方法**完全一致**，如果自己可以熟练安装WSL2，或更想参考官方文档，请在完成后跳到步骤6

# 步骤 0：确保Windows版本可以支持

请确保您当前的系统是运行在x64下，版本号1903或更高，内部版本18362或更高的Windows

若要检查 Windows 版本及内部版本号，选择 Windows 徽标键 + R，然后键入“winver”，选择“确定”。 （或者在 Windows 命令提示符下输入 `ver` 命令）

# 步骤 1：学会启动PowerShell

在“开始”按钮上点击右键，在菜单中选择`Windows PowerShell（管理员）`，即可启动PowerShell。在教程后文中可能会多次需要您在PowerShell中运行命令

# 步骤 2：启用适用于 Linux 的 Windows 子系统

在PowerShell中运行命令
```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

# 步骤 3：启用虚拟机功能

在PowerShell中运行命令

```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

然后重新启动计算机

# 步骤 4：下载Linux内核更新包

下载 [适用于 x64 计算机的 WSL2 Linux 内核更新包 ](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)并运行

# 步骤 5：将 WSL 2 设置为默认WSL版本

在PowerShell中运行命令

```powershell
wsl --set-default-version 2
```

# 步骤 6：安装Ubuntu 20.04 LTS

打开win10自带的应用商店，搜索`Ubuntu`，选择Ubuntu 20.04 LTS下载安装

注意：（即使已经安装过WSL也）请务必安装20.04，亲测更早的版本（18.04 LTS）是不可用的

安装完成后，在开始屏幕中启动Ubuntu 20.04 LTS，配置用户名和密码

进入Linux终端后，尝试运行`cat /etc/debian_version`

正确的结果应该为

```
bullseye/sid
```

# 步骤7：安装所需的软件包

和完整Ubuntu20系统不同，WSL并没有自带Python2解释器，所以下面的软件包列表和官方指南有所不同

在Linux Shell中运行

```bash
sudo apt update
sudo apt install git build-essential gdb-multiarch qemu-system-misc gcc-riscv64-linux-gnu binutils-riscv64-linux-gnu python
```

会要求输入您的密码，并输入y确认下载，按提示操作即可

若觉得网速感人，可以更换镜像源

安装完成后，尝试运行`qemu-system-riscv64 --version`

正确的结果应该为

```
QEMU emulator version 4.2.1 (Debian 1:4.2-3ubuntu6.14)
Copyright (c) 2003-2019 Fabrice Bellard and the QEMU Project developers
```

# 步骤8：开始试用Lab

请参考[MIT的官方指南](https://pdos.csail.mit.edu/6.828/2020/labs/util.html)

# 关于GDB

## 配置

若要使用gdb，需要禁用对.gdbinit文件的安全限制

可以通过在Linux Shell中运行如下命令修改配置

```bash
echo set auto-load safe-path / > ~/.gdbinit
```

## 使用

在希望调试xv6时，通过cd打开xv6代码文件夹，在终端中输入

```bash
make qemu-gdb
```

这时当前终端会输出大量内容并卡住

不要关闭当前终端，开启另一个终端，通过cd打开xv6代码文件夹，输入

```bash
gdb-multiarch
```

即可进入gdb命令行，此时新终端是gdb命令行，旧终端是xv6命令行
