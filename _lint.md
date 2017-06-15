## 代码规范

boi目前只集成了JavaScript代码规范测试功能，可以通过[配置](_config.md)API配置自定义规范。针对规范测试的配置项为以下两项：

* `lint`：`Boolean`，是否开启规范测试，默认为`true`；
* `lintConfigFile`：`String`，指定自定义规范配置的文件路径。

> `development`环境下不会执行代码规范检查。
