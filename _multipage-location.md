## 资源定位
boi处理多页面项目的资源定位问题有两种模式：
* `loose` - 宽松模式；
* `strict` - 严格模式。

默认使用`loose`模式，你可以通过配置html模块的[`staticLocateMode`](_config-html.md)选择其他模式。

### loose模式
loose模式解决资源定位的原理是使用正则表达式匹配html文件中的`<script>`和`<link>`标签。如果匹配到`src`为js主文件的`<script>`标签则将其`src`值替换为对应js文件编译后的地址；如果匹配到`href`为css主文件的`<link>`标签则将其`href`值替换为对应css文件编译后的地址。

loose模式的优点是正则表达式的匹配速度非常快。缺点是：
* 依赖boi中对js和style主文件的命名规范；
* 不能达到100%准确。举个例子，请看下述html源码：
```html
<!-- index.a.html -->
<html>
  <head>
    <title>webapp</title>
    <link href="main.a.css" rel="stylesheet"></link>
  </head>
  <body>
    <div id="app"></div>
    <script type="text/javascript" src="main.a.js"></script>
    <script type="text/javascript">
      console.log("<script type='text/javascript' src='main.a.js'></script>");
    </script>
  </body>
</html>
```

宽松模式下的编译输出结果如下：
```html
<!-- index.a.html -->
<html>
  <head>
    <title>webapp</title>
    <link href="//test.app.com/webapp/style/main.a.7sfsdf9d.css" rel="stylesheet"></link>
  </head>
  <body>
    <div id="app"></div>
    <script type="text/javascript" src="//test.app.com/webapp/js/main.a.7sdfw12.js"></script>
    <script type="text/javascript">
      console.log("<script type='text/javascript' src='//test.app.com/webapp/js/main.a.7sdfw12.js'></script>");
    </script>
  </body>
</html>
```

也就是说，宽松模式不能够完全区分目标是规范的`<script>`标签还是文本。你也许怀疑是因为正则表达式写的不够精确，但是文本的内容多种多样，无法完全确定其前后边界。加上html文档的高容错性，所以并不存在完全精确的正则表达式。

如果你确定项目中的html文档中不存在如上例所述的场景，可以放心地使用宽松模式。幸运的是，目前前端项目的趋势是重逻辑轻view，大部分项目的html文件只是一个壳子，这也是boi默认使用loose模式的原因。

### strict模式

严格模式的工作原理是使用[parse5](https://github.com/inikulin/parse5)将html文件解析成json格式的树形结构。分离出script和link类型的节点，替换匹配的`src`和`href`属性值。最后重新组合整个nodeTree并将其序列化为规范的html文档。

严格模式的资源定位能够提供100%准确的匹配度，你不用担心上文例子中出现的问题。缺点也同样明显：构建速度慢。因为html文档要依次经过解析、寻址、替换、重组和序列化，尤其是寻址要花费大量的时间。我们在寻址的循环算法上做了基本的优化，可以参考[源码](https://github.com/boijs/html-webpack-plugin-replaceurl/blob/master/index.js)。

对比两种模式如表格所示：

|模式|构建速度|准确度|备注|
|:--:|:--:|:--:|:--:|
|loose|快|不够准确|默认|
|stirct|慢|100%准确|-|

可以根据项目具体需求选择合适的模式。
