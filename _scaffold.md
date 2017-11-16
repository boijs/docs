## 脚手架

Boi脚手架完整的命令格式如下：
```bash
boi new <dirname> -t <template>
```
* `--template`选项指定项目模板，简写为`-t`。如果不指定项目模板，Boi将使用默认模板[generator-boiapp](https://github.com/boijs/generator-boiapp)；
* `<dirname>`指定创建项目的文件夹名称，可以有以下取值：
  * `./`或者`.`或者为空，将当前目录指定为项目目录；
  * 任何合法的文件夹名称，比如"webapp",将在当面目录下创建新的文件夹作为项目目录。

> Boi的脚手架功能是基于[yeoman](http://yeoman.io/)构建的，理论上可以安装任何yeoman的脚手架方案。你可以根据yeoman的规范自定义Boi脚手架模板，详细的开发规范请查阅[自定义脚手架](_advance-scaffold.md)。

默认的脚手架模板[generator-boiapp](https://github.com/boijs/generator-boiapp)的执行流程如下：
1. 选择模板类型：
```base
$ boi new webapp
? Which template would you like to generate: (Use arrow keys)
❯ Single Page Application 
  Vue Application 
  Multi Page Application 
```
generator-boiapp内置三种模板类型：
  * 'Single Page Application' - 单页项目；
  * 'Vue Application' - Vue项目；
  * 'MultiPage Application' - 多页项目。

2. 选择你的项目需要的独立第三方js库文件，这部分文件是以`<script>`或者`<link>`标签从html文档中引入的，不会参与编译：
```bash
? Which thirdparty lib(s) would your project needs(import with <script> tag by html document)
❯◯ jQuery 1.10.1
 ◯ BootStrap
```
> 以上第三方库文件使用官方的cdn。

3. 是否自动补全CSS hack前缀，默认为`Y`:
```bash
? Enable CSS autoprefixer? (Y/n)
```

4. 选择CSS预编译器类型，默认为`CSS`：
```bash
? Which syntax do you want to write your stylesheet (Use arrow keys)
❯ CSS
  Less
  Scss
```

7. 与第三条类似，选择项目需要的第三方css库文件：
```bash
? Which thirdparty stylesheet(s) would your project needs(import with <link> tag by html document) 
❯◯ BootStrap
```

8. 是否启用自动生成CSS Sprites功能，默认为`Y`：
```bash
? Enable CSS Sprites? (Y/n)
```

执行成功后一个单页项目的初始目录如下图所示：
```bash
├── boi-conf.js
├── boi-mock.js
├── package.json
└── src
    ├── assets
    │   ├── icons
    │   │   ├── icon-about.png
    │   │   └── icon-home.png
    │   └── logo.png
    ├── index.html
    ├── js
    │   ├── main.webapp.js
    │   └── part
    │       ├── moduleA.js
    │       └── moduleB.js
    └── style
        └── main.webapp.less
```

根目录下的`boi-conf.js`是boi的配置文件，脚手架已经为你创建了基础的配置项，你可以参照[配置文档](_config.md)进行更细化的配置。
