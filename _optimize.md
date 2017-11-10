## 构建性能优化
Boi集成Webpack过程中封装了一些性能相关的逻辑，包括但不限于以下几点：
* 通过[Rule.include](https://doc.webpack-china.org/configuration/module/#rule-include)限制参与编译的JavaScript文件目录；
* 使用`try{}catch{}`配合`require.resolve`取代`npm list`判断模块是否安装；
* 本地服务器监听文件中排除`node_modules`目录。

虽然部分优化手段牺牲了逻辑的严谨性，但在整体逻辑本身保证正确性的前提下使用一些hack方案不会影响Boi的功能。

除了Boi内部封装的优化手段以外，你也可以通过配置对性能进行优化和定制：
1. Boi默认使用[`loose`模式](_multipage-location.md)处理多页面项目的资源定位，这种模式是以牺牲准确度换取高性能。如果你的html文档中没有冲突文本，请保持使用；
2. 通过[`limit`配置](_config-basic.md)编译输出文件的体积上限，如果超出限制，Boi将会抛出警告，提醒你获取可以重新组织模块的划分结构。
