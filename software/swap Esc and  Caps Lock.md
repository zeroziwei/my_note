要在 Ubuntu 中交换 Esc 和 Caps Lock 键，你可以使用 `dconf-editor` 或 `gsettings`。首先，我们来看如何使用 `dconf-editor`：

1. 安装 `dconf-editor`：
   如果你还没有安装 `dconf-editor`，可以通过以下命令安装：
   ```
   sudo apt-get install dconf-editor
   ```

2. 打开 `dconf-editor`：
   在终端中输入 `dconf-editor` 或者从应用菜单中打开。

3. 修改键盘映射设置：
   在 `dconf-editor` 中导航到：`/org/gnome/desktop/input-sources/xkb-options`。然后点击右侧的值，将其设置为 `['caps:swapescape']`。完成后，点击右上角的 ✅ 按钮应用更改。

如果你想使用 `gsettings`，请按照以下步骤操作：

1. 打开终端。

2. 运行以下命令：
   ```
   gsettings set org.gnome.desktop.input-sources xkb-options "['caps:swapescape']"
   ```

现在，Esc 和 Caps Lock 键应该已经交换了。如果你想恢复原始设置，可以使用 `dconf-editor` 将值设置为空数组 `[]`，或者使用 `gsettings` 运行以下命令：

```
gsettings set org.gnome.desktop.input-sources xkb-options "[]"
```

请注意，这些设置仅适用于 GNOME 桌面环境。如果你使用的是其他桌面环境，如 KDE Plasma、XFCE 或 MATE，设置方法可能会有所不同。