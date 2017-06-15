## Html编译配置

Html编译配置使用`boi.spec`API：

```JavaScript
boi.spec('html',<options>);
```

第一个参数代表需要配置的模块，Html对应value为`html`。

`options`包括以下可配置项：

* `ext`：`String`，源文件扩展类型，默认为`html`；

* `engine`： `String`，指定模块引擎类型。boi封装了Html、Jade、Swig、Ejs、Handlebar对应的编译方案，此列表之外的模板引擎需自行[编写boi插件](_advance-plugin.md)实现；

* `source`：`String`，源码html文件目录，相对于`[basic](_config-basic.md).source`。默认为`views`；

* `output`：`String`，编译输出html文件目录，相对于`[basic](_config-basic.md).output`。默认为`views`；

* `mainFilePrefix`：`String`，html文件的命名前缀，默认为`index`。也就是说，默认的js主文件名为`index.[name].html`；

* `buildToHtml`：`Boolean`，是否将html模板引擎语法编译为html语法，默认为`true`；

* `staticLocateMode`：`String`，资源定位模式，可取值`"strict"|"loose"`，默认为`"loose"`。此配置是针对存在多个html入口并且每个html所需的静态资源不同的多页面项目。更多细节请参阅[多页面开发](_multipage.md);

> 此配置项在单页面项目中无效。

* `favicon`：`String`，为项目指定favicon，取值为相对于项目根目录的相对地址，默认为`null`。如果指定正确的favicon路径，boi会将favicon进行编译并将其引用地址自动注入到html文件中。favicon编译输出的本地路径为`[image](_config-image.md).output`指定目录的`/favicon`子目录；

* `urlTimestamp`：`Boolean`，是否在静态资源url后加上时间戳（比如`//static.app.com/common.js?t=1476183177875`），默认为`false`。此配置项是处理浏览器缓存的一种方案，理想方案是使用文件hash指纹，而不是url query。之所以有此配置项是为了配合仍然使用url query作为静态资源更新方案的团队。

### 示例
```JavaScript
boi.spec('html', {
    // 源文件后缀类型，默认为html
    ext: 'html',
    // 模板引擎，默认为html
    engine: 'html',
    // 如果采用模板引擎，此配置项为true时将模板语法编译为HTML语法，false时保留原语法
    buildToHtml: true,
    // Html文件目录，相对于basic.source
    source: './',
    // Html文件编译输出目录，相对于basic.output
    output: './',
    // 模板入口文件的前缀，入口文件的命名规则为[mainFilePrefix].*.[ext]
    mainFilePrefix: 'index',
    // 静态资源js&css的url是否加上query时间戳
    urlTimestamp: false,
    // favicon路径
    favicon: null,
    // 指定需要编译的模板文件
    // 如果用户不配置，则boi将匹配模板目录下的所有符合命名规范的模板文件
    // files: [],
});
```
