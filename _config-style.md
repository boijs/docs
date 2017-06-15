## Style编译配置

Style编译配置使用`boi.spec`API：

```JavaScript
boi.spec('style',<options>);
```

第一个参数代表需要配置的模块，Style对应value为`style`。

`options`包括以下可配置项：

* `ext`：`String`，源文件扩展类型，默认为`css`。boi会根据`ext`类型自动分配对应的编译方案。boi默认支持以下预编译类型：
  * `css`
  * `less`
  * `scss`

> 其他预编译类型请自行[编写boi插件](_advance-plugin.md)实现。

* `source`：`String`，style源文件的二级目录名称，相对于`[basic](_config-basic.md).source`。默认为`style`；

* `output`：`String`，编译输出style文件的二级目录名称，相对于`[basic](_config-basic.md).output`。默认为`style`；

* `mainFilePrefix`：`String`，style主文件的命名前缀，默认为`main`。也就是说，默认的style主文件名为`main.[name].[extType]`；

* `useHash`：`Boolean`，编译输出的css文件名是否加上hash指纹，默认为`true`。也就是说，默认的编译输出js文件名为`main.[name].[hash].css`;

* `autoprefix`：`Boolean`，是否自动补全前缀，默认为`false`。
> boi使用postcss实现自动补全前缀，但是会引起一些**中间版本**浏览器无法兼容的问题。比如，flex布局在IOS8的Safari下的写法应该是`display:-webkit-box-flex`，而大多数编译工具的自动补全会将其修复为`display: -webkit-flex`。所以，我们推荐不使用编译工具的自动补全功能，而是由开发人员编写预编译mixin实现hack兼容性。

* `sprites`：`false|Object`，配置CSS Sprites自动生成功能。取值为`false`时关闭此功能；取值为`Object`时有以下子配置项：
  * `source`：`String`，指定散列图标的目录。取值可以是项目目录内的路径，比如`./assets/icons`。也可以是文件夹的名称，比如`icons`。生成的Sprites图片输出的目录是[image](_config-image.md)模块指定的`output`;
  * `split`：`Boolean`，是否根据子目录分别编译输出。如果配置`sprites.split`为`true`，boi会根据子目录将散列图标单独打包。比如`icons`内有两个子目录`home`和`auth`，子目录内分别是两个页面对应的散列icon。boi会编译输出`sprites.<appname>.home.png`和`sprites.<appname>.auth.png`两个雪碧图。其中`appname`是通过[basic配置](_config-basic.md)指定；
  * `retina`：`Boolean`，是否识别多分辨率标识并生成多分辨率sprites图片。如果设置为`true`，boi可识别以`@2x`后缀命名的图片并单独打包。更多细节请参考[postcss-sprite](https://github.com/2createStudio/postcss-sprites)文档；
  * `postcssSpritesOpts`：`Object`，boi的自动生成sprites图片功能由[postcss-sprites](https://github.com/2createStudio/postcss-sprites)实现，此配置可以允许你直接配置。

  > 关于css sprites的实现方案可以参考这篇文章[基于webpack的css sprites实现方案](http://www.caiziguoguo.com/cj21j94e5000uwj0hlbvlisk6/)。

### 示例
```JavaScript
boi.spec('style', {
    // Style文件后缀类型
    ext: 'scss',
    // Style文件目录，相对于basic.source
    source: 'style',
    // Style文件输出目录，相对于basic.output
    output: 'style',
    // 使用hash
    useHash: true,
    // 是否自动补全hack前缀
    autoprefix: false,
    // 主文件前缀
    mainFilePrefix: 'main',
    // 是否启用CSS Sprites自动生成功能
    sprites: {
        // 散列图片目录
        source: 'icons',
        // 是否根据子目录分别编译输出
        split: true,
        // 是否识别retina命名标识
        retina: true,
        // 自行配置postcss-sprite编译配置
        // @see https://github.com/2createStudio/postcss-sprites
        postcssSpritesOpts: null
    }
});
```