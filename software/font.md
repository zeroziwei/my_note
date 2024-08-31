## outlink 
[GitHub - ryanoasis/nerd-fonts: Iconic font aggregator, collection, & patcher. 3,600+ icons, 50+ patched fonts: Hack, Source Code Pro, more. Glyph collections: Font Awesome, Material Design Icons, Octicons, & more](https://github.com/ryanoasis/nerd-fonts)

## install 

在 Ubuntu 22.04 中，安装 `.ttf` 字体文件的步骤基本上是相同的。在最新版本的 Ubuntu 中，用户级别的字体应该放在 `~/.local/share/fonts` 而不是 `~/.fonts`。以下是具体步骤：

1. 打开终端。
2. 创建一个名为 `fonts` 的文件夹（如果尚未存在）在你的 `~/.local/share` 目录中。运行以下命令：
   ```bash
   mkdir -p ~/.local/share/fonts
   ```
3. 将 `.ttf` 字体文件复制到 `~/.local/share/fonts` 目录中。例如，如果你的字体文件名为 `myfont.ttf`，并且它位于 `~/Downloads` 目录中，你可以使用以下命令：
   ```bash
   cp ~/Downloads/myfont.ttf ~/.local/share/fonts/
   ```
   如果你有多个 `.ttf` 文件，你可以一次性复制它们。例如：
   ```bash
   cp ~/Downloads/*.ttf ~/.local/share/fonts/
   ```
4. 最后，更新字体缓存以使新字体生效：
   ```bash
   fc-cache -f -v
   ```

现在，新安装的字体应该在 Ubuntu 的字体选择器中可用，你可以在文本编辑器、图形设计软件等程序中使用它。



## instations

	 ubuntu nerd font 