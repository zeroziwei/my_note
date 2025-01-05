
## 1	nvim 系统 

- [[主题]]
- [[折叠代码]]


## 2	outlink

[从零开始配置 Neovim(Nvim) - MartinLwx's Blog](https://martinlwx.github.io/zh-cn/config-neovim-from-scratch/)

[ADkun/lvim-config-suggest: Suggestions of Neovim powered by LunarVim Nightly](https://github.com/ADkun/lvim-config-suggest)
## 3	install 

I had the same issue, I first tried using the neovim PPA here: [https://launchpad.net/~neovim-ppa/+archive/ubuntu/stable](https://launchpad.net/%7Eneovim-ppa/+archive/ubuntu/stable)

However the latest version on there is still only 0.7.2.

I got it to work installing via AppImage as described here, to get the latest release build which is currently 0.9.5: [https://github.com/neovim/neovim/releases/tag/stable](https://github.com/neovim/neovim/releases/tag/stable)

So first remove your current install:

```
sudo apt remove neovim
```

Then download the AppImage:

```
wget https://github.com/neovim/neovim/releases/download/stable/nvim.appimage
```

Make it executable and run it:

```
chmod u+x nvim.appimage && ./nvim.appimage
```

If you get a fuse error (as I did on Ubuntu 22.04), then you can first verify that you have fuse3:

```
dpkg -l | grep fuse3
```

If you see output showing you have it, then you can safely install the fuse 2 library side by side:

```
sudo apt install libfuse2
```

If you do NOT have fuse3 (as might be the case in pre-22 versions of Ubuntu), follow the instructions here: [https://docs.appimage.org/user-guide/troubleshooting/fuse.html#how-to-install-fuse](https://docs.appimage.org/user-guide/troubleshooting/fuse.html#how-to-install-fuse) OR you can look at the manual extraction instructions on the nvim release build page: [https://github.com/neovim/neovim/releases/tag/stable](https://github.com/neovim/neovim/releases/tag/stable)

Then try to run the app image again:

```
./nvim.appimage
```

Once working, I first moved it into a .bin folder so it wasn't just sitting in my home directory:

```
mv nvim.appimage ~/.bin/nvim.appimage
```

And added the following lines (aliases) to my .bashrc as per my personal preference:

```
# Editors
alias vi='~/.bin/nvim.appimage'
alias vim='~/.bin/nvim.appimage'
alias nvim='~/.bin/nvim.appimage'
```

Since I use those first two aliases anyway, I just added the nvim alias and didn't bother adding anything to $PATH.

Apparently some app images have updating built into them, but I don't think Neovim's does, so you will probably have to update it manually (unfortunately) - however it is a simple procedure (just download it and make it executable).

However, if you ever want to be able to run it as root (including with sudo), then you'll want to install it machine-wide, in which case just move it to /usr/bin:

```
sudo mv ~/.bin/nvim.appimage /usr/bin/nvim.appimage
```

And update aliases accordingly in your .bashrc

```
# Editors
alias vi='/usr/bin/nvim.appimage'
alias vim='/usr/bin/nvim.appimage'
alias nvim='/usr/bin/nvim.appimage'
```

Or just add it to $PATH if you prefer.



## 4	structure

- where  the plugins  were installed 
	/home/orange/.local/share/nvim/lazy/
	![[Pasted image 20240628025138.png]]

- configure 
	 /home/orange/.config/nvim
![[Pasted image 20240628033519.png]]

- [x] coc ✅ 2024-07-18
- [ ] snippts


## 5	coc plugins 

[zbirenbaum/neodim: Neovim plugin for dimming the highlights of unused functions, variables, parameters, and more](https://github.com/zbirenbaum/neodim)

[chentoast/marks.nvim: A better user experience for viewing and interacting with Vim marks.](https://github.com/chentoast/marks.nvim)

[基于 coc.nvim python 补全 |](https://breezetemple.github.io/2020/05/08/coc-python-completion/)

