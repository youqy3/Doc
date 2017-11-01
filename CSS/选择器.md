## 元素选择器
- 在进行设计时，为了消除浏览器之间的显示差异，通常将html元素reset一下


```
body,div,span,object,frame,h1,h2,h3,h4,h5,h6,p,
blockquote,a,code,em,img,q,small,strong,dd,dl,dt,
li,ol,ul,fieldset,form,label,table,tbody,tr,th,td,
input,menu,button { margin:0; padding:0; }

ul, ol { list-style-type: none; }
```

## 通配选择器(*)
- 通配选择器对页面中的所有元素都起作用

```
//使页面中所有元素的字体都为红色
* { color: red;}
```

## 类选择器和ID选择器
- 元素选择器会使页面中所有的元素都应用该样式
- 类选择器和ID选择器则用来个性化某个元素的样式

```
//ID选择器
#ID {}  

//类选择器
.class {}

//类选择器与元素结合使用
//只有类名为warning的p元素才会应用该样式，仅使用类名warning的其他元素不会应用该样式
//适用范围：可定义一个通用的.warning类使字体斜体显示，当碰到p元素并包含warning类时，加粗显示
p.warning {font-weight: bold}

html
	<div class="warning">
		<p>Hello world</p>  //字体不会加粗
	</div>
	<p class="warning">new hello world</p>  //字体加粗显示

//类选择器与类选择器结合适用
//只有在元素同时应用.warning类和.show类时，元素内字体才为红色，单独应用.warning或.show时，元素内字体不会变为红色
//类似与类选择与元素的结合
.warning.show{ color: red; }
```


## 属性选择器
1. 简单属性选择器：用于选择拥有某个属性的元素，而不管该属性的值是什么

```
//选择有class属性(值不限)的所有h1元素，使其文本为银色
h1[class]{color: silver;}

//上述定义的属性选择器，对下面的所有元素都起作用
<h1 class="hoopla">Hello</h1>
<h1 class="severe">Serenity</h1>
<h1 class=fancy">Fooling</h1>

//多属性选择器组合使用
a[href][title] {font-weight:bold}  //将同时有href和title属性的html超链接的文本设置为粗体
```

2. 根据具体属性值选择

```
//只将href值为http://www.baidu.com的超链接元素文本设置为粗体
a[href="http://www.baidu.com"]{font-weight: bolid;}

//这种格式要求必须与属性值完全匹配
<p class="class1 class2">Hello World</p>
//要匹配上面的元素，class的值必须为"class1 class2",class="class1"或class="class2"都不能匹配
p[class="class1 class2"]{font-weight: bolid;}
```

3. 根据部分属性值选择：解决了根据具体属性值选择时属性的值必须要与元素的属性值完全一致
```
//部分属性值选择器只要有class包含warning类的p元素都会器作用
p[class~="warning"]{font-weight: bolid;} === p.warning{font-weight: bolid;}

<p class="urgent warning">Hello World</p>

//为什么需要部分属性值选择？
因为这个规则能作用于任何属性，而不只是class
img[title~="Figure"] {border: 1px solid gray;} //作用于所有title中包含Figure值的img元素
```

4. 子串匹配属性选择器：类似与部分属性值选择(采用正则来进行匹配)

```
*[foo^="bar"]   //选择foo属性值以"bar"开头的所有元素
*[foo$="bar"]   //选择foo属性值以"bar"结尾的所有元素
*[foo*="bar"]   //选择foo属性值中包含子串"bar"的所有元素
```

5. 特定属性选择类型:[att|="var"](选择所有属性att等于var或以var开头的所有元素)
```
//选择lang属性等于en或以en-开头的所有元素
*[lang|="en"]{color: white;}

<h1 lang="en">Hello</h1>   //被选中
<p lang="en-us">Greetings</p>   //被选中
<p lang="fr">World</p>   //未被选中
<h1 lang="cy-en">Jrooana</h1>   //未被选中

```


## 后代选择器：用空格间隔两个元素/类
- 后代选择器就是来创建一些规则，它们仅在某些结构中起作用，而在另外一些结构中不起作用

```
//使从h1元素继承的em元素文本变为灰色
//后代选择器不仅仅只作用于直接子元素，而是会作用于所有后代元素（即两个元素之间的层次间隔可以是无限的）
h1 em {color: gray;}

<h1>Hello <em>World</em></h1>      //起作用，em元素中的文本变成灰色
<h1><span>Hello <em>World</em></span></h1>   //仍然起作用，em元素中的文本变成灰色
```


## 选择子元素：用>间隔两个元素
- 后代选择器会选择一个任意的后代元素，但有时候为了缩小范围，我们只选择一个元素的子元素
- 子选择器限制为只匹配结构树中直接相连的元素

```
h1 > em {color: gray;}

<h1>Hello <em>World</em></h1>      //起作用，em元素中的文本变成灰色
<h1><span>Hello <em>World</em></span></h1>   //不起作用，em元素不是h1元素的子元素，而只是h1的后代元素
```

## 选择相邻兄弟元素：用+间隔两个元素


