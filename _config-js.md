## JavaScript编译配置

JavaScript编译配置使用`boi.spec`API：

```JavaScript
boi.spec('js',options);
```

`options`包括以下可配置项：

* `ext`：`String`，源文件扩展类型，默认为`js`；

* `source`：`String`，js源文件的二级目录名称，相对于`[basic](_config-basic.md).source`。默认为`js`；

* `output`：`String`，编译输出js文件的二级目录名称，相对于`[basic](_config-basic.md).output`。默认为`js`；

* `mainFilePrefix`：`String`，js主文件的命名前缀，默认为`main`。也就是说，默认的js主文件名为`main.[name].[ext]`；
> boi会将js主文件作为编译入口，所以主文件的命名规则必须严格遵守，否则会导致找不到编译入口文件。

* `uglify`：`Boolean`，编译输出的js文件内容是否混淆压缩，默认为`true`；

* `useHash`：`Boolean`，编译输出的js文件名是否加上hash指纹，默认为`true`。也就是说，默认的编译输出js文件名为`main.[name].[hash].js`;

* `asyncModuleHash`：`Boolean`，编译输出的异步模块js文件名是否加上hash指纹，默认为`true`。也就是说，默认编译输出的异步js文件名为`[appname].[moduleName].[hash].js`；
> 更多细节请查阅[模块化开发](_modules.md)；

* `splitCommonModule`：`Boolean`，是否提取公共模块并将其单独打包成一个文件，默认为`true`。开启此选项后Boi将提取Webpack runtime和manifest文件并编译为独立的`common.[appname].[hash].js`；

* `lint`：`Boolean`，是否进行代码规范测试，默认为`true`。此选项只在运行`boi build`命令是可用，开启后Boi将会在项目根目录下创建`.eslintrc`文件（需执行`boi build -i`或`boi serve -i`），并使用[eslint](http://eslint.cn)对源码进行规范测试；

* `define`：`Object`，定义编译过程需要替换的变量。通过此配置项，可以定义将源代码中指定的变量在编译之后替换为指定的对应内容。比如有如下boi配置：

    ```JavaScript
    boi.spec('js',{
      define: {
        API: '/login'
      }
    });
    ```

    源码中有以下代码：

    ```JavaScript
    $.ajax({
      url: API,
      dataType: 'jsonp',
      success: function(res){
        // success
      }
    });
    ```

    执行`boi build`之后，编译输出的文件如下：

    ```JavaScript
    $.ajax({
      url: '/login',
      dataType: 'jsonp',
      success: function(res){
        // success
      }
    });
    ```

    源码中的`API`经编译后被替换为`/login`。

    结合`define`配置项和[多环境支持](_envs.md)，可以针对不同的环境自动生成对应的内容。比如有以下boi配置：

    ```JavaScript
    boi.spec('js',{
      dev: {
        define: {
          API: '/login'
        }
      },
      testing: {
        define: {
          API: '//passport.test.boi.com/login'
        }
      },
      prod: {
        define: {
          API: '//passport.boi.com/login'
        }
      }
    });
    ```

    * `dev`为本地开发环境，将`API`配置为本地接口以便使用[Mock服务](_mock.md)；
    * `testing`为测试环境，将`API`配置为测试服务器接口；
    * `prod`为生产环境，将`API`配置为线上服务器接口。

    > 关于编译环境的详细配置请参阅[多环境支持](_envs.md)。

* `files`：`Object`，指定需要编译的js主文件和待提取的公共模块。如果不配置此选项，boi会自动查找**所有**符合命令规则的js文件作为入口文件，如果你只想编译某些指定的文件，可以使用此配置项指定。如下：

    ```
    boi.spec('js',{
      files: {
        main: {
          'index': 'main.index.js',
          'auth': 'main.auth.js'
        },
        common: ['vue','vue-router']
      }
    });
    ```

    * `main`：`Object|null`，指定主js文件，也就是编译入口文件，key-value格式。其中：
      * key代表文件名`[name]`，决定编译输出的文件名`[name].[hash].js`；
      * value代表对应的源文件名，必须是存在于`source`指定目录下的文件名。请注意，**value的取值必须符合`mainFilePrefix`和`ext`命名规范**，否则无法识别。
    * `common`：`Array|undefined|null`，指定需要提取的公共模块。 上述代码将vue和vue-router文件提取出来，与webpack runtime&manifest共同打包成一个独立的`common.[appname].[hash].js`。此配置项与`splitCommonModule`的目标一致，为了更利于浏览器缓存。


### 示例
```JavaScript
boi.spec('js', {
    // JavaScript文件后缀类型
    ext: 'js',
    // JavaScript文件目录，相对于basic.source
    source: 'js',
    // JavaScript文件输出目录，相对于basic.output
    output: 'js',
    // js入口文件的前缀，入口文件的命名规则为[mainFilePrefix].*.[ext]
    mainFilePrefix: 'main',
    // 是否启用文件hash指纹
    useHash: true,
    // 异步模块是否使用hash指纹
    asyncModuleHash: true,
    // 是否压缩混淆
    uglify: true,
    // 是否开启代码规范测试
    lint: true,
    // 否将公共模块提取出来
    splitCommonModule: true,
    // 自行指定入口文件，未指定的文件即使符合命名规范也不会参与编译
    files: {
        main: {
          'index': 'main.index.js'
        },
        common: ['vue','vue-router']
    },
    dev: {
        // 定义开发环境下编译过程替换的变量
        define: {
            'API_TEST': '/test'
        }
    },
    testing: {
        // 定义测试环境下编译过程替换的变量
        define: {
            'API_TEST': '//passport.test.boi.com/test'
        }
    },
    prod: {
        // 定义生产环境下编译过程替换的变量
        define: {
            'API_TEST': '//passport.boi.com/test'
        }
    }
});
```
