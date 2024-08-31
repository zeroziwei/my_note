在Fish shell中设置别名与在其他shell（如Bash）中略有不同。Fish shell使用函数来实现别名功能。以下是如何在Fish shell中设置别名的步骤：

1. **打开终端**：首先，你需要打开你的终端。

2. **创建或编辑配置文件**：Fish shell的配置文件是`~/.config/fish/config.fish`。你可以使用任何文本编辑器来编辑这个文件。例如，你可以使用`nano`编辑器：

   ```sh
   nano ~/.config/fish/config.fish
   ```

3. **添加别名**：在`config.fish`文件中，你可以通过定义函数来创建别名。例如，如果你想为`ls -la`命令创建一个别名`ll`，你可以这样做：

   ```sh
   function ll
       ls -la $argv
   end
   ```

   这里，`$argv`是一个特殊的变量，它包含了传递给函数的所有参数。

4. **保存并退出**：在`nano`编辑器中，你可以按`Ctrl+O`保存文件，然后按`Ctrl+X`退出编辑器。

5. **重新加载配置**：为了使更改生效，你需要重新加载Fish shell的配置文件。你可以通过在终端中运行以下命令来实现：

   ```sh
   source ~/.config/fish/config.fish
   ```

现在，你应该可以使用你刚刚创建的别名了。例如，运行`ll`将会执行`ls -la`命令。

### 其他注意事项

- **别名持久化**：通过将别名定义添加到`config.fish`文件中，你可以确保这些别名在每次启动新的Fish shell会话时都可用。
- **查看现有别名**：你可以通过运行`functions`命令来查看当前定义的所有函数（包括别名）。例如：

  ```sh
  functions
  ```

  这将列出所有定义的函数，你可以从中找到你的别名。

通过这些步骤，你可以在Fish shell中轻松设置和管理别名。