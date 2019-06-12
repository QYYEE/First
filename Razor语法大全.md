> Razor简介（学习来自于https://www.cnblogs.com/dengxinglin/p/3352078.html）
- Razor 是包含了模板引擎和动态编译两部分。
- Razor的发布是和MVC一起的，作为MVC的视图模板引擎。
> Razor文件类型
- Razor可以在vb.net和C#中使用。分别对应了两种文件类型，.vbhtml和.cshtml 

> Razor的标识符
- @字符被定义为Razor服务器代码块的标识符，后面的表示是服务器代码了。web form中使用<%%>中写服务器代码一个道理。在vs工具里面提供了代码着色和智能感应的功能。如下面代码：
  ```
  @{string userName= "祁洋洋";}
    <span>@userName</span>
    <span>@DateTime.Now.ToString("yyyy-MM-hh")</span>
  ```
> Razor的作用域
-  在上面一个例子中都已经使用到了大括号{}，不错，大括号里面的就是表示作用域的范围，用形如@{code}来写一段代码块.
```
@{
    string userName= "祁洋洋";
    @userName
}
```
- 在作用域(代码块)中输出也是用@符号的。



> 用Razor和html代码混合编写
- 在Razor中写html代码和html代码中写Razor语句都是可以的，并且还有智能提示。

  - a.在作用域内如果是以html标签开始则视为文本输出

  - b.如果要输出@，则使用@@

  - c.如果要输出非html标签和非Razor语句的代码，则用@:，他的作用是相当于在处于html下面编写一样了，如在@：后面可以加上@就是表示Razor语句的变量
```
@{

    var str = "abc";
    @: this is a mail：dxl0321@qq.com, this is var: @str,this is  mail@str,this is @@；
    @str 
}
```

> Razor作用块注释
- razor作用块里面本身就是服务器代码了，因此可使用服务器代码的注释，注释有//和/**/分别是单行注释和多行注释。

- 另外razor注释还可以使用自身特有的@* 注释的内容 *@，支持单行和多行的。


> Razor类型转换
  ```
   As系列扩展方法和Is系列扩展方法

          AsInt(), IsInt()

　    　AsBool(),IsBool()

    　　AsFloat(),IsFloat()

　   　AsDecimal(),IsDecimal()

　    　AsDateTime(),IsDateTime()

　  　ToString()
```
```
@{
    var i = “10”;
}
<p> i = @i.AsInt() </p> <!-- 输出 i = 10 --> 
```


>布局（Layout）
-  layout方式布局就是相当于一个模板一样的，我们在它地址地方去添加代码。相当于定义好了框架，作为一个母版页的，在它下面的页面需要修改不同代码的地方使用@RenderBody()方法
```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8"/>
        <title>我的网站 - @Page.Title</title>
    </head>
    <body>
        @RenderBody()
    </body>
</html>
```
```
@{
    Layout = "/LayoutPage.cshtml";
    Page.Title = "测试页面哦";
}

<p>This is a layout test</p>
```

> 页面（Page）
-  page是当需要在一个页面中，输出另外一个razor文件的内容时候用到，比如头部或者尾部这些公共的内容时候需要用到。输出就使用 @RenderPage()方法
```
如：A页面中也要把B页面的内容输出
A页面：

<p>
    @RenderPage("/b.cshtml")
</p>
<font color="red">这是一个子页面</font>
```

> Section区域
-  Section是定义在Layou的中使用的。在Layout的页面中用。在要Layout的父页面中使用@RenderSection("Section名称 ")定义：
```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8"/>
        <title>我的网站 - @Page.Title</title>
    </head>
    <body>
     @RenderSection("SubMenu")
        @RenderBody()
    </body>
</html>
在它的子页面中使用
@section SubMenu{
    Hello This is a section implement in About View.
 }
  如果在子页面中没有去实现了SubMenu了，则会抛出异常。我们可以它的重载@RenderSection("SubMenu", false)
   @if (IsSectionDefined("SubMenu"))
        {
            @RenderSection("SubMenu", false)
        }
        else
        {
            <p>SubMenu Section is not defined!</p>
        }
```

>  Helper
- helper就是可以定义可重复使用的帮助器方法，不仅可以在同一个页面不同地方使用，还可以在不同的页面使用.如在cshtml中那么写：
```
@helper sum(int a,int b)
{  
   var result=a+b;
　　@result  

}
<div >
    <p>@@helper的语法</p>    <p>2+3=@sum(2,3)</p> 
    <p>5+9=@sum(5,9)</p>
</div>
```