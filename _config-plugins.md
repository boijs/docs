## 使用插件

`boi.spec`API只支持常规的几个模块：js/style/html/image。遇到比较复杂的编译需求，比如编译react或者vue文件，编译本身需要非常复杂的配置。这种场景下，`boi.spec`API就显得捉襟见肘了。

boi插件就是为了弥补`boi.spec`的不足，你可以使用第三方boi插件，也可以根据自身的需求[编写boi插件](_advance-plugin.md)。

使用boi插件的API是`boi.use(name,options)`，比如：

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

上述代码使用了[boi-plugin-loader-vue](https://github.com/boijs/boi-plugin-loader-vue)插件，这个插件的作用是编译`.vue`后缀的文件。第二个参数`options`是由插件本身提供的可配置项。

需要注意的是，**插件比`boi.spec`配置项有更高的优先级**。也就是说，如果插件中存在与`boi.spec`冲突的配置项，将会以插件的配置为准。

> `boi.use`的优先级与在`boi-conf.js`中的位置无关。但是为了保持代码的美观，建议将其集中放置在顶部。

### 示例
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
