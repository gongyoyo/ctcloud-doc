# 如何在 Clash 中使用订阅配置。\(Windows / macOS\) - 帮助中心

Clash for Windows \(CFW\) 和 ClashX \(macOS\) 可免费在 GitHub 获取，无需付费。

订阅配置功能仅提供给 Standard 和 Advanced 用户，如您使用 Basic，请使用其它客户端，或通过工单升级至 Standard 或 Advanced。  


## Clash for Windows 配置

[在 GitHub 上下载并安装 CFW \(选择最新版本 setup exe 文件\)](https://github.com/Fndroid/clash_for_windows_pkg/releases)

打开浏览器，前往 GFW.cat 客户中心 - [我的服务](https://my.gfw.cat/clientarea.php?action=productdetails)，点击「订阅信息」中的「其它配置」。

![](https://i.loli.net/2019/03/05/5c7e655068efd.png)![](https://i.loli.net/2019/03/06/5c7f3f6dbfe05.png)  


在新页面中，点击 Clash 托管配置右侧的「复制」，复制后打开 Clash for Windows，进入 Profiles 页面，将链接 URL 复制至下方 Remote Configuration file 框中，点击 Download 下载。

该页面中的链接和密码一样重要，请勿泄漏。如不慎外泄，请立即重置密码和订阅链接。

![CFW General &#x9875;&#x9762;&#x3002;](https://i.loli.net/2019/02/15/5c663b38dce52.png)

点击 Proxies 面板，选择你需要使用的代理，并选择是否开启广告屏蔽（DIRECT 为直连，REJECT 为屏蔽）

我们为您提供了三个自动节点「稳定」& 「低延迟」和「Netflix 最佳节点」，这三个节点可以根据网络环境自动切换至最佳节点。如您有自己的使用习惯，也可自行选择节点。  
 ![Clash for Windows Proxies &#x9875;&#x9762;&#x3002;](https://i.loli.net/2019/02/15/5c663b38decb3.png)  


前往 General 面板，开启系统代理 system proxy，将桌面上的 Clash 快捷方式复制至**开始菜单 -&gt; 程序 -&gt; 启动**中，以实现开机自启。  
 ![CFW General &#x9875;&#x9762;&#x3002;](https://i.loli.net/2019/02/15/5c663b38e26a0.png)

操作完成后，你就可以正常访问国际互联网了，之后您可以在任务栏中切换代理节点或关闭代理，我们建议您 24 小时开启应用。  


## ClashX \(macOS\) 配置

[在 GitHub 上下载并安装 ClashX \(选择最新版本 dmg 文件\)](https://github.com/yichengchen/clashX/releases)

打开浏览器，前往 GFW.cat 客户中心 - [我的服务](https://my.gfw.cat/clientarea.php?action=productdetails)，点击「订阅信息」中的「其它配置」。  
 ![](https://i.loli.net/2019/03/05/5c7e655068efd.png)![](https://i.loli.net/2019/03/05/5c7e6550a6718.png)  


开启 ClashX，点击上方任务栏中的 ClashX -&gt; Config -&gt; Remote config -&gt; Set URL，粘贴链接并确认

该页面中的链接和密码一样重要，请勿泄漏。如不慎外泄，请立即重置密码和订阅链接。  
 ![ClashX &#x914D;&#x7F6E;&#x3002;](https://i.loli.net/2019/02/15/5c663b393c2b0.png)  


点击任务栏上的图标选择代理节点和广告屏蔽功能（DIRECT 为直连，REJECT 为屏蔽）。

我们为您提供了三个自动节点「稳定」& 「低延迟」和「Netflix 最佳节点」，这三个节点可以根据网络环境自动切换至最佳节点。如您有自己的使用习惯，也可自行选择节点。  
 ![ClashX &#x9009;&#x62E9;&#x4EE3;&#x7406;&#x3002;](https://i.loli.net/2019/02/15/5c663b392ab6f.png)

开启 **Start at login** 和 **Set as system Proxy**，实现开机自启并自动设置为系统代理。  


操作完成后，你就可以正常访问国际互联网了，之后您可以在任务栏中切换代理节点或关闭代理，我们建议您 24 小时开启应用。  


## 其它

我们目前不确定 CFW 和 ClashX \(macOS\) 能否自动更新配置，如节点列表有更新（或您修改了密码），您可以手动更新配置。

如果一些特定 Windows 程序无法使用系统代理，请查看该程序设置项，如果需要手动设置代理服务器，地址为 127.0.0.1 或 localhost，端口请见 General 面板，Port 为 HTTP 代理端口，Socks Port 为 Socks4/5 代理端口，用户名和密码请留空。

