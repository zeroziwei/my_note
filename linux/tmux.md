---
created: 2024-07-08T10:01:34 (UTC +08:00)
tags: 
source: https://louiszhai.github.io/2017/09/30/tmux/
---
[[tmux 快捷键]]

开始之前，我们先了解下基本概念：

tmux采用C/S模型构建，输入tmux命令就相当于开启了一个服务器，此时默认将新建一个**会话**，然后会话中默认新建一个**窗口**，窗口中默认新建一个**面板**。会话、窗口、面板之间的联系如下：

一个tmux `session`（会话）可以包含多个`window`（窗口），窗口默认充满会话界面，因此这些窗口中可以运行相关性不大的任务。

一个`window`又可以包含多个`pane`（面板），窗口下的面板，都处于同一界面下，这些面板适合运行相关性高的任务，以便同时观察到它们的运行情况。

[![](http://louiszhai.github.io/docImages/tmux01.png "基本概念")](http://louiszhai.github.io/docImages/tmux01.png "基本概念")

### [](https://louiszhai.github.io/2017/09/30/tmux/#%E4%BC%9A%E8%AF%9D "会话")会话

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E6%96%B0%E5%BB%BA%E4%BC%9A%E8%AF%9D "新建会话")新建会话

新建一个tmux session非常简单，语法为`tmux new -s session-name`，也可以简写为`tmux`，为了方便管理，建议指定会话名称，如下。

```
tmux # 新建一个无名称的会话
tmux new -s demo # 新建一个名称为demo的会话
```

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E6%96%AD%E5%BC%80%E5%BD%93%E5%89%8D%E4%BC%9A%E8%AF%9D "断开当前会话")断开当前会话

会话中操作了一段时间，我希望断开会话同时下次还能接着用，怎么做？此时可以使用detach命令。

```
tmux detach # 断开当前会话，会话在后台运行
```

也许你觉得这个太麻烦了，是的，tmux的会话中，我们已经可以使用tmux快捷键了。使用快捷键组合`Ctrl+b` + `d`，三次按键就可以断开当前会话。

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E8%BF%9B%E5%85%A5%E4%B9%8B%E5%89%8D%E7%9A%84%E4%BC%9A%E8%AF%9D "进入之前的会话")进入之前的会话

断开会话后，想要接着上次留下的现场继续工作，就要使用到tmux的attach命令了，语法为`tmux attach-session -t session-name`，可简写为`tmux a -t session-name` 或 `tmux a`。通常我们使用如下两种方式之一即可：

```
tmux a # 默认进入第一个会话
tmux a -t demo # 进入到名称为demo的会话
```

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E5%85%B3%E9%97%AD%E4%BC%9A%E8%AF%9D "关闭会话")关闭会话

会话的使命完成后，一定是要关闭的。我们可以使用tmux的kill命令，kill命令有`kill-pane`、`kill-server`、`kill-session` 和 `kill-window`共四种，其中`kill-session`的语法为`tmux kill-session -t session-name`。如下：

```
tmux kill-session -t demo # 关闭demo会话
tmux kill-server # 关闭服务器，所有的会话都将关闭
```

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E6%9F%A5%E7%9C%8B%E6%89%80%E6%9C%89%E7%9A%84%E4%BC%9A%E8%AF%9D "查看所有的会话")查看所有的会话

管理会话的第一步就是要查看所有的会话，我们可以使用如下命令：

```
tmux list-session # 查看所有会话
tmux ls # 查看所有会话，提倡使用简写形式
```

如果刚好处于会话中怎么办？别担心，我们可以使用对应的tmux快捷键`Ctrl+b` + `s`，此时tmux将打开一个会话列表，按上下键(⬆︎⬇︎)或者鼠标滚轮，可选中目标会话，按左右键（⬅︎➜）可收起或展开会话的窗口，选中目标会话或窗口后，按回车键即可完成切换。

[![](http://louiszhai.github.io/docImages/tmux02.png "查看会话")](http://louiszhai.github.io/docImages/tmux02.png "查看会话")

### [](https://louiszhai.github.io/2017/09/30/tmux/#Tmux%E5%BF%AB%E6%8D%B7%E6%8C%87%E4%BB%A4 "Tmux快捷指令")Tmux快捷指令

关于快捷指令，首先要认识到的是：tmux的所有指令，都包含同一个前缀，默认为`Ctrl+b`，输入完前缀过后，控制台激活，命令按键才能生效。前面tmux会话相关的操作中，我们共用到了两个快捷键`Ctrl+b` + `d`、`Ctrl+b` + `s`，但这仅仅是冰山一角，欲窥tmux庞大的快捷键体系，请看下表。

表一：系统指令。

| 前缀 | 指令 | 描述 |
| --- | --- | --- |
| `Ctrl+b` | `?` | 显示快捷键帮助文档 |
| `Ctrl+b` | `d` | 断开当前会话 |
| `Ctrl+b` | `D` | 选择要断开的会话 |
| `Ctrl+b` | `Ctrl+z` | 挂起当前会话 |
| `Ctrl+b` | `r` | 强制重载当前会话 |
| `Ctrl+b` | `s` | 显示会话列表用于选择并切换 |
| `Ctrl+b` | `:` | 进入命令行模式，此时可直接输入`ls`等命令 |
| `Ctrl+b` | `[` | 进入复制模式，按`q`退出 |
| `Ctrl+b` | `]` | 粘贴复制模式中复制的文本 |
| `Ctrl+b` | `~` | 列出提示信息缓存 |

表二：窗口（window）指令。

| 前缀 | 指令 | 描述 |
| --- | --- | --- |
| `Ctrl+b` | `c` | 新建窗口 |
| `Ctrl+b` | `&` | 关闭当前窗口（关闭前需输入`y` or `n`确认） |
| `Ctrl+b` | `0~9` | 切换到指定窗口 |
| `Ctrl+b` | `p` | 切换到上一窗口 |
| `Ctrl+b` | `n` | 切换到下一窗口 |
| `Ctrl+b` | `w` | 打开窗口列表，用于且切换窗口 |
| `Ctrl+b` | `,` | 重命名当前窗口 |
| `Ctrl+b` | `.` | 修改当前窗口编号（适用于窗口重新排序） |
| `Ctrl+b` | `f` | 快速定位到窗口（输入关键字匹配窗口名称） |

表三：面板（pane）指令。

| 前缀 | 指令 | 描述 |
| --- | --- | --- |
| `Ctrl+b` | `"` | 当前面板上下一分为二，下侧新建面板 |
| `Ctrl+b` | `%` | 当前面板左右一分为二，右侧新建面板 |
| `Ctrl+b` | `x` | 关闭当前面板（关闭前需输入`y` or `n`确认） |
| `Ctrl+b` | `z` | 最大化当前面板，再重复一次按键后恢复正常（v1.8版本新增） |
| `Ctrl+b` | `!` | 将当前面板移动到新的窗口打开（原窗口中存在两个及以上面板有效） |
| `Ctrl+b` | `;` | 切换到最后一次使用的面板 |
| `Ctrl+b` | `q` | 显示面板编号，在编号消失前输入对应的数字可切换到相应的面板 |
| `Ctrl+b` | `{` | 向前置换当前面板 |
| `Ctrl+b` | `}` | 向后置换当前面板 |
| `Ctrl+b` | `Ctrl+o` | 顺时针旋转当前窗口中的所有面板 |
| `Ctrl+b` | `方向键` | 移动光标切换面板 |
| `Ctrl+b` | `o` | 选择下一面板 |
| `Ctrl+b` | `空格键` | 在自带的面板布局中循环切换 |
| `Ctrl+b` | `Alt+方向键` | 以5个单元格为单位调整当前面板边缘 |
| `Ctrl+b` | `Ctrl+方向键` | 以1个单元格为单位调整当前面板边缘（Mac下被系统快捷键覆盖） |
| `Ctrl+b` | `t` | 显示时钟 |

tmux的丝滑分屏功能正是得益于以上系统、窗口、面板的快捷指令，只要你愿意，你就可以解除任意的快捷指令，然后绑上你喜欢的指令，当然这就涉及到它的可配置性了，请继续往下读。

### [](https://louiszhai.github.io/2017/09/30/tmux/#%E7%81%B5%E6%B4%BB%E7%9A%84%E9%85%8D%E7%BD%AE%E6%80%A7 "灵活的配置性")灵活的配置性

除了快捷指令外，tmux还提供了类似vim的配置性功能。可配置性是软件的一项进阶级功能，只有具备了可配置性，软件才有了鲜活的个性，用户才能体会到操作的快感。

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E4%BF%AE%E6%94%B9%E6%8C%87%E4%BB%A4%E5%89%8D%E7%BC%80 "修改指令前缀")修改指令前缀

相信只要你用过几次tmux，就会发现`Ctrl+b`指令前缀，着实不太方便。这两个键相距太远，按键成本太高了。因此我们首先需要将它更换为距离更近的`Ctrl+a`组合键，或者不常用的 \` 键（当然其他键也是可以的）。

tmux的用户级配置文件为`~/.tmux.conf`（没有的话就创建一个），修改快捷指令，只需要增加如下三行即可。

```
set -g prefix C-a #
unbind C-b # C-b即Ctrl+b键，unbind意味着解除绑定
bind C-a send-prefix # 绑定Ctrl+a为新的指令前缀

# 从tmux v1.6版起，支持设置第二个指令前缀
set-option -g prefix2 ` # 设置一个不常用的`键作为指令前缀，按键更快些
```

修改的`~/.tmux.conf`配置文件有如下两种方式可以令其生效：

-   restart tmux。
-   在tmux窗口中，先按下`Ctrl+b`指令前缀，然后按下系统指令`:`，进入到命令模式后输入`source-file ~/.tmux.conf`，回车后生效。

既然快捷指令如此方便，更为优雅的做法是新增一个加载配置文件的快捷指令 ，这样就可以随时随地load新的配置了，如下所示。

```
# 绑定快捷键为r
bind r source-file ~/.tmux.conf \; display-message "Config reloaded.."
```

请特别注意，在已经创建的窗口中，即使加载了新的配置，旧的配置依然有效（只要你新加的功能没有覆盖旧的配置，因此如果你第一次绑定快捷指令为`x`键，然后又改为绑定`y`键，那么`x`和`y`都将有效），新建会话不受此影响，将直接采用新的配置。

既然我们已经迈出配置化的第一步，那么接下来我们可以做得更多。

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E6%96%B0%E5%A2%9E%E9%9D%A2%E6%9D%BF "新增面板")新增面板

tmux中，使用最多的功能之一就是新增一个面板。水平方向新增面板的指令是 `prefix` + `"` ，垂直方向是 `prefix` + `%`，`"` 和 `%`需要两个键同时按下才能完成，加上指令前缀至少需要3~4次按键才能组成一个完整的指令，同时这个两个键也不够醒目和方便，因此我们可以绑定两个更常用的指令 `-`、`|`，如下所示：

```
unbind '"'
bind - splitw -v -c '#{pane_current_path}' # 垂直方向新增面板，默认进入当前目录
unbind %
bind | splitw -h -c '#{pane_current_path}' # 水平方向新增面板，默认进入当前目录
```

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E5%BC%80%E5%90%AF%E9%BC%A0%E6%A0%87%E6%94%AF%E6%8C%81 "开启鼠标支持")开启鼠标支持

默认情况下，tmux的多窗口之间的切换以及面板大小调整，需要输入指令才能完成，这一过程，涉及到的指令较多，而且操作麻烦，特别是面板大小调整，指令难以一步到位，这个时候开启鼠标支持就完美了。

对于tmux v2.1(2015.10.28)之前的版本，需加入如下配置：

```
setw -g mode-mouse on # 支持鼠标选取文本等
setw -g mouse-resize-pane on # 支持鼠标拖动调整面板的大小(通过拖动面板间的分割线)
setw -g mouse-select-pane on # 支持鼠标选中并切换面板
setw -g mouse-select-window on # 支持鼠标选中并切换窗口(通过点击状态栏窗口名称)
```

有的地方可能会出现`set-window-option`的写法，`setw`就是它的别名。

对于tmux v2.1及以上的版本，仅需加入如下配置：

```
set-option -g mouse on # 等同于以上4个指令的效果
```

需要注意的是，开启鼠标支持后，iTem2默认的鼠标选中即复制功能需要同时按下 `Alt` 键，才会生效。

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E5%BF%AB%E9%80%9F%E9%9D%A2%E6%9D%BF%E5%88%87%E6%8D%A2 "快速面板切换")快速面板切换

鼠标支持确实能带来很大的便捷性，特别是对于习惯了鼠标操作的tmux新手，但对于键盘爱好者而言，这不是什么好消息，对他们而言，双手不离键盘是基本素质。

虽然指令前缀加`方向键`可以切换面板，但`方向键`太远，不够快，不够Geek。没关系，我们可以将面板切换升级为熟悉的`h`、`j`、`k`、`l`键位。

```
# 绑定hjkl键为面板切换的上下左右键
bind -r k select-pane -U # 绑定k为↑
bind -r j select-pane -D # 绑定j为↓
bind -r h select-pane -L # 绑定h为←
bind -r l select-pane -R # 绑定l为→
```

`-r`表示可重复按键，大概500ms之内，重复的`h`、`j`、`k`、`l`按键都将有效，完美支持了快速切换的Geek需求。

除了上下左右外， 还有几个快捷指令可以设置。

```
bind -r e lastp # 选择最后一个面板
bind -r ^e last # 选择最后一个窗口

bind -r ^u swapp -U # 与前一个面板交换位置
bind -r ^d swapp -D # 与后一个面板交换位置
```

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E9%9D%A2%E6%9D%BF%E5%A4%A7%E5%B0%8F%E8%B0%83%E6%95%B4 "面板大小调整")面板大小调整

习惯了全键盘操作后，命令的便捷性不言而喻。既然面板切换的指令都可以升级，面板大小调整的指令自然也不能落后。如下配置就可以升级你的操作：

```
# 绑定Ctrl+hjkl键为面板上下左右调整边缘的快捷指令
bind -r ^k resizep -U 10 # 绑定Ctrl+k为往↑调整面板边缘10个单元格
bind -r ^j resizep -D 10 # 绑定Ctrl+j为往↓调整面板边缘10个单元格
bind -r ^h resizep -L 10 # 绑定Ctrl+h为往←调整面板边缘10个单元格
bind -r ^l resizep -R 10 # 绑定Ctrl+l为往→调整面板边缘10个单元格
```

以上，`resizep`即`resize-pane`的别名。

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E9%9D%A2%E6%9D%BF%E6%9C%80%E5%A4%A7%E5%8C%96 "面板最大化")面板最大化

当窗口中面板的数量逐渐增多时，每个面板的空间就会逐渐减少。为了保证有足够的空间显示内容，tmux从v1.8版本起，提供了面板的最大化功能，输入`tmux-prefix+z`，就可以最大化当前面板至窗口大小，只要再重复输入一次，便恢复正常。那么tmux v1.8以下的版本，怎么办呢？别急，有大神提供了如下的解决方案。

首先编写一个zoom脚本，该脚本通过新建一个窗口，交换当前面板与新的窗口默认面板位置，来模拟最大的功能；通过重复一次按键，还原面板位置，并关闭新建的窗口，来模拟还原功能，如下所示：

```
#!/bin/bash -f
currentwindow=`tmux list-window | tr '\t' ' ' | sed -n -e '/(active)/s/^[^:]*: *\([^ ]*\) .*/\1/gp'`;
currentpane=`tmux list-panes | sed -n -e '/(active)/s/^\([^:]*\):.*/\1/gp'`;
panecount=`tmux list-panes | wc | sed -e 's/^ *//g' -e 's/ .*$//g'`;
inzoom=`echo $currentwindow | sed -n -e '/^zoom/p'`;
if [ $panecount -ne 1 ]; then
    inzoom="";
fi
if [ $inzoom ]; then
    lastpane=`echo $currentwindow | rev | cut -f 1 -d '@' | rev`;
    lastwindow=`echo $currentwindow | cut -f 2- -d '@' | rev | cut -f 2- -d '@' | rev`;
    tmux select-window -t $lastwindow;
    tmux select-pane -t $lastpane;
    tmux swap-pane -s $currentwindow;
    tmux kill-window -t $currentwindow;
else
    newwindowname=zoom@$currentwindow@$currentpane;
    tmux new-window -d -n $newwindowname;
    tmux swap-pane -s $newwindowname;
    tmux select-window -t $newwindowname;
fi
```

不妨将该脚本存放在`~/.tmux`目录中（没有则新建目录），接下来只需要绑定一个快捷指令就行，如下。

```
unbind z
bind z run ". ~/.tmux/zoom"
```

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E7%AA%97%E5%8F%A3%E5%8F%98%E4%B8%BA%E9%9D%A2%E6%9D%BF "窗口变为面板")窗口变为面板

通过上面的zoom脚本，面板可以轻松地最大化为一个新的窗口。那么反过来，窗口是不是可以最小化为一个面板呢？

> 试想这样一个场景：当你打开多个窗口后，然后想将其中几个窗口合并到当前窗口中，以便对比观察输出。

实际上，你的要求就是将其它窗口变成面板，然后合并到当前窗口中。对于这种操作，我们可以在当前窗口，按下`prefix` + `:`，打开命令行，然后输入如下命令：

```
join-pane -s window01 # 合并名称为window01的窗口的默认（第一个）面板到当前窗口中
join-pane -s window01.1 # .1显式指定了第一个面板，.2就是第二个面板(我本地将面板编号起始值设置为1，默认是0)
```

每次执行`join-pane`命令都会合并一个面板，并且指定的窗口会减少一个面板，直到面板数量为0，窗口关闭。

除了在当前会话中操作外，`join-pane`命令甚至可以从其它指定会话中合并面板，格式为`join-pane -s [session_name]:[window].[pane]`，如`join-pane -s 2:1.1` 即合并第二个会话的第一个窗口的第一个面板到当前窗口，当目标会话的窗口和面板数量为0时，会话便会关闭。

注：上一节中的`swap-pane`命令与`join-pane`语法基本一致。

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E5%85%B6%E4%BB%96%E9%85%8D%E7%BD%AE "其他配置")其他配置

```
bind m command-prompt "splitw -h 'exec man %%'"   # 绑定m键为在新的panel打开man
# 绑定P键为开启日志功能，如下，面板的输出日志将存储到桌面
bind P pipe-pane -o "cat &gt;&gt;~/Desktop/#W.log" \; display "Toggled logging to ~/Desktop/#W.log"
```

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E6%81%A2%E5%A4%8D%E7%94%A8%E6%88%B7%E7%A9%BA%E9%97%B4 "恢复用户空间")**恢复用户空间**

tmux会话中，Mac的部分命令如 `osascript`、`open`、`pbcopy` 或 `pbpaste`等可能会失效（失效命令未列全）。

部分bug列表如下：

-   [applescript - Unable to run ‘display notification’ using osascript in a tmux session](https://apple.stackexchange.com/questions/174779/unable-to-run-display-notification-using-osascript-in-a-tmux-session)
-   [osx - “open” command doesn’t work properly inside tmux](https://stackoverflow.com/questions/30404944/open-command-doesnt-work-properly-inside-tmux/30412054#30412054)
-   [clipboard - Can’t paste into MacVim](https://stackoverflow.com/questions/16618992/cant-paste-into-macvim/16661806#16661806)

对此，我们可以通过安装`reattach-to-user-namespace`包装程序来解决这个问题。

```
brew install reattach-to-user-namespace
```

在`~/.tmux.conf`中添加配置：

```
set -g default-command "reattach-to-user-namespace -l $SHELL"
```

这样你的交互式shell最终能够重新连接到用户级的命名空间。由于连接状态能够被子进程继承，故以上配置保证了所有从 shell 启动的命令能够被正确地连接。

有些时候，我们可能会在不同的操作系统中共享配置文件，如果你的tmux版本大于1.9，我们还可以使用`if-shell`来判断是否Mac系统，然后再指定`default-command`。

```
if-shell 'test "$(uname -s)" = Darwin' 'set-option -g default-command "exec reattach-to-user-namespace -l $SHELL"'

```

对于tmux v1.8及更早的版本，可以使用如下包装后的配置：

```
set-option -g default-command 'command -v reattach-to-user-namespace &gt;/dev/null &amp;&amp; exec reattach-to-user-namespace -l "$SHELL" || exec "$SHELL"'
```

以上，`$SHELL`对应于你的默认Shell，通常是`/usr/bin/bash` 或 `/usr/local/bin/zsh`。

### [](https://louiszhai.github.io/2017/09/30/tmux/#%E5%A4%8D%E5%88%B6%E6%A8%A1%E5%BC%8F "复制模式")复制模式

tmux中操作文本，自然离不开复制模式，通常使用复制模式的步骤如下：

1.  输入 `` `+[ `` 进入复制模式
2.  按下 `空格键` 开始复制，移动光标选择复制区域
3.  按下 `回车键` 复制选中文本并退出复制模式
4.  按下 `` `+] `` 粘贴文本

查看复制模式默认的快捷键风格：

```
tmux show-window-options -g mode-keys # mode-keys emacs
```

默认情况下，快捷键为`emacs`风格。

为了让复制模式更加方便，我们可以将快捷键设置为熟悉的`vi`风格，如下：

```
setw -g mode-keys vi # 开启vi风格后，支持vi的C-d、C-u、hjkl等快捷键
```

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E8%87%AA%E5%AE%9A%E4%B9%89%E5%A4%8D%E5%88%B6%E5%92%8C%E9%80%89%E6%8B%A9%E5%BF%AB%E6%8D%B7%E9%94%AE "自定义复制和选择快捷键")**自定义复制和选择快捷键**

除了快捷键外，复制模式的启用、选择、复制、粘贴等按键也可以向`vi`风格靠拢。

```
bind Escape copy-mode # 绑定esc键为进入复制模式
bind -t vi-copy v begin-selection # 绑定v键为开始选择文本
bind -t vi-copy y copy-selection # 绑定y键为复制选中文本
bind p pasteb # 绑定p键为粘贴文本（p键默认用于进入上一个窗口，不建议覆盖）
```

以上，绑定 `v`、`y`两键的设置只在tmux v2.4版本以下才有效，对于v2.4及以上的版本，绑定快捷键需要使用 `-T` 选项，发送指令需要使用 `-X` 选项，请参考如下设置：

```
bind -T copy-mode-vi v send-keys -X begin-selection
bind -T copy-mode-vi y send-keys -X copy-selection-and-cancel
```

#### Buffer缓存

tmux复制操作的内容默认会存进`buffer`里，`buffer`是一个粘贴缓存区，新的缓存总是位于栈顶，它的操作命令如下：

```
tmux list-buffers # 展示所有的 buffers
tmux show-buffer [-b buffer-name] # 显示指定的 buffer 内容
tmux choose-buffer # 进入 buffer 选择页面(支持jk上下移动选择，回车选中并粘贴 buffer 内容到面板上)
tmux set-buffer # 设置buffer内容
tmux load-buffer [-b buffer-name] file-path # 从文件中加载文本到buffer缓存
tmux save-buffer [-a] [-b buffer-name] path # 保存tmux的buffer缓存到本地
tmux paste-buffer # 粘贴buffer内容到会话中
tmux delete-buffer [-b buffer-name] # 删除指定名称的buffer
```

以上buffer操作在不指定buffer-name时，默认处理是栈顶的buffer缓存。

在tmux会话的命令行输入时，可以省略上述tmux前缀，其中list-buffers的操作如下所示：

[![](http://louiszhai.github.io/docImages/tmux06.png "list-buffers")](http://louiszhai.github.io/docImages/tmux06.png "list-buffers")

choose-buffer的操作如下所示：

[![](http://louiszhai.github.io/docImages/tmux05.png "choose-buffer")](http://louiszhai.github.io/docImages/tmux05.png "choose-buffer")

默认情况下，buffers内容是独立于系统粘贴板的，它存在于tmux进程中，且可以在会话间共享。

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E4%BD%BF%E7%94%A8%E7%B3%BB%E7%BB%9F%E7%B2%98%E8%B4%B4%E6%9D%BF "使用系统粘贴板")**使用系统粘贴板**

存在于tmux进程中的buffer缓存，虽然可以在会话间共享，但不能直接与系统粘贴板共享，不免有些遗憾。幸运的是，现在我们有成熟的方案来实现这个功能。

**在Linux上使用粘贴板**

通常，Linux中可以使用`xclip`工具来接入系统粘贴板。

首先，需要安装`xclip`。

```
sudo apt-get install xclip
```

然后，`.tmux.conf`的配置如下。

```
# buffer缓存复制到Linux系统粘贴板
bind C-c run " tmux save-buffer - | xclip -i -sel clipboard"
# Linux系统粘贴板内容复制到会话
bind C-v run " tmux set-buffer \"$(xclip -o -sel clipboard)\"; tmux paste-buffer"
```

按下`prefix` + `Ctrl` + `c` 键，buffer缓存的内容将通过`xlip`程序复制到粘贴板，按下`prefix` + `Ctrl` + `v`键，tmux将通过`xclip`访问粘贴板，然后由set-buffer命令设置给buffer缓存，最后由paste-buffer粘贴到tmux会话中。

**在Mac上使用粘贴板**

我们都知道，Mac自带 `pbcopy` 和 `pbpaste`命令，分别用于复制和粘贴，但在tmux命令中它们却不能正常运行。这里我将详细介绍下原因：

> Mac的粘贴板服务是在引导命名空间注册的。命名空间存在层次之分，更高级别的命名空间拥有访问低级别命名空间（如root引导命名空间）的权限，反之却不行。流程创建的属于Mac登录会话的一部分，它会被自动包含在用户级的引导命名空间中，因此只有用户级的命名空间才能访问粘贴板服务。tmux使用守护进程(3)库函数创建其服务器进程，在Mac OS X 10.5中，苹果改变了守护进程(3)的策略，将生成的过程从最初的引导命名空间移到了根引导命名空间。而根引导命名空间访问权限较低，这意味着tmux服务器，和它的子进程，一同失去了原引导命名空间的访问权限（即无权限访问粘贴板服务）。

如此，我们可以使用一个小小的包装程序来重新连接到合适的命名空间，然后执行访问用户级命名空间的粘贴板服务，这个包装程序就是`reattach-to-user-namespace`。

那么，Mac下`.tmux.conf`的配置如下：

```
# buffer缓存复制到Mac系统粘贴板
bind C-c run "tmux save-buffer - | reattach-to-user-namespace pbcopy"
# Mac系统粘贴板内容复制到会话
bind C-v run "reattach-to-user-namespace pbpaste | tmux load-buffer - \; paste-buffer -d"
```

`reattach-to-user-namespace` 作为包装程序来访问Mac粘贴板，按下`prefix` + `Ctrl` + `c` 键，buffer缓存的内容将复制到粘贴板，按下`prefix` + `Ctrl` + `v`键，粘贴板的内容将通过 load-buffer 加载，然后由 paste-buffer 粘贴到tmux会话中。

为了在复制模式中使用Mac系统的粘贴板，可做如下配置：

```
# 绑定y键为复制选中文本到Mac系统粘贴板
bind-key -T copy-mode-vi 'y' send-keys -X copy-pipe-and-cancel 'reattach-to-user-namespace pbcopy'
# 鼠标拖动选中文本，并复制到Mac系统粘贴板
bind-key -T copy-mode-vi MouseDragEnd1Pane send -X copy-pipe-and-cancel "pbcopy"
```

完成以上配置后记得重启tmux服务器。至此，复制模式中，按`y`键将保存选中的文本到Mac系统粘贴板，随后按`Command` + `v`键便可粘贴。

### [](https://louiszhai.github.io/2017/09/30/tmux/#%E4%BF%9D%E5%AD%98Tmux%E4%BC%9A%E8%AF%9D "保存Tmux会话")保存Tmux会话

信息时代，数据尤为重要。tmux保护现场的能力依赖于tmux进程，如果进程退出，则意味着会话数据的丢失，因此关机重启后，tmux中的会话将被清空，这不是我们想要见到的。幸运的是，目前有这样两款插件：`Tmux Resurrect` 和 `Tmux Continuum`，可以永久保存tmux会话（它们均适用于tmux v1.9及以上版本）。

#### [](https://louiszhai.github.io/2017/09/30/tmux/#Tmux-Resurrect "Tmux Resurrect")Tmux Resurrect

Tmux Resurrect无须任何配置，就能够备份tmux会话中的各种细节，包括窗口、面板的顺序、布局、工作目录，运行程序等等数据。因此它能在系统重启后完全地恢复会话。由于其幂等的恢复机制，它不会试图去恢复一个已经存在的窗口或者面板，所以，即使你不小心多恢复了几次会话，它也不会出现问题，这样主动恢复时我们就不必担心手抖多按了一次。另外，如果你是[tmuxinator](https://github.com/tmuxinator/tmuxinator)用户，我也建议你迁移到 tmux-resurrect插件上来，具体请参考[Migrating from `tmuxinator`](https://github.com/tmux-plugins/tmux-resurrect/blob/master/docs/migrating_from_tmuxinator.md#migrating-from-tmuxinator)。

Tmux Resurrec安装过程如下所示：

```
cd ~/.tmux
mkdir plugins
git clone https://github.com/tmux-plugins/tmux-resurrect.git
```

安装后需在`~/.tmux.conf`中增加一行配置：

```
run-shell ~/.tmux/plugins/tmux-resurrect/resurrect.tmux
```

至此安装成功，按下`prefix + r`重载tmux配置。

Tmux Resurrec提供如下两个操作：

-   **保存**，快捷指令是`prefix` + `Ctrl + s`，tmux状态栏在保存开始，保存后分别提示”Saving…”，”Tmux environment saved !”。
-   **恢复**，快捷指令是`prefix` + `Ctrl + r`，tmux状态栏在恢复开始，恢复后分别提示”Restoring…”，”Tmux restore complete !”。

保存时，tmux会话的详细信息会以文本文件的格式保存到`~/.tmux/resurrect`目录，恢复时则从此处读取，由于数据文件是明文的，因此你完全可以自由管理或者编辑这些会话状态文件（如果备份频繁，记得定期清除历史备份）。

**可选的配置**

Tmux Resurrec本身是免配置开箱即用的，但同时也提供了如下选项以便修改其默认设置。

```
set -g @resurrect-save 'S' # 修改保存指令为S
set -g @resurrect-restore 'R' 修改恢复指令为R
# 修改会话数据的保持路径，此处不能使用除了$HOME, $HOSTNAME, ~之外的环境变量
set -g @resurrect-dir '/some/path'
```

默认情况下只有一个保守的列表项（即`vi vim nvim emacs man less more tail top htop irssi mutt`）可以恢复，对此 [Restoring programs doc](https://github.com/tmux-plugins/tmux-resurrect/blob/master/docs/restoring_programs.md) 解释了怎么去恢复额外的项目。

**进阶的备份**

除了基础备份外，Tmux Resurrec还提供**进阶的备份功能**，如下所示：

-   恢复vim 和 neovim 会话
-   恢复面板内容
-   恢复shell的历史记录（实验性功能）

进阶的备份功能默认不开启，需要特别配置。

1）恢复vim 和 neovim 会话，需要完成如下两步：

-   通过vim的vim-obsession插件保存vim/neovim会话。
    
    ```
    cd ~/.vim/bundle
    git clone git://github.com/tpope/vim-obsession.git
    vim -u NONE -c "helptags vim-obsession/doc" -c q
    ```
    
-   在`~/.tmux.conf`中增加两行配置：
    
    ```
    set -g @resurrect-strategy-vim 'session' # for vim
    set -g @resurrect-strategy-nvim 'session' # for neovim
    ```
    

2）恢复面板内容，需在`~/.tmux.conf`中增加一行配置：

```
set -g @resurrect-capture-pane-contents 'on' # 开启恢复面板内容功能
```

目前使用该功能时，请确保tmux的`default-command`没有包含`&&` 或者`||`操作符，否则将导致[bug](https://github.com/tmux-plugins/tmux-resurrect/issues/98)。（查看`default-command`的值，请使用命令`tmux show -g default-command`。）

3）恢复shell的历史记录，需在`~/.tmux.conf`中增加一行配置：

```
set -g @resurrect-save-shell-history 'on'
```

由于技术的限制，保存时，只有无前台任务运行的面板，它的shell历史记录才能被保存。

#### [](https://louiszhai.github.io/2017/09/30/tmux/#Tmux-Continuum "Tmux Continuum")Tmux Continuum

可能你嫌手动保存和恢复太过麻烦，别担心，这不是问题。Tmux Continuum 在 Tmux Resurrec的基础上更进一步，现在保存和恢复全部自动化了，如你所愿，可以无感使用tmux，不用再担心备份问题。

Tmux Continuum安装过程如下所示（它依赖Tmux Resurrect，请保证已安装Tmux Resurrect插件）：

```
cd ~/.tmux/plugins
git clone https://github.com/tmux-plugins/tmux-continuum.git
```

安装后需在`~/.tmux.conf`中增加一行配置：

```
run-shell ~/.tmux/plugins/tmux-continuum/continuum.tmux
```

Tmux Continuum默认每隔15mins备份一次，我设置的是一天一次：

```
set -g @continuum-save-interval '1440'
```

**关闭自动备份**，只需设置时间间隔为 `0` 即可：

```
set -g @continuum-save-interval '0'
```

想要在**tmux启动时就恢复最后一次保存的会话环境**，需增加如下配置：

```
set -g @continuum-restore 'on' # 启用自动恢复
```

如果不想要启动时自动恢复的功能了，直接移除上面这行就行。想要绝对确定自动恢复不会发生，就在用户根目录下创建一个`tmux_no_auto_restore`空文件（创建命令：`touch ~/tmux_no_auto_restore`），该文件存在时，自动恢复将不触发。

对于tmux高级用户（可能就是你）而言，同时运行多个tmux服务器也是有可能的。你可能并不希望后面启用的几个tmux服务器自动恢复或者自动保存会话。因此Tmux Continuum会优先在第一个启用的tmux服务器中生效，随后启用的tmux服务器不再享受自动恢复或自动保存会话的待遇。

实际上，不管Tmux Continuum功能有没有启用，或者多久保存一次，我们都有办法从状态栏知晓。Tmux Continuum提供了一个查看运行状态的插值`#{continuum_status}`，它支持`status-right` 和 `status-left`两种状态栏设置，如下所示：

```
set -g status-right 'Continuum status: #{continuum_status}'
```

tmux运行时，`#{continuum_status}` 将显示保存的时间间隔（单位为分钟），此时状态栏会显示：

```
Continuum status: 1440
```

如果其自动保存功能关闭了，那么状态栏会显示：

```
Continuum status: off
```

借助Tmux Continuum插件，Mac重启时，我们甚至可以选择在`Terminal` 或者 `iTerm2` 中自动全屏启用tmux。

为此，需在`~/.tmux.conf`中增加一行配置：

```
set -g @continuum-boot 'on'
```

Mac下，自动启用tmux还支持如下选项：

-   `set -g @continuum-boot-options 'fullscreen'` ，`Terminal`自动全屏，tmux命令在`Terminal`中执行。
-   `set -g @continuum-boot-options 'iterm'` ， `iTerm2` 替换 `Terminal` 应用，tmux命令在`iTerm2`中执行。
-   `set -g @continuum-boot-options 'iterm,fullscreen'`，`iTerm2`自动全屏，tmux命令在`iTerm2`中执行。

Linux中则没有这些选项，它只能设置为自动启用tmux服务器。

#### [](https://louiszhai.github.io/2017/09/30/tmux/#Tpm "Tpm")Tpm

以上，我们直接安装了tmux插件。这没有问题，可当插件越来越多时，我们就会需要统一的插件管理器。因此官方提供了tpm（支持tmux v1.9及以上版本）。

tpm安装过程如下所示：

```
cd ~/.tmux/plugins
git clone https://github.com/tmux-plugins/tpm
```

安装后需在`~/.tmux.conf`中增加如下配置：

```
# 默认需要引入的插件
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'

# 引入其他插件的示例
# set -g @plugin 'github_username/plugin_name' # 格式：github用户名/插件名
# set -g @plugin 'git@github.com/user/plugin' # 格式：git@github插件地址

# 初始化tmux插件管理器(保证这行在~/.tmux.conf的非常靠后的位置)
run '~/.tmux/plugins/tpm/tpm'
```

然后按下`prefix + r`重载tmux配置，使得tpm生效。

基于tpm插件管理器，**安装插件**仅需如下两步：

1.  在`~/.tmux.conf`中增加新的插件，如`set -g @plugin '...'`。
2.  按下`prefix` + `I`键下载插件，并刷新tmux环境。

**更新插件**，请按下`prefix` + `U` 键，选择待更新的插件后，回车确认并更新。

**卸载插件**，需如下两步：

1.  在`~/.tmux.conf`中移除插件所在行。
2.  按下`prefix` + `alt` + `u` 移除插件。

### [](https://louiszhai.github.io/2017/09/30/tmux/#%E4%BC%9A%E8%AF%9D%E5%85%B1%E4%BA%AB "会话共享")会话共享

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E7%BB%93%E5%AF%B9%E7%BC%96%E7%A8%8B "结对编程")结对编程

tmux多会话连接实时同步的功能，使得结对编程成为了可能，这也是开发者最喜欢的功能之一。现在就差一步了，就是借助tmate把tmux会话分享出去。

tmate是tmux的管理工具，它可以轻松的创建tmux会话，并且自动生成ssh链接。

安装tmate

```
brew install tmate
```

使用tmate新建一个tmux会话

```
tmate
```

此时屏幕下方会显示ssh url，如下所示：

[![](http://louiszhai.github.io/docImages/tmux07.png "ssh url")](http://louiszhai.github.io/docImages/tmux07.png "ssh url")

查看tmate生成的ssh链接

```
tmate show-messages
```

生成的ssh url如下所示，其中一个为只读，另一个可编辑。

[![](http://louiszhai.github.io/docImages/tmux08.png "ssh url")](http://louiszhai.github.io/docImages/tmux08.png "ssh url")

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E5%85%B1%E4%BA%AB%E8%B4%A6%E5%8F%B7-amp-%E7%BB%84%E4%BC%9A%E8%AF%9D "共享账号&组会话")共享账号&组会话

使用tmate远程共享tmux会话，受制于多方的网络质量，必然会存在些许延迟。如果共享会话的多方拥有同一个远程服务器的账号，那么我们可以使用`组会话`解决这个问题。

先在远程服务器上新建一个公共会话，命名为`groupSession`。

```
tmux new -s groupSession
```

其他用户不去直接连接这个会话，而是通过创建一个新的会话来加入上面的公共会话`groupSession`。

```
tmux new -t groupSession -s otherSession
```

此时两个用户都可以在同一个会话里操作，就会好像第二个用户连接到了`groupSession`的会话一样。此时两个用户都可以创建新建的窗口，新窗口的内容依然会实时同步，但是其中一个用户切换到其它窗口，对另外一个用户没有任何影响，因此在这个共享的组会话中，用户各自的操作可以通过新建窗口来执行。即使第二个用户关闭`otherSession`会话，共享会话`groupSession`依然存在。

组会话在共享的同时，又保留了相对的独立，非常适合结对编程场景，它是结对编程最简单的方式，如果账号不能共享，我们就要使用下面的方案了。

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E7%8B%AC%E7%AB%8B%E8%B4%A6%E5%8F%B7-amp-Socket%E5%85%B1%E4%BA%AB%E4%BC%9A%E8%AF%9D "独立账号&Socket共享会话")独立账号&Socket共享会话

开始之前我们需要确保用户对远程服务器上同一个目录拥有相同的读写权限，假设这个目录为`/var/tmux/`。

使用new-session（简写new）创建会话时，使用的是默认的socket位置，默认socket无法操作，所以我们需要创建一个指定socket文件的会话。

```
tmux -S /var/tmux/sharefile
```

另一个用户进入时，需要指定socket文件加入会话。

```
tmux -S /var/tmux/sharefile attach
```

这样，两个不同的用户就可以共享同一个会话了。

通常情况下，不同的用户使用不同的配置文件来创建会话，但是，使用指定socket文件创建的tmux会话，会话加载的是第一个创建会话的用户的`~/.tmux.conf`配置文件，随后加入会话的其他用户，依然使用同一份配置文件。

### [](https://louiszhai.github.io/2017/09/30/tmux/#Tmux%E4%BC%98%E5%8C%96 "Tmux优化")Tmux优化

要想tmux更加人性化、性能更佳，不妨参考下如下配置。

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E8%AE%BE%E7%BD%AE%E7%AA%97%E5%8F%A3%E9%9D%A2%E6%9D%BF%E8%B5%B7%E5%A7%8B%E5%BA%8F%E5%8F%B7 "设置窗口面板起始序号")设置窗口面板起始序号

```
set -g base-index 1 # 设置窗口的起始下标为1
set -g pane-base-index 1 # 设置面板的起始下标为1
```

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E8%87%AA%E5%AE%9A%E4%B9%89%E7%8A%B6%E6%80%81%E6%A0%8F "自定义状态栏")自定义状态栏

```
set -g status-utf8 on # 状态栏支持utf8
set -g status-interval 1 # 状态栏刷新时间
set -g status-justify left # 状态栏列表左对齐
setw -g monitor-activity on # 非当前窗口有内容更新时在状态栏通知

set -g status-bg black # 设置状态栏背景黑色
set -g status-fg yellow # 设置状态栏前景黄色
set -g status-style "bg=black, fg=yellow" # 状态栏前景背景色

set -g status-left "#[bg=#FF661D] ❐ #S " # 状态栏左侧内容
set -g status-right 'Continuum status: #{continuum_status}' # 状态栏右侧内容
set -g status-left-length 300 # 状态栏左边长度300
set -g status-right-length 500 # 状态栏左边长度500

set -wg window-status-format " #I #W " # 状态栏窗口名称格式
set -wg window-status-current-format " #I:#W#F " # 状态栏当前窗口名称格式(#I：序号，#w：窗口名称，#F：间隔符)
set -wg window-status-separator "" # 状态栏窗口名称之间的间隔
set -wg window-status-current-style "bg=red" # 状态栏当前窗口名称的样式
set -wg window-status-last-style "fg=red" # 状态栏最后一个窗口名称的样式

set -g message-style "bg=#202529, fg=#91A8BA" # 指定消息通知的前景、后景色
```

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E5%BC%80%E5%90%AF256-colors%E6%94%AF%E6%8C%81 "开启256 colors支持")开启256 colors支持

默认情况下，tmux中使用vim编辑器，文本内容的配色和直接使用vim时有些差距，此时需要开启256 colors的支持，配置如下。

```
set -g default-terminal "screen-256color"
```

或者：

```
set -g default-terminal "tmux-256color"
```

或者启动tmux时增加参数`-2`：

```
alias tmux='tmux -2' # Force tmux to assume the terminal supports 256 colours
```

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E5%85%B3%E9%97%AD%E9%BB%98%E8%AE%A4%E7%9A%84rename%E6%9C%BA%E5%88%B6 "关闭默认的rename机制")关闭默认的rename机制

tmux默认会自动重命名窗口，频繁的命令行操作，将频繁触发重命名，比较浪费CPU性能，性能差的计算机上，问题可能更为明显。建议添加如下配置关闭rename机制。

```
setw -g automatic-rename off
setw -g allow-rename off
```

#### [](https://louiszhai.github.io/2017/09/30/tmux/#%E5%8E%BB%E6%8E%89%E5%B0%8F%E5%9C%86%E7%82%B9 "去掉小圆点")去掉小圆点

tmux默认会同步同一个会话的操作到所有会话连接的终端窗口中，这种同步机制，限制了窗口的大小为最小的会话连接。因此当你开一个大窗口去连接会话时，实际的窗口将自动调整为最小的那个会话连接的窗口，终端剩余的空间将填充排列整齐的小圆点，如下所示。

[![](http://louiszhai.github.io/docImages/tmux03.png "dot")](http://louiszhai.github.io/docImages/tmux03.png "dot")

为了避免这种问题，我们可以在连接会话的时候，断开其他的会话连接。

```
tmux a -d
```

如果已经进入了tmux会话中，才发现这种问题，这个时候可以输入命令达到同样的效果。

```
`: a -d
```

[![](http://louiszhai.github.io/docImages/tmux04.gif "remove dot")](http://louiszhai.github.io/docImages/tmux04.gif "remove dot")

### [](https://louiszhai.github.io/2017/09/30/tmux/#%E8%84%9A%E6%9C%AC%E5%8C%96%E7%9A%84Tmux "脚本化的Tmux")脚本化的Tmux

tmux作为终端复用软件，支持纯命令行操作也是其一大亮点。你既可以启用可视化界面创建会话，也可以运行脚本生成会话，对于tmux依赖者而言，编写几个tmux脚本批量维护会话列表，快速重启、切换、甚至分享部分会话都是非常方便的。可能会有人说为什么不用Tmux Resurrect呢？是的，Tmux Resurrect很好，一键恢复也很诱人，但是对于一个维护大量tmux会话的用户而言，一键恢复可能不见得好，分批次恢复可能是他（她）更想要的，脚本化的tmux就很好地满足了这点。

脚本中创建tmux会话时，由于不需要开启可视化界面，需要输入`-d`参数指定会话后台运行，如下。

```
tmux new -s init -d # 后台创建一个名称为init的会话
```

新建的会话，建议**重命令会话的窗口名称**，以便后续维护。

```
# 重命名init会话的第一个窗口名称为service
tmux rename-window -t "init:1" service
```

现在，可以在刚才的窗口中**输入指令**了。

```
# 切换到指定目录并运行python服务
tmux send -t "init:service" "cd ~/workspace/language/python/;python2.7 server.py" Enter
```

一个面板占用一个窗口可能太浪费了，我们来**分个屏**吧。

```
# 默认上下分屏
tmux split-window -t "init:service"
# 切换到指定目录并运行node服务
tmux send -t "init:service" 'cd ~/data/louiszhai/node-webserver/;npm start' Enter
```

现在一个窗口拥有上下两个面板，是时候**创建一个新的窗口**来运行更多的程序了。

```
# 新建一个名称为tool的窗口
tmux neww -a -n tool -t init # neww等同于new window
# 运行weinre调试工具
tmux send -t "init:tool" "weinre --httpPort 8881 --boundHost -all-" Enter
```

另外新建窗口运行程序，有更方便的方式，比如使用 `processes` 选项。

```
tmux neww-n processes ls # 新建窗口并执行命令，命令执行结束后窗口将关闭
tmux neww-n processes top # 由于top命令持续在前台运行，因此窗口将保留，直到top命令退出
```

新的窗口，我们尝试下水平分屏。

```
# 水平分屏
tmux split-window -h -t "init:tool"
# 切换到指定目录并启用aria2 web管理后台
tmux send -t "init:tool" "cd ~/data/tools/AriaNg/dist/;python -m SimpleHTTPServer 10108" Enter
```

类似的脚本，我们可以编写一打，这样快速重启、切换、甚至分享会话都将更加便捷。

### [](https://louiszhai.github.io/2017/09/30/tmux/#%E5%BC%80%E6%9C%BA%E8%87%AA%E5%8A%A8%E5%90%AF%E7%94%A8Web%E6%9C%8D%E5%8A%A1%E5%99%A8 "开机自动启用Web服务器")开机自动启用Web服务器

开机自动准备工作环境是一个很好的idea，但却不好实现。对于程序员而言，一个开机即用的计算机会节省大量的初始化操作，特别是前端工程师，本地常常会启用多个服务器，每次开机挨个启动将耗时耗力。为此，在遇到tmux之前，我常常拖延重启计算机的时机，一度连续运行Mac一月之久，直到它不堪重负。

有了tmux脚本化的基础，开机自动启用web服务器就不在话下了，接杯水的时间，计算机就重启恢复了满血。如下是操作步骤：

首先，上面的tmux脚本，可以合并到同一个文件中，指定文件权限为可执行，并命名为`init.sh`（名称可自取）。

```
chmod u+x ./init.sh
```

然后，打开 `系统偏好设置` - `用户与群组` - `登录项`，点击添加按钮`+`，选择刚刚保存的`init.sh`脚本，最终效果如下：

[![](http://louiszhai.github.io/docImages/tmux09.png "init.sh")](http://louiszhai.github.io/docImages/tmux09.png "init.sh")

至此，Mac开机将自动运行 `init.sh` 脚本，自动启用web服务器。

完成了上面这些配置，就真正实现了一键开机。

最后，附上我本地的配置文件 [.tmux.conf](https://github.com/Louiszhai/tmux/blob/master/.tmux.conf)，以及启动脚本 [init.sh](https://github.com/Louiszhai/tmux/blob/master/init.sh)。明天就是国庆了，祝大家国庆快乐！

___

版权声明：转载需注明作者和出处。

本文作者: [louis](https://github.com/Louiszhai)

本文链接: [http://louiszhai.github.io/2017/09/30/tmux/](http://louiszhai.github.io/2017/09/30/tmux/)

相关文章

-   [FAQ · tmux/tmux Wiki](https://github.com/tmux/tmux/wiki/FAQ#i-found-a-bug-in-tmux-what-do-i-do)
-   [“Maximizing” a pane in tmux](https://superuser.com/questions/238702/maximizing-a-pane-in-tmux/357799#433702)
-   [vim - selecting text tmux copy-mode](https://superuser.com/questions/196060/selecting-text-tmux-copy-mode#1204738)
-   [Getting 256 colors to work in tmux](https://unix.stackexchange.com/questions/1045/getting-256-colors-to-work-in-tmux)
-   [tmux: Productive Mouse-Free Development 中文版](https://www.kancloud.cn/kancloud/tmux/62462)
-   [Notes and workarounds for accessing the Mac OS X pasteboard in tmux sessions](https://github.com/ChrisJohnsen/tmux-MacOSX-pasteboard)
-   [Unable to copy from tmux to the OS X clipboard · Issue #909 · tmux/tmux](https://github.com/tmux/tmux/issues/909)
