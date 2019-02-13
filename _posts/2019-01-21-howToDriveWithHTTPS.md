---
layout: post
title: 如何持证 (HTTPS) 开车
categories:
- Geek
---


## 背景

打算在 udemy 开设一门供小白使用的 **GitHub Pages** 课程, 于是强迫自己使用小白的操作习惯, 用鼠标 **点点点** 浏览 GitHub 的仓库. 结果发现在仓库的 setting 选项中, 竟然有强制设置 HTTPS 选项了.   

![GitHubPagesHttps](/images/https.png)

简要分析了一下, 目测作起来很简单, 所以开始作...

    参考教程:
    1. [GitHub Pages HTTPS 设置 | 查错指南](https://help.github.com/articles/troubleshooting-custom-domains/)
    2. [如何给你的 GitHub Pages 加上 HTTPS 证书](https://help.github.com/articles/securing-your-github-pages-site-with-https/)

作完后成果:

![driveWithHttps.png](/images/driveWithHttps.png)

:) 成功持证驾驶了是也乎...

## 分析

主要的流程貌似只有三个:  
- 在你的域名上增补新的 DNS Record.(万网, 狗爹, NameCheap, etc.)
- 在 GitHub 的 setting 选项里, 把 **Custom domain** 删除后再添加.
- 在 GitHub 的 setting 选项里, 选中 **Enforce HTTPS**.

## 实操

### 1. 在域名上增补新的 DNS Record

我的域名是在 **NameCheap** 上购买的, 所以直接按照教程所述, 在域名上加上这些 IP, 类型是**A records**:

    If you configured A records through your DNS provider, your A records must point your custom domain to the following IP addresses:

    185.199.108.153
    185.199.109.153
    185.199.110.153
    185.199.111.153

如图所示:  

![newDNS](/images/newDNS.png)

注意: 更新完 IP 后, 可能需要一定的时间才能生效. 需要用命令行 dig 一下. 把下面命令行中的 **example.com** 改为你的博客地址即可.  

    dig +noall +answer example.com

如果输出的结果依旧是:

    example.com		22903	IN	A	192.30.252.153

那么就还没有更新成功, 需要再等等. 如果输出的结果中包含:

    example.com			21313	IN	A	185.199.111.153
    example.com			21313	IN	A	185.199.109.153
    example.com			21313	IN	A	185.199.108.153
    example.com			21313	IN	A	185.199.110.153

那么新的 IP 已经绑定到了域名上, 我们就添加成功了 :)    


###  2. 在 GitHub 的网页上打开你的项目.  

点开 **setting** 选项:  

![setting](/images/setting.png)

- 向下滚动到 **GitHub Pages > Custom domain**, 把里面的域名删除后, 点击 save.  
- 然后再重新添加, 点击 save.  

###  3. 然后选中 **Enforce HTTPS**, 等待一段时间即可. 直到有一个绿色条显示:  
![publishe](/images/published.png)
就可以去网页上看看啦!

## 注意的点.

按照自己摸索和官方指示, 坑就这么几个:  

1. **你没耐心.**  
无论是在 DNS 上加 A record, 还是 **Enforce HTTPS**, 都需要等待一段时间. 不要疑神疑鬼疯狂刷新自己的 blog. 耐心的等就是了. 如果有错误, GitHub 会给你提示的.
2. **HTTP > https**  
注意你的 CSS, JS 和图片引用不要出问题. 因为我们已经改为了 HTTPS, 确保你引用的也是 HTTPS. 否则可能会炸.
3. **小概率事件:**    
If you're using an A record that points to **207.97.227.245 or 204.232.175.78**, you'll need to update your DNS settings, as we no longer serve Pages directly from those servers.  
如上所嗯哼...不过这么古老的 IP 估计已经没人在用了吧.



**PPPPP.S:**   
总之一切坑及梗已经在开头的两个参考教程中完备, 所有的坑都已经递归到开头的两个 links 中. 大家各显神通吧.
