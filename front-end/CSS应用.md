# 文字段落

## 分析

**新闻页面** 分为  图片、标题字、正文 几部分

DIV标签是用来HTML文档内==大块内容==提供结构和背景的元素

分别建立 .container | .content | .source 类选择器

## 源码

1.页面基本结构，内容基本布局

```html
<!DOCTYPE html>
<html>
<head>
	<style type="text/css">
		.container{width:750px;margin:0 auto;text-align:center;backgroun-color:#fff;padding:30px;}
	</style>
</head>
<body>
	<!-- 区隔标记 -->
	<div class="container">
		<!-- 设定文字、图片、表格等的摆放位置 -->
	</div>
</body>
</html>
```

2.添加CSS样式设置

```html
<!DOCTYPE html>
<html>
<head>
	<meta >
	<title>标题</title>
	<!-- CSS样式设置 -->
	<style type="text/css">
		.container{width:750px;margin:0 auto;text-align:center;backgroun-color:#fff;padding:30px;}
		h1{font-family:黑体;font-size:24px;color:#059;letter-spacing:12px;}
		.source{font-family:宋体;color:black;font-size:14px;}
		.content{font-size:14px;}
		p{text-align:left;font-size:14px;text-indent:2em;}
	</style>
</head>
<body>
	<!-- 区隔标记 -->
	<div class="container">
		<!-- 设定文字、图片、表格等的摆放位置 -->
		<img src="imgs/news.png" width=750 height=450>
		<h1> <i>一级标题</i></h1>
			<div class="source"><u><a href="https://news.cctv.com/">央视网</a></u>| 2022.04.26 13:32 | 来源：央视网</div>
			<div class="content">
				<p>前言</p>
				<p>主要内容<font size="4px" color="red">>></font></p>
				<p>1.不负韶华 国聘行动”重庆两江新区智能制造专场启动</p>
				<p>...</p>
				<p>2.焦点访谈：我们村的新能人 “慧”种田 “智”富路</p>
				<p>...</p>
				<p>3.中国驻巴使馆工作人员证实：卡拉奇爆炸案造成3名中国公民遇难</p>
				<p>...</p>
			</div>
	</div>
</body>
</html>
```

![image-20220426225553153](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220426225553153.png)





# 列表

**导航条**



## 框架

```html
<!DOCTYPE html>
<html>
<head>
	<title>标题</title>
	<style type="text/css">
		.container{width:650px;margin:0 auto;text-align:center;backgroun-color:#fff;padding:20px;}
	</style>
</head>
<body>
	<div class="container">
		<ul id="nav">
			<li class="home">1</li>
			<li class="activity">2</li>
			<li class="personal">3</li>
		</ul>
	</div>
</body>
</html>
```

## 添加CSS样式

```html
<!DOCTYPE html>
<html>
<head>
	<title>标题</title>
	<style type="text/css">
		.container{width:650px;margin:0 auto;text-align:center;backgroun-color:#fff;padding:20px;}
		#nav{list-style:none;font-size:22px;line-height:40px;}
		.home{border-top:4px solid #7bc110;background:#be6;}
		.activity{border-top:4px solid #ff9900;background:#fc3;}
		.personal{border-top:4px solid #ff66ff;background:#fcf;}
		#nav li{margin-right:5px;float:left;width:100px;}
	</style>
</head>
<body>
	<div class="container">
		<ul id="nav">
			<li class="home">1</li>
			<li class="activity">2</li>
			<li class="personal">3</li>
		</ul>
	</div>
</body>
</html>

<!-- 

#nav li{margin-top:5px;} 列表间距 (行)

#nav li{margin-right:5px;float:left;width:100px;}  （列）

<li> 添加 float:left 将导航条垂直变水平

-->
```

![image-20220427102916092](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220427102916092.png)



![image-20220427102925254](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220427102925254.png)



# 超链接

相册

## 框架

```html
<!DOCTYPE html>
<html>
<head>
	<title>album</title>
	<style type="text/css">
		.container{width:400px;margin:0 auto;text-align:center;background-color:#fff;padding:20px;}

	</style>
</head>
<body>
	<div class="container">
		<ul>
			<li><img src="imgs/album01.png" width="45" height="52"/>1</li>
			<li><img src="imgs/album02.png" width="45" height="52"/>2</li>
			<li><img src="imgs/album03.png" width="45" height="52"/>3</li>
			<li><img src="imgs/album04.png" width="45" height="52"/>4</li>
			<li><img src="imgs/album05.png" width="45" height="52"/>5</li>
			<li><img src="imgs/album06.png" width="45" height="52"/>6</li>
			<li><img src="imgs/album07.png" width="45" height="52"/>7</li>

		</ul>
	</div>
</body>
</html>
```

## CSS样式

```html
<!DOCTYPE html>
<html>
<head>
	<title>album</title>
	<style type="text/css">
		.container{width:240px;margin:0 auto;text-align:center;background-color:#fff;padding:20px;}
		#album{list-style:none;font-size:12px;line-height:1.5;}
		#album li{float:left;width:45px;margin:10px;}
		img{border:0;}
		a:link{color:#33333;text-decoration:none;}
		a:visited{color:#333333;text-decoration:none;}
		a:hover{color:#ff0000;text-decoration:underline;}
		a:active{color:#ff0000;text-decoration:underline;}
	</style>
</head>
<body>
	<div class="container">
		<ul id="album">
			<li><a href="imgs/pages/album01.html" target="_blank"><img src="imgs/album01.png" width="45" height="52"/>Armin</a></li>
			<li><a href="imgs/pages/album02.html" target="_blank"><img src="imgs/album02.jpg" width="45" height="52"/>Anine</a></li>
			<li><a href="imgs/pages/album03.html" target="_blank"><img src="imgs/album03.png" width="45" height="52"/>Eren</a></li>
			<li><a href="imgs/pages/album04.html" target="_blank"><img src="imgs/album04.png" width="45" height="52"/>Levi</a></li>
			<li><a href="imgs/pages/album05.html" target="_blank"><img src="imgs/album05.png" width="45" height="52"/>Mikasa</a></li>
			<li><a href="imgs/pages/album06.html" target="_blank"><img src="imgs/album06.png" width="45" height="52"/>Historia</a></li>
			<li><a href="imgs/pages/album07.html" target="_blank"><img src="imgs/album07.png" width="45" height="52"/>Ymir</a></li>

		</ul>
	</div>
</body>
</html>

<!--

<a href="imgs/pages/album01.html" target="_blank">
	<img src="imgs/album01.png" width="45" height="52"/>Armin
</a>

点击图片或者文字，跳转到新界面 ( href ) 

-->
```

```html
<!-- 跳转示例界面-->
<!DOCTYPE html>
<html>
<head>
</head>
<body>
<center>
   <img src="E:/Front/imgs/album01.png" width="441" height="548">
</center>
</body>
</html>



<!-- 

<img src="E:/Front/imgs/album01.png" width="441" height="548">

-->
```

![image-20220427121432189](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220427121432189.png)

![image-20220427230133134](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220427230133134.png)

![image-20220427230158976](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220427230158976.png)

# 表格

产品介绍

## 框架

```html
<!DOCTYPE html>
<html>
<head>
	<title>1</title>
	<style type="text/css">
	</style>
</head>
<body>
	<table>
		<tr>
			<td><img src="" width="144" height="129"/></td>
			<td>
				<p class="productName">1</p>
				<p class="paraValue">1<br>2<br></p>
			</td>
		</tr>
		<tr><td colspan="2">1</td></tr>
		<tr><td colspan="2">
			<table>
				<tr><td>1</td></tr>
				<tr><td>1</td></tr>
				<tr><td>1</td></tr>
				<tr><td>1</td></tr>
				<tr><td>1</td></tr>
			</table>
		</td></tr>
		<tr><td colspan="2">1</td></tr>
		<tr><td colspan="2">1</td></tr>
		<tr><td colspan="2">1</td></tr>
		<tr><td colspan="2">1</td></tr>
	</table>
</body>
</html>
```



## 添加CSS样式

```html
<html>
<head>
<title>产品介绍</title>
<style type="text/css">
	.product{width:500px;border: 1px solid #FF6600;border-collapse: collapse;}
	.product td {border: 1px solid #FF6600;}
	.productName{font-size: 14px;	color: #993300;	font-weight: bold;}
	.category{font-size:14px;color:#993300;font-weight:bold;background-color:#FFB468;}
	.productImg{text-align:center;vertical-align:middle;width:170px;}
	.productDescription{background-color:#FEE8AB}
	.paraTable td {border-style:none;}
	.paraName{font-size: 13px;color: #666666;text-align:right;vertical-align:top;width:70px;}
	.paraValue{font-size: 13px;text-align:left;padding-left:5px;}
</style>
</head>
<body>
<table class="product">
	<tr><td class="productImg"><img src="imgs/041.jpg" width="144" height="129" /></td>
			<td  class="productDescription">
	      <p class="productName">产品名称：佳能 IXUS 130</p>
 				<p class="paraValue">佳能 IXUS 130（官方标配）<BR>
             松下原装SD卡2G（高速正品）<BR>
             佳能IXUS系列专用皮包<BR>
             索尼2.7英寸LCD保护贴（防刮/高透光/静电吸附)<BR>
             摄影指南</p>		
			</td>
	</tr>
	<tr class="category"><td colspan="2">主要参数</td></tr>
	<tr><td  colspan="2">
		<table class="paraTable">
			<tr><td class="paraName">型号:</td><td class="paraValue">PMP169B</td></tr>
			<tr><td class="paraName">内存容量:</td><td  class="paraValue">512M</td></tr>
			<tr><td class="paraName">屏幕尺寸:</td><td  class="paraValue">2.12英寸（最佳视觉比列16：9的宽屏）</td></tr>
			<tr><td class="paraName">屏幕特性:</td><td  class="paraValue">LTPS TFT   (720x240)</td></tr>
			<tr><td class="paraName">视频功能:</td><td  class="paraValue">支持ASF格式的MPG4,或通过软件转换成ASF播放,播放效果:320×420,
30fps/视频输出,输入/电视节目定时录制。</td></tr>
		</table>
	</td></tr>	
	<tr  class="category"><td  colspan="2">功能参数</td></tr>
	<tr><td  colspan="2">
		<table class="paraTable">
			<tr><td class="paraName">音频功能:</td><td class="paraValue">支持音频格式:MP3,WMA,WAV/内置麦克风,支持LINEIN寻录/收音功能/内置喇叭</td></tr>
			<tr><td class="paraName">录音格式:</td><td  class="paraValue">44.1KHz, 128 Kbps, MP3, 支持MIC/LINE-IN直录</td></tr>
			<tr><td class="paraName">附加功能:</td><td  class="paraValue">支持图片格式:JPEG(EXIF2.1)/电子书浏览/多语言(中/英)设置</td></tr>
		</table>			
	</td></tr>
	<tr  class="category"><td  colspan="2">其它参数</td></tr>
	<tr><td  colspan="2">
		<table class="paraTable">
			<tr><td class="paraName">接口:</td><td class="paraValue">USB接口,AV OUT接口, AV IN接口</td></tr>
			<tr><td class="paraName">扩展卡:</td><td  class="paraValue">可扩充2G SD,MMC卡</td></tr>
			<tr><td class="paraName">电池:</td><td  class="paraValue">内置锂电池</td></tr>
			<tr><td class="paraName">尺寸:</td><td  class="paraValue">105.2 x47.6x15.6mm</td></tr>
			<tr><td class="paraName">重量:</td><td  class="paraValue">90g</td></tr>
		</table>			
		
		</td></tr>
</table>
</body>
</html>


<!--

product, 
	category,
		paraTable, paraName, paraValue, productDescription



-->
```

![image-20220427230112992](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220427230112992.png)

# 表单

在线界面

## 框架

```html
<!DOCTYPE HTML>

   <TITLE>酒店预订</TITLE>            
<META http-equiv="Content-Type" content="text/html; charset=gb2312">
<link rel="stylesheet" type="text/css" href="css/html.css"/>
     
</HEAD>     
<BODY>
<DIV id="content"><!-- 主体内容 -->                 
	<DIV class="tip">
	<P>现在&nbsp;<A href="">登录</A>&nbsp;即可预订可享酒店会员专属优惠，并可直接使用常用预订信息</P>
	</DIV>
 <form id="room" action="" method="post">
	
<DIV class="order">
		<LABEL>入离日期</LABEL>                     
		<INPUT name="inDate" type="date"><SPAN>至</SPAN><INPUT name="outDate" type="date"> 
	</DIV>
	<DIV class="order">
		<LABEL>房间数量</LABEL>                         
		<INPUT type="number" min="1" value="1"> 
		<SPAN>房费</SPAN>￥<STRONG>XXX</STRONG>         
	</DIV>
	<DIV class="order">
		<LABEL>住客姓名</LABEL>                         
		<INPUT type="text" placeholder="住客（中文/英文）（必填）" value=""> 
		<span>请填写实际入住人姓名，每个房间只需填1位。</span>
	</DIV>
	<DIV class="order">
		<LABEL>联系手机</LABEL>                     
		<INPUT type="tel" placeholder="联系人手机号码（必填）" value="">                            
		<SPAN>预订成功后会向您发送短信通知</SPAN>                 
	</DIV>
	<DIV class="order">
		<LABEL>邮箱</LABEL>                     
		<INPUT type="email" placeholder="选填" value="">                     
		<SPAN>接收确认邮件</SPAN>                 
	</DIV>
	<DIV class="order">
		<LABEL>最晚抵店</LABEL>                     
		<SELECT name="select" autocomplete="off">
			<OPTION selected="selected" value="18:00">18:00</OPTION>    
			<OPTION value="19:00">19:00</OPTION>                      
			<OPTION value="20:00">20:00</OPTION>                         
			<OPTION value="21:00">21:00</OPTION> 
			<OPTION value="22:00">22:00</OPTION>                   
			<OPTION value="23:00">23:00</OPTION>                         
			<OPTION value="24:00">24:00</OPTION>                         
			<OPTION value="次日6:00">次日6:00</OPTION>                     
		</SELECT>                     
	<SPAN>该酒店14:00办理入住，早到可能需要等待。</SPAN>                 
	</DIV>
	<DIV>
		<span>酒店前台付款￥ </span> 
		<SPAN style="color: rgb(255, 102, 0);">XXX</SPAN>						 
	    <DIV class="submit">提交订单</DIV>
	</DIV>
 
</form>
</DIV>
</BODY>
</HTML>

```

## 添加CSS样式

```css
/* css/hmtl.css*/
body{
	color:#333;
	font-size:14px;
	line-height:1.5;
	background-color:#fff;
	}
p{
	margin:0;
	padding:0
}
.content{
	margin:0 auto;
	width:600px;
	border:1px solid red;
	
	}
.tip{
	background-color:#fefaf1; 
	border:solid 1px #fed698;
	padding:8px 8px 8px 32px;
	}
.order{
	height:26px;
	margin:7px  0;
	}
label{
	float:left;
	width:60px;
	text-align:right;
	}

.submit{
	background-color:#f90;
	display:inline-block;
	border-radius:3px;
	color:#fff;
	cursor:pointer;
	font-weight:bold;
	padding:6px 22px
	}

#content{
	margin:10px;
	width:600px;
	height:300px;
	border:1px solid red;
	}

input,select{
	margin-right:4px;
	margin-left:10px;
	float:left;
	width:170px
	}
span{
	float:left;
	margin-right:4px;
	}
	
	input:focus,input:hover, select:focus,select:hover{ color: #000;  background: #E7F1F3; border: 1px solid #888;}

```

![image-20220427230054016](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220427230054016.png)
