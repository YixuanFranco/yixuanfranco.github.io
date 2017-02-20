---
layout: post
title: 通过微信的 token (Django微信公众号开发二)
categories:
- Geek
---

	version: 0.0 
	by Yixuan Li
	TIME: Feb.19, 2017

第一篇已经讲完如何把 Django 的 APP 设置到 80 端口啦! 啊哈...部署 + Token 已经占了微信开发的 80% 的坑. 剩下的就容易很多很多很多很多很多了...

看了一下微信的[官方文档][1], 里面说道: (点击链接后, 点开接入指南)

	开发者通过检验signature对请求进行校验（下面有校验方式）。若确认此次GET请求来自微信服务器，请原样返回echostr参数内容，则接入生效，成为开发者成功，否则接入失败。加密/校验流程如下：
	1）将token、timestamp、nonce三个参数进行字典序排序
	2）将三个参数字符串拼接成一个字符串进行sha1加密
	3）开发者获得加密后的字符串可与signature对比，标识该请求来源于微信

上面就是开发思路了, 我们可以按照思路先写一个半伪代码:

 
	def wechat(request): #拿到微信发给咱们的请求
	    if request.method == "GET":   # 如果是 GET 请求就继续
	        # 1. 然后在这个部分里, 
	        # 我们要拿到微信 GET 发给我们的一些参数, 
	        # 官文里写了, 微信发给我们的 GET 里有:
	        # signature, timestamp, nonce, echostr
	
	             # 2. 然后我们按照上面的加密流程依次
	         # 排序: 把 token, timestamp 和 nonce 进行 字典排序
	              #  拼成字符串然后加密字符串
	
	        #3. 看看加密字符串和之前拿到的 signature 一不一样, 
	        # 如果一样就原样返回 echostr 参数内容

嗯哼…大体思路就是这样的, 把它写成可用的代码就行了. 打开你 Django 项目中的 view…

	# coding=utf-8
	import hashlib 
	from django.http import HttpResponse 
	from django.shortcuts import render
	from django.views.decorators.csrf import csrf_exempt
	
	@csrf_exempt
	wechat_token = '12345678'  # 改成你自己设置的 token 就可以啦.
	
	def wechat(request):
	    if request.method == "GET":    # 确定微信发来了 GET, 得到所有参数
	        signature = request.GET.get("signature", None)
	        timestamp = request.GET.get("timestamp", None)
	        nonce = request.GET.get("nonce", None)
	        echostr = request.GET.get("echostr", None)
	    token = wechat_token 
	        range_dict = [token, timestamp, nonce] # 做成一个字典
	        range_dict.sort()  # 把字典排序
	        range_str = "%s%s%s" % tuple(range_dict)  # 转换成元祖
	        range_str = hashlib.sha1(range_str).hexdigest() 然后用 hashlib 加密一下.
	
	        if range_str == signature:
	            return HttpResponse(echostr)
	        else:
	            return HttpResponse("weixin  index")  # 随便返回点儿什么

 以防你是小白…我的说点儿 Django 里的常识. 我们前一篇提到了, 假设你的服务器 外网 IP 是  http://123. 12. 123. 12, 要打开你的 Django 应用, 可以去 http://123. 12. 123. 12/wechat. 

Django 整个项目文件里, 有两个 urls.py, 一个在项目底下, 一个在 APP 底下. 拿我们举的例子来说:  
\* 我们的项目名叫 mysite, 有个 mysite/mysite/urls.py
* 我们的 App 名叫 wechat, 有个 mysite/wechat/urls.py

因为一个项目里面可以有多个 APP, 所以, 我们可以在 mysite/mysite/urls.py 里设置:

	urlpatterns = [
	    url(r'^admin/', admin.site.urls),
	    url(r'^wechat/', include('wechat.urls')) 
	]
上面设置好了之后, 我们输入 http://123. 12. 123. 12/admin 就可以打开管理应用界面, 打开 http://123. 12. 123. 12/wechat 就可以打开微信应用界面. 

同理, 在 wechat 底下, 我们也可以有不同的页面…于是, 我们可以去 mysite/wechat/urls.py 下面设置…

	urlpatterns = [
	    url(r'^archive', wechat.views.archive),
	    url(r'^cool/', wechat.views.wechat),
	]

^archive 是正则表达式, 也就是说, 如果我们的网址是: http://123. 12. 123. 12/wechat/archive, 我们就调用 wechat/views.py 里的 archive  函数.  

^cool代表以 cool 起头, 也就是说, 如果我们的网址是: http://123. 12. 123. 12/wechat/cool, 我们就调用 wechat/views.py 里的 wechat  函数. 

所以呐...我们验证 token 的函数, 可以放在 APP 下, views.py 文件中, wechat 函数里. 所以, 我们如果打开 views.py 这个文件夹的话, 里面会有两个函数(不是系统默认的, 都是自己写的)

	def archive(request):
	    # 一些 archive 函数要干的事情....
	    # 打开这个函数的地址地址: 
	    # http://123. 12. 123. 12/wechat/archive
	
	def wechat(request):
	    # 一些 wechat 函数要干的事情....
	    # 打开地址是:
	    # http://123. 12. 123. 12/wechat/wechat


所以, 我们上面已经写了 wechat(request) 这个函数, 你直接粘贴到你的 views.py 里即可, 但一定要记得在 urls.py 里加上东西. 并且要在 urls.py 的开头引用 wechat 应用里的 views 文件…(import wechat.views)

把这个文件写好了之后, 打开你微信公众号 token 验证页面(或者测试号验证页面), token 输入 12345678 (如果你已经把 token 改了就换成你改的数字), URL 输入  http://123. 12. 123. 12/wechat/wechat , 就能验证过去啦.

如果验证不过去...欢迎来问询, 我的邮件地址是: me@yixuan.li
BTW, 如果你已经开了 VPN, 那你应该能看到留言板…直接留言就好, 我也可以看到的昂!


[1]:	https://mp.weixin.qq.com/wiki