## cookie对象使用举例

### 设置cookie

每个cookie以一个用等号分隔开键和值的字符串，设置时赋值给document.cookie，一次设置多个用分号加空格隔开，每次赋值不会丢失原来的值，而是添加到已存在的cookie字符串，如果要改变一个cookie的值，只需重新赋值
> 注意，cookie键或值中不能使用分号、逗号、等号以及空格，必要时可用`escape()`函数进行编码，它能将一些特殊符号使用十六进制表示，取值后需要使用`unescape()`进行解码

```js
document.cookie="userId=828";
document.cookie="userId=828; userName=hulk";
document.cookie="str="+escape("I love ajax");//相当于document.cookie="str=I%20love%20ajax";
```

### 获取cookie的值

cookie的值由document.cookie直接获得，返回值为以分号隔开的多个键/值对所组成的字符串，这些键/值对包括了该域名下的所有cookie。用户必须自己分析这个字符串，来获取指定的cookie值

```js
document.cookie="userId=828";
document.cookie="userName=hulk";
var strcookie=document.cookie;
var arrcookie=strcookie.split("; ");
var userId;
for(var i=0;i<arrcookie.length;i++){
      var arr=arrcookie[i].split("=");
      if("userId"==arr[0]){
             userId=arr[1];
             break;
      }
}
alert(userId);
```
### 给cookie设置终止日期

一般情况下cookie都是单会话cookie，即浏览器关闭后这些cookie将会丢失，事实上这些cookie仅仅是存储在内存中，而没有建立相应的硬盘文件。如需长期保存，可用expires指定过期时间

```js
document.cookie="userId=828; expires=GMT_String";//GMT_String是以GMT格式表示的时间字符串
var date=new Date();
var expireDays=10;
date.setTime(date.getTime()+expireDays*24*3600*1000);//将date设置为10天以后的时间，最终cookie为十天后过期
document.cookie="userId=828; userName=hulk; expire="+date.toGMTString();
```

### 删除cookie

删除一个cookie，可以将其过期时间设定为一个过去的时间，

```js
var date=new Date();
date.setTime(date.getTime()-10000);
document.cookie="userId=828; expire="+date.toGMTString();
```

### 指定可访问cookie的路径

默认情况下，如果在某个页面创建了一个cookie，那么该页面所在目录中的其他页面也可以访问该cookie。如果这个目录下还有子目录，则在子目录中也可以访问，如果要控制cookie可以访问的目录，需要使用path参数设置cookie，

```js
document.cookie="userId=320; path=/shop";//只在shop目录下可用
document.cookie="userId=320; path=/";//整个网站可用
```

### 指定可访问cookie的主机名

主机名是指同一个域下的不同主机，例如：www.google.com 和 gmail.google.com就是两个不同的主机名。默认情况下，一个主机中创建的cookie在另一个主机下是不能被访问的，可以通过domain参数来实现对其的控制

```js
document.cookie="name=value;domain=.google.com";//所有google.com下的主机都可以访问该cookie
```