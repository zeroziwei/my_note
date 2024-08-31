## 安装的应用在哪？

在Ubuntu系统中，使用`dpkg`安装的应用程序通常会遵循Linux的文件系统层次结构（Filesystem Hierarchy Standard, FHS）。下面是一些关于应用程序文件和目录的常见位置：

1. **二进制文件**：应用程序的可执行文件通常位于以下目录之一：
   - `/usr/bin`：大多数用户可执行程序的位置。
   - `/usr/sbin`：系统管理员可执行程序的位置。
   - `/usr/local/bin`：手动安装的本地应用程序的二进制文件位置。
   - `/usr/local/sbin`：手动安装的本地系统管理员可执行程序的位置。

2. **库文件**：应用程序的库文件通常位于`/usr/lib`或`/usr/local/lib`目录下。

3. **配置文件**：应用程序的配置文件通常位于`/etc`目录下。有时，配置文件会放在一个特定于应用程序的子目录中，例如`/etc/appname`。

4. **数据文件**：应用程序的数据文件（如模板、图标、文档等）通常位于`/usr/share`目录下的一个子目录中。

5. **日志文件**：应用程序的日志文件通常位于`/var/log`目录下，有时会在一个特定于应用程序的子目录中，例如`/var/log/appname`。

要查找使用`dpkg`安装的特定应用程序的文件，你可以运行以下命令：

```bash
dpkg -L package-name
```

其中`package-name`是你要查找的软件包名称。这个命令将列出该软件包安装的所有文件及其位置。

## 根据包名模糊查找 

如果你不确定包的完整名称，可以使用 `dpkg -l | grep keyword` 命令来查找包含关键字的包。例如，如果你想查找所有包含 "python3" 的包，你可以运行：

```bash
dpkg -l | grep python3
```

这将列出所有包含 "python3" 的包。然后，你可以使用 `dpkg -L package-name` 命令来查看特定包的文件。

另外，如果你只记得包名的一部分，你也可以使用 `apt-cache search keyword` 命令来查找包含关键字的包。例如：

```bash
apt-cache search python3
```

这将列出所有包含 "python3" 的包。然后，你可以使用 `dpkg -L package-name` 命令来查看特定包的文件。