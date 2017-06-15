## Image编译配置

Image编译配置使用`boi.spec`API：

```
boi.spec('image',<options>);
```

第一个参数代表需要配置的模块，Image对应value为`image`。

`options`包括以下可配置项：

* `ext`：`Array`，图片后缀名数组，默认值为`['jpg','jpeg','svg','png','eot','ttf','ico','gif','woff','woff2']`；

* `output`：`String`，编译输出的目录，相对于`[basic](config.basic.md).output`。默认为`'assets'`；

* `base64`：`Boolean`，是否对小体积图片使用base64嵌入，默认为`true`；

* `base64Limit`：`Number`，参与base64编码的文件体积上限，以**byte**为单位，默认为`1000`。此配置项单独配置无效，必须在`base64`配置项为`true`时才会生效。

* `useHash`：`Boolean`，编译输出的文件名是否加上hash指纹，默认为`true`。

### 示例
```JavaScript
boi.spec('image', {
    // 图片后缀类型
    ext: ['png', 'jpg', 'gif', 'jpeg'],
    output: 'assets',
    // 是否对小尺寸图片进行base64编码，默认false
    base64: true,
    // 应用base64编码图片的体积临界值，小于此值得图片会被base64编码
    base64Limit: 1000,
    useHash: true
});
```
