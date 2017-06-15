## 编译环境配置

boi支持为不同的环境配置独立的编译选项。boi将环境划分为三种：`development`、`testing`和`production`，三者的定义与执行时机如下表：

| 名称 | value | 说明 | 执行时机 | uglify是否可用 | sourcemap | 备注 |
| :---: | :---: | --- | --- | :---: | :---: | --- |
| development | dev     | 本地开发环境 | `boi serve` | N | Y | entry包括livereload模块 |
| testing     | testing | 测试环境 | `boi build --env testing` | Y | N | -- |
| production  | prod    | 生产环境 | `boi build --env prod` | Y | N | -- |


* `development`：本地开发环境，只在开启本地服务器时生效，所以`development`环境的配置项不会影响最终的编译结果；
* `testing`：测试环境，编译结果可用于上线之前的内部测试；
* `production`：生产环境。

boi之所以将以上三种环境进行区分，是为了覆盖一个前端项目从始至终的任何一个阶段。`development`环境下，boi提供本地服务容器和mock服务，方便前端工程师不依赖后端接口进行独立开发；区分`testing`环境和`production`环境是由于前端资源都是静态的，代码写死了就不会根据运行环境改变。最常见的场景是：

* 接口地址与主站是异域的，进行跨域请求时需要完整的接口url而不是path；
* 测试环境与生产环境的接口域名不相同；
* 测试完毕之后，如果需要提交上线，必须修改js脚本中接口的url。

这种场景下麻烦的是最后一步，要么人工手动修改，要么通过工具自动修改。即使使用工具，也需要手动修改工具的配置项。所以不论哪一种方式，都存在不必要的手动操作，不仅麻烦，而且容易出错。

boi对以上场景的解决方案是：结合环境配置和[JavaScript的define功能](_config-js.md)，为不同环境配置对应的配置项。比如：

```
boi.spec('js',{
  dev: {
    define: {
      LOGIN_API: '/login'
    }
  },
  testing: {
    useHash: false,
    define: {
      LOGIN_API: '//passport.test.com/login'
    }
  },
  prod: {
    useHash: true,
    define: {
      LOGIN_API: '//passport.app.com/login'
    }
  }
});

boi.mock({
  'GET /login': {
    ok: {
      code: 100,
      msg: '登录成功',
      data: {
        user: 'boi'
      }
    },
    fail: {
      code: 200,
      msg: '登录失败'
    }
  }
});
```

以上配置的表现效果为：

* `development`环境下将login接口映射到本地，通过本地服务器的Mock服务进行开发调试；
* 执行`boi build --env testing`，根据`testing`环境的配置项，编译输出的文件中login接口被替换为测试url，并且不使用hash指纹；
* 执行`boi build --env prod`，根据`production`环境的配置项，编译输出的文件中login接口被替换为线上url，并且文件url加上hash指纹。

> 目前分环境配置功能只支持通过`boi.spec`API进行配置。
