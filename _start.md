## 开始使用

### 安装

boi以命令行工具的形式工作，使用npm全局安装：

```bash
npm install boi -g
```

> 目前boi版本为v3.x，如果想安装旧版本请执行：`npm install boi@2 -g`

国内网络环境下访问npm仓库比较缓慢，可以将npm仓库地址修改为国内淘宝镜像：

```bash
npm config set registry https://registry.npm.taobao.org
```

或者使用[cnpm](https://cnpmjs.org/)安装：

```bash
cnpm install boi -g
```

安装成功后，你可以使用boi[脚手架](_scaffold.md)创建一个新的boi项目，或者按照[构建配置文档](_build.md)将已存项目改造为boi项目。

### 配置

Boi的配置文件入口为项目**根目录**下的`boi-conf.js`。

Boi的配置API包括以下内容：

* 项目基本信息配置；
* 针对具体模块的编译配置；
* 本地服务器配置；
* Mock服务配置；
* 部署功能配置；
* 使用插件。

详细情况如下：

* `boi.spec(pattern,options)`：编译配置API
    * `pattern`代表配置的模块类型，可取值包括：
        * `basic`：项目基本信息配置，详见[basic配置](_config-basic.md)；
        * `js`：JavaScript文件相关配置，详见[JavaScript编译配置](_config-js.md)；
        * `style`：style文件相关配置，详见[Style编译配置](_config-style.md)；
        * `html`：html文件相关配置，详见[Html编译配置](_config-html.md)；
        * `image`: 图片文件相关配置，详见[Image编译配置](_config-image.md)。
    * `options`为具体的配置项。不同模块的配置项有差异，具体可参阅各模块配置文档。
* `boi.serve(options)`：本地服务器监听状态配置API，详见[本地服务器](_devserver.md)；
* `boi.mock(options)`：Mock服务配置API，详见[Mock服务](_mock.md)；
* `boi.deploy(options)`：部署功能配置API，详见[远程部署](_deploy.md)；
* `boi.use(name,options)`：使用插件API，详见[使用插件](_plugins.md)
    * `name`代表插件名称；
    * `options`为具体的配置项，json类型。配置项细节由具体插件定义。
* `boi.envs([...])`：自定义环境变量，详见[多环境支持](_envs.md)。

配置细节请查阅[配置文档](_config.md)。


### 脚手架

使用下述命令创建一个新的boi项目：
```bash
boi new <dirname> -t <template>
```

* `dirname`取值为空或`.`或`./`时将会在当前目录创建项目；如果`dirname`为合法的目录名称，将会在当前目录创建以`dirname`命名的新文件夹并在新文件夹内创建项目文件;
* `template`为项目脚手架模板，如果不指定（即执行`boi new <dirname>`）则使用boi默认模板[`generator-boiapp`](https://github.com/boijs/generator-boiapp)。

> 更多的脚手架使用细节请参照[脚手架文档](_scaffold.md)。

### 构建

在项目根目录下执行：

```bash
boi build -e <env> [-i]
```

* `<env>`指定构建项目的环境。如果不指定则默认在`testing`环境下编译。boi内置支持以下环境：
    * `dev`- 开发环境；
    * `testing`- 测试环境；
    * `prod`- 生产环境；
    
> 你可以使用`envs()`扩展更多环境支持，具体细节请参考[多环境支持](./_envs.md)

* `-i`/`--init`(可选)选项的作用是指明是否让boi自动检查依赖(包括boi自身的插件、配置以及项目的dependencies)并自动安装。以下情况必须指定此选项：
  * 项目初始化后第一次运行`build`或`serve`（取其一）；
  * 修改`boi-conf.js`中以下选项后：
    * `style`-`ext`类型/开启`autoprefix`/开启`sprites`;
    * `html`-`engine`类型;

> 由于检查依赖需要花费大量时间，所以除以上情况以外，尽量避免使用`init`选项。

> 关于boi环境配置的细节请参阅[多环境支持](_envs.md)。

编译成功后，默认输出目录为`dest`。

> 关于构建的详细功能配置请参阅[构建](_build.md)。

### 本地服务器
boi提供一个本地服务容器，方便前端工程师独立开发。在项目根目录下运行`serve`命令：

```bash
boi serve [-i]
```

> `-i`/`--init`(可选)选项作用同编译功能。

* 本地服务器默认监听`8888`端口；
* 如果项目中只存在一个`index.html`文件，直接访问`localhost:8888`即可；
* 如果项目中存在多个`index.*.html`文件:
  * 直接访问`localhost:8888`默认返回`index.html`；
  * 如果要访问其他入口，地址格式为`localhost:8888/index.*.html`或者完整路径`localhost:8888/<views dir>/index.*.html`(views路径和html文件名可自行[配置](_config-html.md))；
  * 如果开启了`html`-`removePrefixAfterBuilt`，则可以省略每个html文件的“index”前缀。比如源码存在`index.home.html`和`index.about.html`则可以直接访问`localhost:8888/home.html`和`localhost:8888/about.html`

#### HMR与Livereload

本地开发服务器开启期间，对于JavaScript和CSS源文件的修改无需刷新浏览器即可由HMR即时更新至客户端，而HTML源文件的修改会触发Livereload。

开发过程中**已存文件**的内容修改无需重启本地服务器，但如果**增加新文件或者已存文件重命名，则必须重启本地服务器**才可看到效果。

> 本地服务器详细功能配置请参阅[本地服务器](_devserver.md)。

#### Mock服务

Boi的Mock服务有两种运行模式：独立运行和Dev server集成。两种模式的配置项完全一致，如下：

```javascript
boi.mock('Get /userinfo').params(['name']).custom({
  jsonpCallback: 'cb'
}).response({
  success: {
    code: 200,
    msg: '请求成功'
  },
  fail: {
    code: 500,
    msg: '请求失败'
  }
});

boi.mock('Post /signup').proxy('https://passport.boi.com/signup');
```

上述代码定义了两个Mock接口：
* 支持Get请求的`/userinfo`接口。必选参数`name`，支持jsonp并且jsonpCallback识别为`cb`(默认为`callback`)，返回数据自定义数据；
* 代理接口`/signup`。将本地接口`/signup`的Post请求代理到'https://passport.boi.com/signup'接口。

##### 独立运行
在项目根目录下运行：
```bash
boi mock -p 9999
```
* `-p`指定监听端口号，默认为8889。

然后运行本地开发服务器时增加`-s`选型：
```bash
boi server -s
```
* `-s`代表运行dev server不会集成mock服务。

> 之所以支持独立Mock服务是为了多项目共用同一Mock接口的场景，增强Mock的复用性。

##### Dev server集成
不添加`-s`选项运行dev server即可集成Mock服务：
```bash
boi serve
```

> 关于Mock的详细配置请参阅[Mock](_mock.md)。

### 部署

编译完成之后，执行以下命令将生成的代码部署到远程服务器：

```bash
boi deploy -e <env>
```

* `env`可选**`dev`以外**的任意有效环境变量，包括boi内置的`testing/prod`以及通过`envs()`API定义的环境变量；
* 部署功能使用sftp作为传输协议，所以需要接收文件的服务器提供相应权限。

> 部署的详细功能配置可参阅[远程部署](_deploy.md)。

#### 兼容性
##### 浏览器版本
boi v2版本的构建系统基于webpack v3，构建产出的代码不支持IE8及以下浏览器。

##### Node.js版本
boi源码使用了部分ES6特性，请保证Node.js版本在6及以上。
