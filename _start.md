## 开始使用

### 安装

boi以命令行工具的形式工作，使用npm全局安装：

```bash
npm install boi -g
```

国内网络环境下访问npm仓库比较缓慢，可以将npm仓库地址修改为国内淘宝镜像：

```bash
npm config set registry https://registry.npm.taobao.org
```

或者使用[cnpm](https://cnpmjs.org/)安装：

```bash
cnpm install boi -g
```

安装成功后，你可以使用boi[脚手架](_start-scaffold.md)创建一个新的boi项目，或者按照[配置文档](_config.md)将已存项目改造为boi项目。


### 脚手架

使用下述命令创建一个新的boi项目：
```bash
boi new <dirname> --template <template>
# or
boi new <dirname> -t <template>
```

* `dirname`取值为空或`.`或`./`时将会在当前目录创建项目；如果`dirname`为合法的目录名称，将会在挡墙目录创建以`dirname`命名的新文件夹并在新文件夹内创建项目文件;
* `template`为项目脚手架模板，如果不指定（即执行`boi new <dirname>`）则使用boi默认模板[`generator-boiapp`](https://github.com/boijs/generator-boiapp)。

> 更多的脚手架使用细节请参照[脚手架文档](_start-scaffold.md)。

### 编译

在项目根目录下执行：

```bash
boi build --env <env>
```

通过`<env>`指定构建项目的环境。`build`命令默认在`testing`环境下编译。

boi内置支持三种执行环境：
* `dev`- 开发环境；
* `testing`- 测试环境；
* `prod`- 生产环境；

> 关于boi环境配置的细节请参阅[编译环境配置](_config-env.md)。

编译成功后，默认输出目录为`dest`。

### 本地服务器
boi提供一个本地服务容器，方便前端工程师独立开发。在项目根目录下运行`serve`命令：

```bash
boi serve
```

* 如果项目中只存在一个`index.*.html`文件，直接访问`localhost:8888`即可；

* 如果项目中存在多个`index.*.html`文件:
  * 直接访问`localhost:8888`默认返回`index.<appname>.html`，其中`appname`通过[`basic`配置](_config-basic.md);
  * 如果要访问其他入口，地址格式为`localhost:8888/index.*.html`或者或者完整路径`localhost:8888/<views dir>/*.html`(views路径和html文件名可自行[配置](_config-html.md))。

> 本地服务器默认监听`8888`端口。

#### 浏览器自动刷新

本地服务器支持浏览器自动刷新，开发过程中**已存文件**的内容修改保存后会动态编译并且出发浏览器自动刷新，无需重启本地服务器。但如果**增加新文件或者已存文件重命名，则必须重启本地服务器**才可看到效果。

> 本地服务器详细功能配置请参阅[本地服务器](_devserver.md)。

#### Mock服务

本地服务器同时集成了Mock服务，你可以自行定义接口名称和返回数据，比如有以下配置：

```JavaScript
boi.mock({
    'GET /login?$name&pwd': {
        ok: {
            code: 100,
            msg: '登录成功',
            data: {
                user: 'boi'
            }
        },
        fail: {
            code: 101,
            msg: '登录失败'
        }
    }
});
```

客户端浏览器对api`/login`发起请求后，Mock服务器会根据**参数的完整性**决定返回`ok`或者`fail`数据。

> 关于Mock的详细功能配置请参阅[Mock](_devserver-mock.md)。

### 部署

编译完成之后，执行以下命令将生成的代码部署到远程服务器：

```bash
boi deploy --env <env>
```

其中`env`可选`testing`或`prod`，分别对应测试服务器和线上服务器。

boi部署功能使用sftp作为传输协议，所以需要接收文件的服务器提供相应ssh权限。部署的详细功能配置可参阅[远程部署](_deploy.md)。

#### 兼容性
##### 浏览器版本
boi v2版本的构建系统基于webpack v2，构建产出的代码不支持IE8及以下浏览器。如果你的项目需要兼容IE8浏览器，请使用[boi v1](https://zhoujunpeng.gitbooks.io/boi/content/)版本。

##### Node.js版本
boi源码使用了部分ES6特性，请保证Node.js版本在6及以上。
