## Mock假数据

boi本地服务器提供mock功能，你可以使用`boi.mock`API自定义接口名称、方法以及返回的假数据。比如：

```JavaScript
boi.mock({
  'GET /login?$user&$password': {
    ok: {
      code: 100,
      msg: 'ok',
      data: {
        a: 1,
        b: 2
      }
    },
    fail: {
      code: 200,
      msg: 'fail'
    }
  },
  'POST /signup?$user&$password': {
    ok: {
      code: 100,
      msg: 'ok',
      data: {
        a: 1,
        b: 2
      }
    },
    fail: {
      code: 200,
      msg: 'fail'
    }
  }
});
```

上述代码配置了两个API规则和返回结果：GET请求的`/login`和POST请求的`/signup`，以及各自的请求成功返回值和请求失败返回值。

### 接口定义规则

以`/login`接口为例，格式如下：

```
'GET /login?$user&$password'
```

接口定义分为两部分：请求方法和接口规则。两者以空格作为分隔符

* 请求方法。上述代码中的`GET`指明`/login`接口可接受GET请求。方法名不要求大小写，但是为了代码易读，建议使用大写字母作为方法名；
> 目前boi只支持GET和POST请求，后续版本会加入更多支持。

* 接口规则。接口规则分为两部分：*接口路径*和*参数*。上述代码中的接口路径为`/login`，`?`后的部分为参数。参数以`$`作为起始字符，以`&`作为分隔符。上述代码中定义了两个参数：`user`和`password`。

### 请求成功和请求失败

* 如果没有定义参数，任何请求都会返回`ok`定义的返回值，代表请求成功；
* 如果定义了参数，但是请求中没有传递对应的参数或者参数不完整，则会返回`fail`定义的返回值，代表请求失败。如果参数完整，则请求成功。

### jsonp和AJAX

你不必定义一个接口是否支持jsonp，boi会根据请求中是否包含`callback`或者`cb`参数决定是跨域jsonp还是同域AJAX。

> 接口规则的参数中不需要配置callback参数。
