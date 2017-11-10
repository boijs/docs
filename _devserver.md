## 本地开发服务器

[![GitHub stars](https://img.shields.io/github/stars/boijs/boi-server.svg?style=social&label=Stars)](https://github.com/boijs/boi-server)
[![GitHub forks](https://img.shields.io/github/forks/boijs/boi-server.svg?style=social&label=Fork)](https://github.com/boijs/boi-server)

Boi提供一个本地的服务器供搭建开发环境使用，同时可集成[Mock](_mock.md)服务。

执行如下命令查看完整的build命令：
```bash
$ boi serve -h

  Usage: serve|server [options]

  run local server for development

  Options:
    -i, --init      init all configurations and install plugins&dependencies
    -s, --separate  run server without mock
    -h, --help      output usage information

  Examples:
    $ boi serve
```

`options`包括以下两个:
* `-i`/`--init`(可选)选项作用同[构建](_build.md)的同名选型功能相同。
* `-s`/`--separate`（可选）开启此选项后，本地服务器将不集成[Mock](_mock.md)服务。

在项目**根目录**下执行`boi serve`开启本地服务器，开启成功后会有以下提示：

```bash
ℹ Server is listening 8888
```

本地服务器默认监听`8888`端口，可以通过`boi.serve`API修改配置，如下：
```JavaScript
boi.serve({
  port: 9999
});
```

本地服务器对应的环境变量为`dev`，所以`boi-conf.js`中针对`dev`环境的配置将影响本地服务器的构建结果。此外，本地服务器环境（即`dev`环境）下构建输出的文件名均不带hash指纹。

路由优化：
* 如果项目中只存在一个`index.html`文件，直接访问`localhost:8888`即可；
* 如果项目中存在多个`index.*.html`文件:
  * 直接访问`localhost:8888`默认返回`index.html`；
  * 如果要访问其他入口，地址格式为`localhost:8888/index.*.html`或者完整路径`localhost:8888/<views dir>/index.*.html`(views路径和html文件名可自行[配置](_config-html.md))；
  * 如果开启了`html`-`removePrefixAfterBuilt`，则可以省略每个html文件的“index”前缀。比如源码存在`index.home.html`和`index.about.html`则可以直接访问`localhost:8888/home.html`和`localhost:8888/about.html`


### HMR&Livereload

本地开发服务器开启期间，对于JavaScript和CSS源文件的修改无需刷新浏览器即可由HMR即时更新至客户端，而HTML源文件的修改会触发Livereload。

开发过程中**已存文件**的内容修改无需重启本地服务器，但如果**增加新文件或者已存文件重命名，则必须重启本地服务器**才可看到效果。