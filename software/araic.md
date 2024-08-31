---
created: 2024-07-01T22:09:34 (UTC +08:00)
tags: [ed2k linux]
source: https://blog.csdn.net/xiao_yi_xiao/article/details/119325348
author: 
---

##  aMule 

相关链接

```
apt install amule # apt安装
 
 
http://www.amule.org/ # 官网
 
https://github.com/amule-project/amule #github主页
 
http://wiki.amule.org/wiki/HowTo_Compile_In_Debian#Are_you_sure_you_want_to_compile.3F #编译安装页面
```

### qbittorrent

看到网上推荐推荐用 utorrent 的比较多，不过在 [Debian](https://so.csdn.net/so/search?q=Debian&spm=1001.2101.3001.7020) 官方仓库里没有搜到，却看到一款叫做 qbittorrent 的软件！下载体验了一下，还不错！自带 GUI，操作简单直观！如果你是在系统下通过 GUI 界面直接下载，qbittorrent 是个不错的选择！但是如果你需求跟我一样，需要远程下载到服务器，那请继续看后面的方案。

### aria2

aria2 是 Linux 平台下很火的一款下载工具，当了解到 aria2 +　webui 的方案后，便立即使用这个软件作为远程下载方案，因为此方案不仅可以方便直观的在 linux 主机上直接下载，也可以作为服务器方便的进行远程下载！

![](https://img-blog.csdnimg.cn/img_convert/ad61146aa495417231baeaa9709d9e3d.png)

安装 aria2

Debian 下通过如下命令可以一键安装

```
sudo apt-get install aria2
```

安装 webui-aria2

通过 git 一键克隆到本地即可

```
git clone https://github.com/ziahamza/webui-aria2.git
```

运行使用启动 aria2 的 rpc 服务 (建议使用 serccn 后台启用)

```
aria2c --enable-rpc --rpc-listen-all --disable-ipv6 --rpc-secret yourpassword
```

为了方便以后使用，可以将以上命令放在脚本中使用

开启 web 服务

如果你的电脑安装了 nginx, 你可以直接将 webui-aria2 目录下的所有文件复制到你的 nginx 服务根目录，然后在浏览器输入你的服务器 IP 或者本机输入 localhost 即可打开。打开后需要点击设置–链接设置，在密码令牌填入你启动 aria2 时设置的密钥！也可在 webui-aria2 目录下的 configuration.js 里面 token 填入你的密钥，以避免每次进入都需要输入！

如果你没有安装 nginx 或者不想使用 nginx, 你也可以使用 webui-aria2 自带的 js 脚本作为 web 服务，需要安装 nodejs, 在 webui-aria2 目录输入以下命令即可启动：

```
node node-server.js
```

参考：[https://blog.csdn.net/weixin\_33849758/article/details/117024657](https://blog.csdn.net/weixin_33849758/article/details/117024657)
