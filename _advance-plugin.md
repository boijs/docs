## 编写boi插件

如果[直接配置webpack](_advance-config.md)仍然难以满足业务需求，boi还提供了插件系统来配置更复杂的需求。

### 构造函数`boi.PluginClass.loader`

`boi.PluginClass.loader`是编写boi *loader* 类型插件的构造函数。所谓loader类型插件，顾名思义，就是对构建功能进行更细致的配置，也就是对webpack进行配置。使用语法如下：
```JavaScript
new boi.PluginClass.loader([pattern,]options);
```

* `pattern`：`String`，指定插件针对的模块类型，可取值为`js/style/html/image/external`。可以不指定，默认为`external`。**请注意**，插件配置比boi内置配置由更高优先级，如果将`pattern`指定为`js/style/html/image`中的任意一项，则会覆盖boi内置的所有相关类型的配置。比如指定`pattern`为`js`，则boi封装的所有相关js的配置项均被插件覆盖。所以，如果你仍然想使用boi内置配置，建议将`pattern`指定为`external`；
* `options`：`Object`，webpack配置项。通过插件配置webpack没有`boi.spec`API[直接配置webpack](_advance-config.md)的限制，你可以通过插件配置所有的webpack选型，详情见[webpack文档](https://doc.webpack-china.org/configuration/)。

### 示例

下面是截取boi编译vue文件插件[boi-plugin-loader-vue](https://github.com/boijs/boi-plugin-loader-vue)代码中的一部分：

```JavaScript
const ClassLoader = boi.PluginClass.loader;

new ClassLoader('external', {
  module: {
    rules: [{
      test: /\.vue$/,
      use: [{
        loader: 'vue-loader',
        options: {
          loaders: GetLoaders()
        }
      }]
    }]
  },
  plugins: globalOpts.style.extract ? [ExtractPlugin] : [],
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js'
    }
  }
});
```

发布你的插件，按照[使用boi插件](_config-plugin.md)文档，在`boi-conf.js`中引入你的插件即可：

```JavaScript
boi.use('boi-plugin-loader-vue', {
  style: {
    output: 'style',
    useHash: true,
    extract: false,
    autoprefixer: false
  }
});
```
### 编写可配置的插件

前文的`boi.use`API使用`'boi-plugin-loader-vue'`时传入了一系列配置项，这些配置项不是针对boi的，而是插件`'boi-plugin-loader-vue'`暴露出来的。

如果插件本身也是可配置的，那么在编写插件时，可以将插件编写为`function`类型，如下：

```JavaScript
module.exports = function(options){};
```

上述代码中函数的参数`options`即可接受的配置项集合。
