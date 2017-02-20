---
layout: post
title: 微信的接收与回复 (Django微信公众号开发三)
categories:
- Geek
---

我靠…这是第一次没跑通就现在写文档的一次. 因为已经深陷在薛定谔的 BUG 里了是也乎...所以呐! 写一篇出来看看能不能把思路撸清楚了.  直觉来说....

	微信会扔出来: GET 与 POST
	如果是 GET:
	    我就验证一下 token 啊什么的...(上一篇已经提到)
	如果是 POST:
	    1. 拿到 xml (官文有模板和字段)
	    2. 解析 xml
	    3. 发回 xml (官文有模板和字段)

所以, 我现在主要解决的, 是如果是收到了 POST, 我们该当如何如何如. 我们的前提是使用 Django 吗. 所以, 我们要琢磨琢磨这架构应该怎么放...

撸一下顺序...

1. 接收类: 我们会收到一个 XML 信息, 
	* 这个信息有不同的类型, 
	* 有不同的字段, 
	* 所以我们在解析时候会拿到不同的东西, 
	* 这意味着我们得把收到的这个信息弄成一个类.
2.  处理回复: 我们处理拿到的 xml,
	* 判断分析一下是什么消息类型...
	* 然后根据类型来判断要拿什么数据
	* 然后扔出去不同的包.
3. 所以我们还会有一个 xml 的模板…
	* 不同的消息套用不同的 模板即可.


### 问题是…道理我都懂...但是我还是过不好这一生呐!
所以, 我还是现在 view 上改改看....

	elif request.method == "GET":
	        # do something about POST here
	        str_xml = request.body.decode('utf-8')    #use body to get raw data
	        xml = etree.fromstring(str_xml)            
	        toUserName = xml.find('ToUserName').text
	        fromUserName = xml.find('FromUserName').text
	        createTime = xml.find('CreateTime').text
	        msgType = xml.find('MsgType').text
	        content = xml.find('Content').text   #获得用户所输入的内容
	        msgId = xml.find('MsgId').text
	        return render(request, 'reply_text.xml',
	                      {'toUserName': fromUserName,
	                       'fromUserName': toUserName,
	                       'createTime': time.time(),
	                       'msgType': msgType,
	                       'content': content,
	                       },
	                       content_type = 'application/xml'
	        )
 
然后我就尝试着…把 view.py 的 POST 部分写了. 然后我又....在 [reply_text.xml][1] 里把模板按照官方文档填写好了. 

 
可是...为什么为什么为什么就是不行啊....传说中可能遇到的坑:
1. 我绝对已经去掉传说中的 csrf 了. 所以这个问题不是...
2. 该引用的我都引用了. 
#### 迷思…

打开服务器…用我的个人号往微信公众号发信息...服务器显示, 扔出了 POST + 一串充满 什么 timestamp, appip 的 URL.

 WTH, 这就奇怪了…这证明俺的服务器和微信的服务器其实是通的了. 可是为啥 Django 就不干活呢? 到底是哪里哪里哪里哪里出了问题呢? 俺不知道. 俺去看会动画片压压惊....   

	-------- 懵逼的分割线 --------
	Time: Feb. 20, 22:58 此时懵逼中

### 解决了!

所以我到底错在哪儿了, 在 Mr.佟的指导下…

我发现我简直是智障呐!!! 上面的 view.py  的第二段代码…我竟然写成了:

	elif request.method == "GET":
	        # do something about POST here
	        str_xml = request.body.decode('utf-8')    #use body to get raw data

我靠…没有想到在这里翻了沟而且一直没发现. 我靠靠靠靠靠…改成:

	elif request.method == "POST":
	        # do something about POST here
	        str_xml = request.body.decode('utf-8')    #use body to get raw data

按照上面说的逻辑改成 POST 就行了…果然代码还要别人 review 才知道自己自己脑袋哪根筋出了问题. 

	-----睡觉的分割线-----
	Time: Feb. 20 23:22 
	一下午的懵逼 VS. 解决只距离不到 30mins

[1]:	https://github.com/YixuanFranco/wx/blob/master/mysite/wechat/templates/reply_text.xml