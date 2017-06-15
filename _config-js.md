## JavaScript编译配置

JavaScript编译配置使用`boi.spec`API：

```JavaScript
boi.spec('js',<options>);
```

第一个参数代表需要配置的模块，JavaScript对应value为`js`。

`options`包括以下可配置项：

* `ext`：`String`，源文件扩展类型，默认为`js`；

* `source`：`String`，js源文件的二级目录名称，相对于`[basic](_config-basic.md).source`。默认为`js`；

* `output`：`String`，编译输出js文件的二级目录名称，相对于`[basic](_config-basic.md).output`。默认为`js`；

* `mainFilePrefix`：`String`，js主文件的命名前缀，默认为`main`。也就是说，默认的js主文件名为`main.[name].[ext]`；
> boi会将js主文件作为编译入口，所以主文件的命名规则必须严格遵守，否则会导致找不到编译入口文件。

* `uglify`：`Boolean`，编译输出的js文件内容是否混淆压缩，默认为`true`；

* `useHash`：`Boolean`，编译输出的js文件名是否加上hash指纹，默认为`true`。也就是说，默认的编译输出js文件名为`main.[name].[hash].js`;

* `asyncModuleHash`：`Boolean`，编译输出的异步模块js文件名是否加上hash指纹，默认为`true`。也就是说，默认编译输出的异步js文件名为`[appname].[moduleName].[hash].js`。详细情况请查阅[模块化开发文档](_modules.md)；

* `splitCommonModule`：`Boolean`，是否提取公共模块并将其单独打包成一个文件，默认为`false`。此配置项是性能优化的选项，根据应用场景不同有两种方案：
  * 当存在多个js主文件并且存在公共模块时，boi提取公共模块打包成一个js文件，利于浏览器缓存；
  * 当只存在一个js主文件时，将这些webpack runtime整合单独打包成一个js文件；

* `lint`：`Boolean`，是否进行代码规范测试，默认为`false`。boi使用[eslint](http://eslint.cn)作为js规范测试工具；

* `lintConfigFile`：`String`，指定自定义的eslint配置文件。默认使用[Airbnb JavaScript](https://github.com/airbnb/javascript)规范。你可以通过此配置项指定eslint配置json文件，如下：

    ```javascript
    lintConfigFile: './eslint_config.json'
    ```

    `lintConfigFile`取值有两种方式：
    * 相对于项目**根目录**的相对路径；
    * 绝对路径。

> 以上6个配置项只会影响测试和生产环境，开发环境无效。

* `libraryType`：`String`，开启此配置项将会把项目源码打包成library，可选值参考[webpack官方文档](https://doc.webpack-china.org/configuration/output/#output-librarytarget)。默认为`undefined`，即不开启；

* `libraryName`：`String`，在`libraryType`开启前提下，指定打包输出的library名称，细节参考[webpack文档](https://doc.webpack-china.org/configuration/output/#output-library)；

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

    结合`define`配置项和boi的[编译环境配置功能](_config-env.md)，可以针对不同的环境自动生成对应的内容。比如有以下boi配置：

    ```JavaScript
    boi.spec('js',{
      dev: {
        define: {
          API: '/login'
        }
      },
      testing: {
        define: {
          API: '//passport.test.com/login'
        }
      },
      prod: {
        define: {
          API: '//passport.app.com/login'
        }
      }
    });
    ```

    * `dev`为本地开发环境，将`API`配置为相对路径以便使用本地服务器的[Mock服务](_devserver-mock.md)功能；
    * `testing`为测试环境，将`API`配置为测试服务器域名和路径；
    * `prod`为生产环境，将`API`配置为线上服务器域名和路径。

    > 关于编译环境的详细配置请参阅[编译环境配置](_config-env.md)。

* `files`：`Object`，指定需要编译的js主文件。如果不配置此选项，boi会自动查找**所有**符合命令规则的js文件作为入口文件，如果你只想编译某些指定的文件，可以使用此配置项指定。如下：

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

    `files`有两个子配置项：

    1. `main`：`Object|null`，指定主js文件，也就是编译入口文件，key-value格式。其中：
      * key代表文件名`[name]`，决定编译输出的文件名`[name].[hash].js`；
      * value代表对应的源文件名，必须是存在于`source`指定目录下的文件名。请注意，**value的取值必须符合`mainFilePrefix`和`ext`规则**，否则无法识别。
    2. `common`：`Array|undefined|null`，指定需要提取的公共模块。 上述代码将vue和vue-router文件提取出来，与webpack runtime共同打包成一个独立的common文件；如果赋值为空数组将会提取webpack runtime单独打包；如果为空值则不会提取公共模块。此配置项与`splitCommonModule`的目标一致，为了更利于浏览器缓存。


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
    lint: false,
    // 否将公共模块提取出来
    splitCommonModule: true,
    // 自行指定入口文件，未指定的文件即使符合命名规范也不会参与编译
    // files: {
    //     main: {},
    //     common: []
    // },
    dev: {
        // 定义开发环境下编译过程替换的变量
        define: {
            'API_TEST': '/api/test'
        }
    },
    testing: {
        // 定义测试环境下编译过程替换的变量
        define: {
            'API_TEST': '//10.10.10.10:8888/api/test'
        }
    },
    prod: {
        // 定义生产环境下编译过程替换的变量
        define: {
            'API_TEST': '//map.sogou.com/api/test'
        }
    }
});
```
