1.vim 与 neovim 使用系统剪切板的不同

Nvim has no direct connection to the system clipboard. Instead it depends on
a provider which transparently uses shell commands to communicate with the
system clipboard or any other clipboard "backend".

2.安装xsel或xclip程序。

	sudo apt install xclip

3.修改配置文件

init.vim中添加一行：

	set clipboard+=unnamedplus