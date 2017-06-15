## 基础信息配置

基础信息配置使用`boi.spec`API：

```JavaScript
boi.spec('basic',options);
```

第一个参数代表需要配置的模块，基础信息对应值为`basic`。

`options`包括以下可配置项：

* `appname`：`String`，项目名称。如果使用[boi脚手架](_start-scaffold.md)可以在创建项目时指定。`appname`将影响[模块化开发](_modules.md)中异步模块打包文件的命名；
* `source`：`String`，源文件所在一级目录，默认为`src`；
* `output`：`String`，编译输出文件一级目录，默认为`dest`；
* `libs`：`String`，第三方js库、css库文件所在的一级目录，默认为`libs`。
* `deployLibs`：`Boolean`，是否部署`libs`文件，默认为`false`。

> 需要说明的是，`libs`配置项只是为了**临时存放**本地的第三方库文件。boi对于`libs`的定义是：不使用npm管理、由单独的`<script>`或`<link>`标签引入的文件，比如`jQuery.js`和`normalize.css`等。我们认为这类文件应该是全站通用的，应该使用全站统一的CDN路径（比如`//static.boi.com/common/jquery.js`）引入。所以本地的libs文件并不会参与编译，默认也**不会被部署**，只是作为**临时文件**使用。如果你想将libs文件一同部署，可以将`deployLibs`设为`true`。

* `checkDependencies`：`Boolean`，编译之前是否自动检查并安装`package.json`中的`depdencies`和`devDependencies`模块，默认为`false`。此配置项的主要目的是为了将boi执行云编译时自动安装依赖模块。**如果你确定不会将boi作为云编译工具，请设置为`false`**，会大幅提升编译速度。

### 示例

```JavaScript
boi.spec('basic', {
  appname: 'boi-webapp',
  source: './src/',
  output: './dest/',
  libs: './libs/',
  deployLibs: false,
  checkDependencies: false
});
```
