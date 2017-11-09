## 直接配置webpack

boi的构建功能是基于webpack实现的，`boi.spec`API将webpack的配置进行简化和映射，最终解释为webpack的配置。

`boi.spec`API提供的可配置项比较有限，对于非常复杂的需求显得捉襟见肘。这种情形下，你可以直接对webpack进行配置。

`boi.spec`API对JavaScript、Style、Html和Image四种资源暴露出可直接配置webpack的选项。以JavaScript为例，请看如下配置：

```JavaScript
boi.spec('js',{
  webpackConfig: {
    module: {
      rules: [{
        //...
      }]
    },
    plugins: [
      // ...
    ]
  }
});
```

**请注意**：
1. 请勿使用此配置项进行`entry`和`output`定制，否则会引起构建失败；
2. boi使用[webpack-merge](https://github.com/survivejs/webpack-merge)将`webpackConfig`与内置的默认配置合并，合并结果并不能保证绝对可用。所以建议将此配置项作为一些微小的补充配置，复杂度较高的配置建议借助[boi插件](./_advance-plugin.md)实现。
