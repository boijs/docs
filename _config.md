## 配置

### 配置文件

boi的配置文件入口为项目**根目录**下的`boi-conf.js`。

boi的配置API包括以下内容：

* 针对具体模块的编译配置，包括：
    * 项目基本信息配置；
    * JavaScript/Style/Html/Image文件的编译配置；
* 使用插件；
* 本地服务器配置，包括：
  * 本地服务器监听状态配置；
  * Mock服务配置。
* 部署功能配置。

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
* `boi.mock(options)`：Mock服务配置API，详见[Mock服务](_devserver-mock.md)；
* `boi.deploy(options)`：部署功能配置API，详见[远程部署](_deploy.md)；
* `boi.use(name,options)`：使用插件API，详见[使用插件](_config-plugins.md)
    * `name`代表插件名称；
    * `options`为具体的配置项，json类型。配置项细节由具体插件定义。

配置细节请查阅具体模块的配置文档。
