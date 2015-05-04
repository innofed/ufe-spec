# 包结构规范

## 包开发说明

### 包定义

`包`是实现某个独立功能，有复用价值的代码集。在具体的实现过程中， **[必须]** 按照[模块和加载器规范](module.md)来开发和管理模块。

### 模块定义

`包`中的模块定义时 **[必须]** 采用匿名id`define( factory )`进行定义， **[不允许]** 使用`define( moduleId, factory )`。

### 依赖管理

`包`中的模块对其他模块的依赖分成两种：`内部模块依赖`和`外部包依赖`。下面是关于模块依赖的管理说明：

* 对`内部模块依赖`的情况， **[必须]** 保证内部模块id与路径的对应关系。require依赖引用 **[必须]** 使用`relative id`， **[不允许]** 使用`top-level id`。
* 对`外部包依赖`的情况，require依赖引用 **[必须]** 使用`top-level id`。

开发时，我们通常会做一些测试用例或示例，此时需要通过AMD Loader将当前包粘合到页面环境，并使其可运行。这时我们需要遵守一些规则：

* 对`内部模块依赖`，AMD Loader配置 **[建议]** 通过`packages`将`location`配置到`${root}`下的`src`目录， **[不允许]** 通过`paths`进行路径映射。
* 对`外部包依赖`，请参照[项目目录结构规范](directory.md)将相关依赖包导入，并且 **[建议]** 通过`packages`项配置AMD Loader。

```javascript
// 示例
require.config({
    packages: [
        {
            name: 'biz1',
            location: '../src',
            main: 'main'
        },
        {
            name: 'jquery',
            location: '../dep/jquery/1.10.0/src',
            main: 'main'
        }
    ]
});
```

### 资源

**[允许]** 包含如下类型的资源：脚本, 样式以及样式相关图片, 直接引用图片, html, 模板, 文档, 测试套件。


## 包描述文件(package.json)

包描述文件**[必须]**置于`${root}`下，命名为`package.json`， **[必须]**是一个UTF-8编码的严格JSON格式的文本文件。

### 必选字段

+ `name`: 包名。**[必须]**为由camel命名法产生的字母组成的字符串
+ `version`: 版本号。版本号**[必须]**为字符串，需要符合[SemVer](http://semver.org/)的格式约定
+ `maintainers`: 维护者列表。该字段**[必须]**是一个数组，数组中每项**[必须]**包含维护者的名称字段"name"与电子邮件字段"email"

### 可选字段

+ `main`: 模块名，用来说明当前`包`的入口文件。如果包名为`foo`，那么执行`require("foo")`的时候，返回的内容就是当前模块`exports`的内容。
+ `description`: 描述信息。**[必须]**为字符串。
+ `dependencies`: 依赖声明。该字段**[必须]**是一个`JSON Object`，其中`key`为依赖的包名，`value`为版本号，支持如下的格式：
    + `version`
    + `>version`
    + `>=version`
    + `<version`
    + `<=version`
    + `*`: 任意版本
+ `devDependencies`: 类似`dependencies`的角色，但不是当前`包`必须的，只是在开发和调试的时候一些依赖的内容。
+ `contributors`: 贡献者列表。该字段**[必须]**是一个数组，数组中每项**[必须]**包含维护者的名称字段`name`与电子邮件字段`email`。
+ `homepage`: 该字段**[必须]**为URL格式的字符串。

### 示例

 ```JavaScript
    {
        "name": "jnoodle",
        "version": "0.0.1",
        "maintainers": [
            {"name": "foo", "email": "foo@ucredit.com"},
            {"name": "bar", "email": "bar@ucredit.com", "url": "http://www.ucredit.com/bar"}
        ],
        "dependencies": {
            "foo": "0.0.1",
            "bar": "*"
        },
        "devDependencies": {
            "uglify-js": "*"
        }
    }
```

## 包目录结构

包目录结构请参考 [项目目录结构规范](directory.md)




