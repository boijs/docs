## 直接配置webpack

boi的构建功能是基于webpack v1实现的，`boi.spec`API将webpack的配置进行简化和映射，最终解释为webpack的配置。

`boi.spec`API提供的可配置项比较有限，对于非常复杂的需求显得捉襟见肘。这种情形下，你可以直接对webpack进行配置。

`boi.spec`API对JavaScript、Style、Html和Image四种资源暴露出可直接配置webpack的选项。以JavaScript为例，请看如下配置：

```JavaScript
boi.spec('js',{
  webpackConfig: {
    rules: [],
    noParse: [],
    plugins: []
  }
});
```

以上代码是四种资源类型支持的全部webpack可配置项，其中：

* `rules`和`noParse`对应webpack的`module`配置的同名配置项。详情请参照[webpack官方文档](https://doc.webpack-china.org/configuration/module/)；
* `plugins`直接映射为webpack的`plugins`配置项，参考[官方文档](https://doc.webpack-china.org/configuration/plugins/)；

**请注意**：直接配置webpack将会覆盖boi内部封装的所有配置，请谨慎使用。
