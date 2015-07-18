---
layout: posts 
title: Http 的 header部分编码问题
---

## {{ page.title }} ##

### 问题描述 ###

一个网站在Cookie中记录了中文信息。我使用tcpdump将这个http请求dump出来之后。这些中文信息是使用%e5之类的urlencode之后的方式显示的。而我想在我的django程序中正确显示这些中文。

但是使用urldecode之后，这些中文信息显示为乱码。

这些中文信息在http request header 和 http response header 中都有出现。所以两处都有乱码出现的情况。

我现在就想把这些中文能够正确显示出来。

### 解决过程 ###

我首先拿请求头来做的修改。为了能在django的template中正确显示中文。我给项目添加了一个filter。代码如下：


	@register.filter()
	def decode_cn(value):
	    res = urllib.unquote(value)
	    return res


在template中使用这个filter之后，界面显示是为一堆乱码。

百思不得其解。urllib.unquote()之后的结果，如果在终端打印，是可以正确显示中文的。但是用在django中打印在网页上就会是乱码。
肯定是编码的问题了。

记得在以前排查一个什么问题的时候，发现有些网页使用iso8859-1的编码方式向服务端提交数据。我想这客户端是不是可以编码为
iso8859-1可以正确显示呢？ 代码就变成了下面这样：


	@register.filter()
	def decode_cn(value):
	    res = urllib.unquote(value)
	    res = res.encode('iso8859-1')
	    return res


终于，在template中显示出了中文。

但是，当我把这个filter使用在响应头的时候却出错了。报一个lastin-1不能识别一个超过256的值好像。因为当时匆忙，所以没有保留当时的出错信息。
这个问题太奇怪了。这段中文在请求头和相应头中一模一样，但是在请求头已经正常工作的情况下，响应头却除了错误。

我尝试在python终端下对请求头unquote之后的结果encode('iso8859-1')的时候，会出现和相应头一样的错误。但是上面的代码在django中却可以正确运行。

一直没想明白原因之后，一次偶然，在解码相应头的时候没有加encode()。中文却正确显示了。

也就是说请求头需要urllib.unquote(value).encode('iso8859-1')。但是相应头只需要urnlib.unquote(value)。

所以，最后代码变为：


	@register.filter()
	def decode_cn(value):
	    res = urllib.unquote(value)
	    res_cn = res
	    try:
		res_cn = res_cn.encode('iso8859-1')
	    except:
		return res
	    return res_cn


总算是解决了这个问题。

### 问题猜想 ###

因为后来也没有再去深入，所以这里只能是猜想原因了。

请求头是客户端发给服务端的。对于中文，客户端应该是采用了utf-8的编码。当我们将这些utf-8编码后的十六进制值尝试显示在html中的时候，
浏览器并不知道这是什么内容。所以按照每个字节的ascii码来解析，所以就变成了一些乱码。为了让浏览器知道知道如何显示这些值。我们将这些内容
进行iso8859-1的编码。(iso8859-1编码，大家可以找其他资料查看。)

相应体是服务端发送给客户端的内容。服务端应该是充分考虑了客户端浏览器识别这些内容的方法。所以，服务端应该是对这些内容直接采用了相应的编码方式。
所以，只要执行urllib.unquote()之后就可以正确显示了。

当然上面的原因都是瞎猜的，有些地方也解释不同。以后有时间了再仔细看看吧。如果您对这方面研究，或者了解更准确的原因，可以email我。我会更新这里的内容。


