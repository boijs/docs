## JavaScript代码规范

boi使用[eslint-loader](https://github.com/MoOx/eslint-loader)实现对JavaScript代码的规范检查，测试内核为[eslint](http://eslint.cn/)。

boi默认使用[Airbnb JavaScript规范](https://github.com/airbnb/javascript)基本一致。

### 自定义规范

你可以根据自身的业务需求自定义规范细节，步骤如下：

1. 在项目中创建json类型的规范配置文件，比如在根目录下创建`eslint_rules.json`；
2. 配置boi：

    ```
    boi.spec('js',{
      lint: true,
      lintConfigFile: `./eslint_rules.json`
    });
    ```
3. 根据[eslint配置](http://eslint.cn/docs/user-guide/configuring)和[eslint rules](http://eslint.cn/docs/rules/)修改`eslint_rules.json`内容。

完成以上步骤后执行`boi build --env <env>`便会根据自定义的规范细节进行规范测试。
