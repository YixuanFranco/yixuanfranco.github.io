---
layout: post
title: 怎么才能放飞自我的玩儿Python
categories:
- Geek
---  

![]()(DraggedImage.png)

问题是有一天, 我在群里抱怨, 为毛每次学习 `data science` 都要重装 Jupyter. 我仿佛一年已经重装了100次 jupyter.

于是大妈怼道, “所以让你装 `pyenv`呐, 提醒过很多次了.”

接下来就很想软广对白了.

我: “到底为啥要用 pyenv 啊. 要用 Python3 吗?”
大妈: “pyenv 可以集中管理配置主要 5 种 100 多版本的 Python 运行环境
以及具体任何一个版本环境中和工程匹配的任意模块桟.”

好, 听到这里我还是不甚明白. 总之我就想让 jupyter 在我的电脑里能够用, 最好也别影响我的 Python 2.7.  乍一听, pyenv 仿佛是可以解决我这个需求的. 于是开始作…

### 1. 安装 pyenv 先
[完整的 pyenv 安装方式在这里]()(https://github.com/pyenv/pyenv "完整的 pyenv 安装方式在这里")

这里我只说 Mac 的安装方法:
`$ brew update`
`$ brew install pyenv`

### 2. pyenv 怎么用

逻辑:
`1. 先查查有哪些可以用的版本`
`2. 将这个版本下载下来`
`3. 切换来...切换去...切换来...切换去`

可用命令:
1. `pyenv install -l`
	这个命令会罗列出所有可用的版本.   
	![]()(DraggedImage-1.png)
2. `pyenv install 3.7.0 `
	这个命令可以让你在电脑上装上 Python 的 3.7.0 版本. 如果你想装别的版本, 可以把 3.7.0 替换成别的东西.
3. `pyenv versions`
	查查你现在已经拥有什么版本的 Python 了.  
	![]()(DraggedImage-2.png)
	这是我目前已经有的版本, system 前面有个小星星, 证明我现在整个电脑的 Python 版本是系统自带的 Python 版本(可以用 python —version 来查询).
4.  `pyenv version`
	注意, 这个命令符的结尾少了一个 s. 和上一个命令不一样.
	这个命令行的作用是, 看看你目前处在哪个版本里.
	![]()(DraggedImage-3.png)
	为了方便你看一下这两个命令的区别, 我把效果一起截图啦
5. `pyenv global 3.7.0 `
	这个命令是, 把整台电脑都变换为 Python 3.7.0 的环境. 变换完之后, 可以用上面学过的 `pyenv version` 查看一下是否已经变换成功.
6. `pyenv local 3.7.0 `
	如果我建立了一个项目文件夹, 名叫 `Yixuan`, 我想把 Yixuan 这个文件夹的环境变为 Python 的 3.7.0 的话, 我就可以   
	`cd Yixuan`
	`pyevn local 3.7.0`
	然后再用 pyenv version 检验一下是否变幻成功即可. 
7. ``  

pyenv 有用的命令大概就这么多. 

### 3. 下面我们说说  pyenv-virtualenv

安装文档是这个地址: https://github.com/pyenv/pyenv-virtualenv
不是 `virtualenv`, 是 `pyenv-virtualenv`. 我就安错了, 所以吓得我把原地址都放出来了.

安装命令是: `brew install pyenv-virtualenv`

大妈提醒, 里面有巨坑一个. 记得在 **/.bash\_profile** (如果你是 zsh 的话, 建议在 **/.zshrc**)里写入:
`export PATH="~/.pyenv/bin:$PATH"`
`eval "$(pyenv init -)"`
`eval "$(pyenv virtualenv-init -)"`

大妈提示道: 这三行是他反复明确, 不能缺少和错误的!!!

### 4. 所以我们稀里糊涂的装上了pyenv-virtual…
那么问题来了: 这玩意儿咋干啥用? 咋用?

1. 这玩意儿干啥用….
	它可以一键复制粘贴你已经配置好的 Python 环境. 
2. 咋用?
	比如我现在建立了一个文件夹叫 jupyter, 我想要给这个 jupyter 一个 Python3.7.0 的环境.
	命令: `pyenv virtualenv 3.7.0 jupyter`
	然后你在这里面装上各种的 jupyter.  而这个时候…你再用 `pyenv versions`这个命令行, 会发现环境里面多了一个:
	![]()(DraggedImage-4.png)
3. 然后….
	下次你再想作什么 jupyter 的东西, 直接建个工程文件夹, 然后 `pyenv local 3.7.0/envs/jupyter `, 就可以一键粘贴 jupyter 的使用环境了. 是不是很吊炸天. 


