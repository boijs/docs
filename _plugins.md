## 使用插件

`boi.spec`API只支持常规的几个模块：js/style/html/image。遇到比较复杂的编译需求，比如编译react或者vue文件，编译本身需要非常复杂的配置。这种场景下，`boi.spec`API就显得捉襟见肘了。

Boi插件就是为了弥补自身封装方案的局限性，你可以使用[第三方Boi插件](https://boijs.github.io/res/)，也可以根据自身的需求[编写Boi插件](_advance-plugin.md)。

使用Boi插件的API是`boi.use(pluginName,pluginOptions)`：

```JavaScript
boi.use('boi-plugin-vue', {
  cssOutputDir: 'style',
  cssUseHash: true,
  extractCss: false,
  enablePostCss: false,
  enableLint: false
});
```

上述代码使用了[boi-plugin-vue](https://github.com/boijs/boi-plugin-vue)插件，这个插件的作用是编译`.vue`后缀的文件。第二个参数`options`是由插件本身提供的配置选项。

> `boi.use`的优先级与在`boi-conf.js`中的位置无关。但是为了保持代码的美观，建议将其集中放置在顶部。

### 环境变量
Boi插件的配置项同样可以使用[环境变量](_envs.md)，包括Boi内置的三种环境变量以及通过`boi.envs()`扩展的自定义环境变量。比如：
```JavaScript
// 新增自定义环境变量grey
boi.envs(['grey']);
// 使用boi-plugin-vue
boi.use('boi-plugin-vue', {
  cssOutputDir: 'style',
  cssUseHash: true,
  extractCss: false,
  enablePostCss: false,
  enableLint: true,
  // 自定义环境变量
  grey: {
    // 灰度环境下禁用hash
    cssUseHash: false
  },
  // Boi内置环境变量
  testing: {
    // 开发环境下禁用lint
    enableLint: false
  }
});
```

以上配置并不会完整的传入'boi-plugin-vue'内部逻辑，而是在外层首先由[boi-parser](https://github.com/boijs/boi-parser)根据当前环境对配置进行解析，而后传入插件内部逻辑中。比如在以上配置基础上运行`boi build -e grey`，经boi-parser解析后的配置项等价于：
```json
{
  cssOutputDir: 'style',
  cssUseHash: false,
  extractCss: false,
  enablePostCss: false,
  enableLint: true
}
```

而执行`boi build -e testing`后经boi-parser解析的内容如下：
```json
{
  cssOutputDir: 'style',
  cssUseHash: true,
  extractCss: false,
  enablePostCss: false,
  enableLint: false
}
```

解析的原理如下：

![](/assets/parser.png)