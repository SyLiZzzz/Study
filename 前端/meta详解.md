meta详解
meta标签可分为两大部分：http-equiv和name变量。
http-equiv
http-equiv相当于http的文件头作用，它可以向浏览器传回一些有用的信息，以帮助浏览器正确地显示网页内容。
值	描述	例子
content-type	设定页面使用的字符集	<meta http-equiv="content-Type" content="text/html; charset=utf-8">
GB2312时，代表说明网站是采用的编码是简体中文；
ISO-8859-1时，代表说明网站是采用的编码是英文；
UTF-8时，代表世界通用的语言编码；
PS：html5页面的做法是直接使用<meta charset="utf-8"/>
X-UA-Compatible	IE8的专用标记，用来指定IE8浏览器去模拟某个特定版本的IE浏览器的渲染方式，以此来解决部分兼容问题。	<meta http-equiv="X-UA-Compatible" content="IE=7">  
以上代码告诉IE浏览器，无论是否用DTD声明文档标准，IE8/9都会以IE7引擎来渲染页面。  
<meta http-equiv="X-UA-Compatible" content="IE=8">  
以上代码告诉IE浏览器，IE8/9都会以IE8引擎来渲染页面。  
<meta http-equiv="X-UA-Compatible" content="IE=edge">  
以上代码告诉IE浏览器，IE8/9及以后的版本都会以最高版本IE来渲染页面。  
<meta http-equiv="X-UA-Compatible" content="IE=Edge,chrome=1">
以上代码IE=edge告诉IE使用最新的引擎渲染网页，chrome=1则可以激活Chrome Frame.
PS：谷歌添加一个插件：Google Chrome Frame（谷歌内嵌浏览器框架GCF），这个插件可以让用户的IE浏览器外不变，但用户在浏览网页时，实际上使用的是Google Chrome浏览器内核，而且支持IE6、7、8等多个版本的IE浏览器。
expires	设定网页的过期时间。	<meta http-equiv="expires"content="Fri,12Jan200118:18:18GMT">
PS：必须使用GMT的时间格式
refresh	自动刷新并指向新页面。	<meta http-equiv="Refresh" content="2;URL=![img](file:///C:\Users\LZP\AppData\Local\Temp\[5UQ[BL(6~BS2JV6W}N6[%S.png)https://www.baidu.com">
PS：2代表页面停留2秒后跳转到后面的网址上
set-cookie	如果网页过期，那么自动删除本地cookie。	<meta http-equiv="Set-Cookie"content="cookie value=xxx;expires=Friday,12-Jan-200118:18:18GMT；path=/">
PS：必须使用GMT的时间格式。
windows-target	强制页面在当前窗口中以独立页面显示，可以防止自己的网页被别人当作一个frame页调用	<meta http-equiv="Window-target" content="_top">
cache-control	缓存机制	<meta http-equiv="cache-control" content="no-cache">
Public：指示响应可被任何缓存区缓存。
Private：指示对于单个用户的整个或部分响应消息，不能被共享缓存处理。这允许服务器仅仅描述当用户的部分响应消息，此响应消息对于其他用户的请求无效。
no-cache：指示请求或响应消息不能缓存。
no-store：用于防止重要的信息被无意的发布。在请求消息中发送将使得请求和响应消息都不使用缓存。
max-age：指示客户机可以接收生存期不大于指定时间（以秒为单位）的响应。
min-fresh：指示客户机可以接收响应时间小于当前时间加上指定时间的响应。
max-stale：指示客户机可以接收超出超时期间的响应消息。如果指定max-stale消息的值，那么客户机可以接收超出超时期指定值之内的响应消息。
name
name属性主要用于描述网页，与之对应的属性值为content，content中的内容主要是便于搜索引擎机器人查找信息和分类信息用的。
值	描述	例子
author	标注网页的作者	<meta name="author" content="dashen" />
keywords	页面关键词，用于被搜索引擎收录	<meta name="keywords" content="新闻,新闻中心, 新闻频道">
description	页面描述，用于搜索引擎收录	<meta name="description" content="新闻中心,包含有时政新闻、国内新闻、国际新闻、社会新闻、时事评论、新闻图片、新闻专题、新闻论坛、军事、历史、的专业时事报道门户网站">
viewport	用于控制页面缩放	<meta name="viewport" content="width=device-width, initial-scale=1, 
maximum-scale=1, minimum-scale=1, user-scalable=no">
详情可查看：![img](file:///C:\Users\LZP\AppData\Local\Temp\%W@GJ$ACOF(TYDYECOKVDYB.png)http://www.cnblogs.com/lovesong/p/4355029.html
renderer	指定双核浏览器默认以何种方式渲染页面。	<meta name="renderer" content="webkit">//默认webkit内核
<meta name="renderer" content="ie-comp">//默认IE兼容模式
<meta name="renderer" content="ie-stand">//默认IE标准模式
PS：360浏览器支持
generator	说明网站的采用的什么软件制作	<meta name="generator" content="Microsoft"/>
revised	网页文档的修改时间	<meta name="revised" content="设计网, 6/24/2015"/>
robots	用来告诉搜索机器人哪些页面需要索引，哪些页面不需要索引。	<meta name="robots" content="none"/>
取值：all|none|index|noindex|follow|nofollow, 默认all
all：文件将被检索，且页面上的链接可以被查询；
none：文件将不被检索，且页面上的链接不可以被查询；
index：文件将被检索；
follow：页面上的链接可以被查询；
noindex：文件将不被检索，但页面上的链接可以被查询；
nofollow：文件将不被检索，页面上的链接可以被查询。
copyright	网站版权信息	<meta name="copyright" content="本页版权XXX所有。All Rights Reserved" />