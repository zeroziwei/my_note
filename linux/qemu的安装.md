## install 
在 Ubuntu 22.04 上安装 QEMU 虚拟机模拟器，请遵循以下步骤：

### 1. 更新系统
首先，确保系统已更新到最新状态：

```bash
sudo apt update
sudo apt upgrade
```

### 2. 安装 QEMU
使用以下命令安装 QEMU 及其依赖包：

```bash
sudo apt install qemu qemu-kvm libvirt-daemon libvirt-clients bridge-utils virt-manager
```

这将安装以下组件：

- `qemu`：QEMU 软件包
- `qemu-kvm`：QEMU 的 [[KVM ]]支持
- `libvirt-daemon` 和 `libvirt-clients`：Libvirt 虚拟化 API 和客户端工具
- `bridge-utils`：用于创建和管理[[网络桥]]的实用程序
- `virt-manager`：虚拟机管理器，一个基于图形的虚拟机管理工具

### 3. 启动并启用 libvirtd 服务
安装完成后，启动并启用 libvirtd 服务：

```bash
sudo systemctl start libvirtd
sudo systemctl enable libvirtd
```

### 4. 验证安装
要验证 QEMU 和 KVM 安装是否成功，可以运行以下命令：

```bash
virsh list --all
```

如果安装成功，你应该会看到类似于以下的输出：

```plaintext
 Id   Name   State
--------------------
```

此外，你还可以运行以下命令检查 QEMU 版本：

```bash
qemu-system-x86_64 --version
```

现在，你已经成功在 Ubuntu 22.04 上安装了 QEMU。你可以使用 `virt-manager`（虚拟机管理器）或命令行工具创建和管理虚拟机。

