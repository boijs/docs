## 编写boi插件

如果[直接配置webpack](_advance-config.md)仍然难以满足业务需求，Boi还提供了插件系统来配置更复杂的需求。

如果你熟悉Webpack，那么编写Boi插件对你来说是一件非常简单的事。你只需要编写一个导出`function`类型的npm模块即可。
```JavaScript
module.exports = function(){};
```

唯一的限制便是对这个函数的返回值有一些额外的要求：必须返回一个对象类型，包含两个属性`webpacConf`和`dependencies`。如下示例：
```JavaScript
module.exports = function(){
  return {
    webpackConf: {
      module: {
        rules: [
          // ...
        ]
      },
      plugins: [
        // ...
      ]
    },
    dependencies: [
      'vue-loader'
    ]
  };
};
```

* `webpackConf`,`Object`。代表的是插件对webpack的配置项，这些配置项将会被Boi合并到默认配置项中；
* `dependencies`,`Array`。代表使用插件必须安装的模块，这些模块会由Boi自动安装。

### 编写可配置的插件

在[使用插件](_plugins.md)中我们使用`boi.use`API引入`'boi-plugin-vue'`插件并且传入了一系列的配置项，这些配置项不是针对Boi的，而是针对插件`'boi-plugin-vue'`的。

如果插件本身也是可配置的，那么在编写插件时，可以将插件编写为可接受参数即可。如下：
```JavaScript
module.exports = function(options){};
```

上述代码中函数的参数`options`即可接受的配置项集合。

### 示例
可参考[boi-plugin-vue](https://github.com/boijs/boi-plugin-vue)源码。
