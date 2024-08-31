在 Ubuntu 中，我们可以通过修改 `.bashrc` 或 `.bash_aliases` 文件来设置别名。以下是具体步骤：

1. 打开终端。

2. 输入以下命令打开 `.bashrc` 文件：

   ```bash
   nano ~/.bashrc
   ```

   或者你也可以选择打开 `.bash_aliases` 文件（如果这个文件存在的话）：

   ```bash
   nano ~/.bash_aliases
   ```

3. 在文件的底部添加你的别名。例如，如果你想创建一个别名 `update` 来代替 `sudo apt update` 和 `sudo apt upgrade -y`，你可以添加以下行：

   ```bash
   alias update='sudo apt update && sudo apt upgrade -y'
   ```

4. 保存并关闭文件。如果你在使用 `nano`，你可以按 `Ctrl+X`，然后按 `Y`，最后按 `Enter` 来保存并关闭文件。

5. 为了让新的别名在当前终端会话中生效，你需要运行以下命令：

   ```bash
   source ~/.bashrc
   ```

   或者如果你在 `.bash_aliases` 文件中添加了别名，你需要运行：

   ```bash
   source ~/.bash_aliases
   ```

现在，你应该可以在终端中使用你的新别名了。需要注意的是，这些别名只在你的用户环境中有效。如果你需要在所有用户环境中都能使用这些别名，你可以在 `/etc/bash.bashrc` 文件中添加它们。