## 自定义脚手架
boi的脚手架围绕yeoman打造，你可以根据[yeoman的规范](http://yeoman.io/authoring/index.html)自定义脚手架模板。

如果想使用`boi new`命令创建项目，则必须对脚手架模板进行部分限制，具体表现为：
* 必须实现`--current`[选项](http://yeoman.io/authoring/user-interactions.html)。此选项的作用是指明是否在当前目录下创建项目；
* 必须实现`appname`[参数](http://yeoman.io/authoring/user-interactions.html)。此参数的作用是指定项目名称以及所创建的项目文件夹名称；
* 不支持appname参数以及`--current`选项以外的参数和选项。

> 更多细节可以参考Boi脚手架的默认模板[generator-boiapp](https://github.com/boijs/generator-boiapp)的源码。

如果想对项目模板进行更多的可定制化参数或选项，你可以选择不使用`boi new`命令创建项目，直接使用`yo`命令。这样就可以支持任意类型的yeoman项目模板。前提是必须在项目中包含`boi-conf.js`并且遵循Boi的配置规范才可在后续的流程中使用Boi。
