SQL 注入:

基本概念:
1、SQL 注入是指针对Web 应用使用的数据库,通过运行非法的SQL而产生的攻击. 该安全隐患具有可能引发极大的威胁，直接导致
个人信息及机密信息的泄漏.

攻击的原理:
利用单引号闭合的原理.
select username,password from admin where username='1' or 2>1 --'and password='111';
union select 1,version(),group_concat(table_name),4 from information_schema.tables where
table_schema=news.

攻击步骤:
1、id=1 union select 1,2,3,4
2、通过infosechma 查看表名.
3、user(),version() 查看权限和版本.
4、database() 查看数据库.
5、查看表.

如何预防SQL注入:
前端:
可以通过js 来做一些验证规则.
后端:
c# 将sql语句变量参数化:
@username 已经在param 里面定义了字符串类型,即便里面有单引号，分号。也会被当作字符串来处理.

https://blog.csdn.net/kl28978113/article/details/50980925


攻击后果:
1、非法查看或篡改数据库内的数据.
2、规避认证.
3、执行和数据库服务器业务关联的程序等.

CSRF:
基本概念:
由于HTTP协议是无状态的，所有客户端和服务器端的数据交换和鉴权信息都必须附带在HTTP请求中。

一个网站用户Bob可能正在浏览聊天论坛，而同时另一个用户Alice也在此论坛中，并且后者刚刚发布了一个具有Bob银行链接的图片消息。设想一下，Alice编写了一个在Bob的银行站点上进行取款的form提交的链接，并将此链接作为图片src。如果Bob的银行在cookie中保存他的授权信息，并且此cookie没有过期，那么当Bob的浏览器尝试装载图片时将提交这个取款form和他的cookie，这样在没经Bob同意的情况下便授权了这次事务。

攻击原理:
试想，一个攻击者需要怎样的条件来发起CSRF攻击？
例如有一个请求为GET http://a.com/removeFriend?friendId=123。
那么很简单，攻击者在自己的网站(http://b.com/evil.html)里插入一段代码(例如<img src=http://a.com/removeFriend?friendId=123>)，那么当攻击者访问evil.html的时候，就会把编码123的好友删掉了


更进一步，如果a.com有评论区，而评论区又允许插入外部图片，那么攻击者直接可以将链接发在评论区中，那么所有查看访问评论区的人都会中招。例如有一个请求为POST http://a.com/removeFriend，POST的数据为friendId: 123。
这个利用起来就稍微麻烦一点，直接将链接插入页面中将会不可行，攻击者需要在b.com构造一个FORM表单，当用户访问时，利用JS自动提交请求即可.

POST类型的CSRF
这种类型的CSRF危害没有GET型的大，利用起来通常使用的是一个自动提交的表单，如：
```
<form action=http://wooyun.org/csrf.php method=POST>
<input type="text" name="xx" value="11" />
</form>
<script> document.forms[0].submit(); </script> 
```

0x01 GET类型的CSRF
```
<img src=http://wooyun.org/csrf.php?xx=11 /> 
```

防御措施:
Token
目前主流的做法是使用Token抵御CSRF攻击。下面通过分析CSRF 攻击来理解为什么Token能够有效
CSRF攻击要成功的条件在于攻击者能够预测所有的参数从而构造出合法的请求。所以根据不可预测性原则，我们可以对参数进行加密从而防止CSRF攻击。
另一个更通用的做法是保持原有参数不变，另外添加一个参数Token，其值是随机的。这样攻击者因为不知道Token而无法构造出合法的请求进行攻击
Token 使用原则
Token要足够随机————只有这样才算不可预测
Token是一次性的，即每次请求成功后要更新Token————这样可以增加攻击难度，增加预测难度
Token要注意保密性————敏感操作使用post，防止Token出现在URL中.

如何防止token被盗用?
JWT 的机制.
https://www.jianshu.com/p/e0ac7c3067eb


2、XSS 攻击（跨域脚本攻击）
原理：向你的页面注入脚本.

攻击方式：
反射型：url 浏览器中.
存储型：数据库，内存.

防御措施：
1、编码. 
对用户输入的数据进行html Entity 编码.

2、过滤.
移除用户上传的DOM属性,如onerror等.
移除用户上传的Style节点，Script 节点, iframe节点.
body: display:none !important

3、校正.
避免直接对HTML Entity编码.
使用DOM Parse转换，校正不匹配的DOM标签.
