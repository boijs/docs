## 部署/发布

Boi可将构建产出的文件一键部署/发布到远程服务器，在项目**根目录**下运行：

```bash
boi deploy -e <env>
```

其中`<env>`代表部署的环境。

请注意，**`deploy`动作并不会对源码进行编译**，所以请确保在执行`deploy`之前已经成功完成了`build`流程，并且**一定要保证`deploy`和`build`的是在相同环境（`<env>`）下执行**。

### 配置

使用`boi.deploy(options)`API进行部署功能配置。

`options`包括以下子配置项：
* `cdn`：cdn服务器配置，包括以下子配置项:
    * `domain`：cdn服务器域名；
    * `path`：项目存放在cdn服务器的路径
* `connect`：连接配置，包括以下子配置项：
  * `type`：`String`，部署文件协议。Boi目前只支持**SFTP**协议部署，后续版本会增加更多支持；
  * `config`：`Object`，连接配置，包括以下子项：
    * `host`：`String`，目标服务器域名或者IP地址。如果使用域名，请确保此域名可识别（可以运行ping命令检测）；
    * `path`：`String`，目标服务器部署目录；
    * `auth`：`Object`，登录目标服务器的用户名和密码：
      * `username`：`String`，登录部署服务器的用户名；
      * `password`：`String`，登录部署服务器的密码。

> `connect.config.path`与`cdn.path`是两个概念。`connect.config.path`代表的是静态文件托放于CDN服务器的路径，`cdn.path`是CDN服务器对外的路由路径。

其中`cdn`配置项同时会影响构建产出的静态资源引用地址，比如编译产出`main.app.js`和`main.css.css`,同时`cdn`配置如下：
```json
cdn: {
  domain: 'static.boi.com',
  path: 'app'
}
```

最终编译产出的`index.html`中对静态资源的引用地址如下：
```html
<html>
  <head>
    <link rel="stylesheet" href="//static.boi.com/app/js/main.app.css">
  </head>
  <body>
    <script src="//static.boi.com/app/js/main.app.js"></script>
  </body>
</html>
```

#### Tips
如果你的代码需要拖放在多台域名和路径不同的CDN服务器，则可以将`cdn`进行如下配置：
```json
cdn: {
  domain: '',
  path: './'
}
```

最终编译产出的`index.html`中对静态资源的引用地址如下：
```html
<html>
  <head>
    <link rel="stylesheet" href="./js/main.app.css">
  </head>
  <body>
    <script src="./js/main.app.js"></script>
  </body>
</html>
```

### 部署
如果是首次部署项目，CDN服务器对应的目录为空，执行`boi deploy`会出现以下提示：
```bash
? The target directory isn't the one of previous deployment, do you still want to deploy your project?
```

输入`N`或者`no`之后会终止部署行为。输入`y`或者点击回车键会继续运行部署流程。

部署成功之后会出现以下提示：
![](./assets/deploy_suc.png)

#### 判断部署目录是否为历史同一目录的方法
Boi在首次部署当前项目后会在目标服务器创建一个特殊的flag文件，命名规则是`<appName>.flag`。其中`appName`是`boi-conf.js`中配置的项目名称。在之执行部署上传之前首先会检测部署目录是否存在此flag文件，如果存在则代表部署目录是当前项目的历史部署目标；如果不存flag文件则代表部署目录是一个全新的目录。

#### windows兼容问题
windows系统部署成功后输出的提示可能会显示错乱，如下图：
![](./assets/win-err.png)

这个问题只会出现在windows系统自带的cmd终端中，是因为windows自带的cmd终端默认配置窗口大小为80，不能容纳输出信息的长度，从而出现了折行。

解决问题的方法如下：点击cmd终端左上角弹出菜单->选择属性->布局->参照下图将终端窗口大小调大->点击确定后重启终端即可解决显示折行问题。

![](./assets/win-setting.png)

### 示例

以下是通过`sftp`协议部署的boi配置项：

```JavaScript
boi.deploy({
    // 测试环境
    testing: {
      // cdn前缀
      cdn:{
        domain: 'test.boi.com',
        path: '/app/'
      },
      connect: {
        // 连接协议
        type: 'sftp',
        // 目标ip和path
        config: {
          host: '192.168.1.1',
          path: '/test/app',
          auth: {
            username: 'admin',
            password: '123456'
          }
        }
      }
    },
    // 生产环境
    prod: {
      cdn:{
        domain: 'staic.boi.com',
        path: '/app/'
      },
      connect: {
        type: 'sftp',
        config: {
          host: '192.168.1.2',
          path: '/static/app',
          auth: {
            username: 'admin',
            password: '123456'
          }
        }
      }
    }
});
```

### 局限性
部署是一项复杂且对细节要求很高的工作，同时需要兼顾安全、准确等因素。不同的团队对于部署有不同的需求和实现方案。Boi的部署功能很轻量，是一个比较“廉价”的功能，是基于作者自身团队的需求而开发出来的方案，普适度并不高。所以Boi在部署环节并没有投入大量精力去打造，因为你无法做出一套适用于绝大多数团队的方案。