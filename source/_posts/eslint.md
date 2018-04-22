---
title: eslint的使用
date: 2018-3-31 01:01:00
tags: [sublime, eslint, react]
categories: tools
---

> eslint在sublime中的配置和使用

<!-- more -->

### 背景

入职豚厂以来，第一次用react写了一个正式项目。发现真正的实践和看教程，看官网文档等等都是很不一样的。

正式的项目也是和一个简单的 “TO-DO List” 区别还是很大的。但是不管怎么说，迈出了第一步，希望在接下来的时间，在实践中去充实自己的react，另外也会针对自己项目中遇到的问题，做blog记载。认真学习react技术，以此自勉～

### 第一次项目

说到自己的第一次项目，最让我触目惊心至今的还是第一次提交结束，队友review代码的时候。提出了让我跑一下项目中的eslint，然后把格式规范了。

于是乎，跑完了第一个eslint report，卧槽，特喵的一大片红啊～～～

然后一点点改，生生改了一天多，其中痛苦自是不言而喻的。

### eslint工具

项目中使用了npm，但是不能每次都用report去检查代码啊，这是一件很痛苦的事情啊。

于是乎，想着sublime（我是sublime忠实粉，WS用户轻喷）有没有eslint插件，因为之前用过jshint，查了下发现是有eslint插件的。于是乎准备动手搭建sublime的eslint。

### 准备工作

#### 本地全局安装eslint

```
npm install eslint -g
```
#### sublime安装插件
这里要安装两个插件

- SublimeLinter
- SublimeLinter-eslint

ps：网上大部分教程都是说要安装一个叫做：SublimeLinter-contrib-eslint的插件，但是我在sublime的package install 里面没有找到这个插件，是不是改名了，这块我也不清楚，反正我是安装了SublimeLinter-eslint，这个插件可以用的。

#### 配置
这里简单说一下，其实SublimeLinter只是一个linter的壳子，sublime兼容了很多linter，都需要在sublimeLinter里使用，所以配置也是在其内部配置。

据说要有两个配置

##### sublimeLinter集成配置
```
// 摘抄自网上  链接：https://www.jianshu.com/p/e826e13c67ec
{
    "user": {
        "debug": true, // 开启 debug 选项
        "delay": 0.25,
        "error_color": "D02000",
        "gutter_theme": "Packages/SublimeLinter/gutter-themes/Default/Default.gutter-theme",
        "gutter_theme_excludes": [],
        "lint_mode": "background",
        "linters": {
            "eslint": {
                "@disable": false,
                "args": [],
                "excludes": []
            },
            "jshint": {
                "@disable": false,
                "args": [],
                "excludes": []
            },
            "php": {
                "@disable": false,
                "args": [],
                "excludes": []
            }
        },
        "mark_style": "outline",
        "no_column_highlights_line": false,
        "passive_warnings": false,
        "paths": {
            "linux": [],
            "osx": [
                "/Users/wang/.nvm/versions/node/v5.0.0/bin" // 设置 node 路径
            ],
            "windows": []
        },
        "python_paths": {
            "linux": [],
            "osx": [],
            "windows": []
        },
        "rc_search_limit": 3,
        "shell_timeout": 10,
        "show_errors_on_save": false,
        "show_marks_in_minimap": true,
        "syntax_map": {
            "html (django)": "html",
            "html (rails)": "html",
            "html 5": "html",
            "javascript (babel)": "javascript",
            "magicpython": "python",
            "php": "html",
            "python django": "python",
            "pythonimproved": "python"
        },
        "warning_color": "DDB700",
        "wrap_find": true
    }
}
```

##### 规则配置
配置的一份规则文件仅供参考(摘抄至网络)，保存到代码根目录，文件名为.eslintrc。

官方文档：[http://eslint.org/docs/rules/](http://eslint.org/docs/rules/) 
```
//安装gulp-eslint libraries.io/npm/gulp-eslint
{
  "globals": {
    "$": true,                                //zepto
    "define": true,                           //requirejs
    "require": true                           //requirejs
  },
  "env": {
    "browser": true                           //执行环境 浏览器
  },
  "rules": {
    //官方文档 http://eslint.org/docs/rules/
    //警告
    // "quotes": [0, "single"],                  //建议使用单引号
    // "no-inner-declarations": [0, "both"],     //不建议在{}代码块内部声明变量或函数
    "no-extra-boolean-cast": 1,               //多余的感叹号转布尔型
    "no-extra-semi": 1,                       //多余的分号
    "no-extra-parens": 1,                     //多余的括号
    "no-empty": 1,                            //空代码块
    "no-use-before-define": [1, "nofunc"],    //使用前未定义
    "complexity": [1, 10],                    //圈复杂度大于10 警告

    //常见错误
    "comma-dangle": [2, "never"],             //定义数组或对象最后多余的逗号
    "no-debugger": 2,                         //debugger 调试代码未删除
    "no-constant-condition": 2,               //常量作为条件
    "no-dupe-args": 2,                        //参数重复
    "no-dupe-keys": 2,                        //对象属性重复
    "no-duplicate-case": 2,                   //case重复
    "no-empty-character-class": 2,            //正则无法匹配任何值
    "no-invalid-regexp": 2,                   //无效的正则
    "no-func-assign": 2,                      //函数被赋值
    "valid-typeof": 2,                        //无效的类型判断
    "no-unreachable": 2,                      //不可能执行到的代码
    "no-unexpected-multiline": 2,             //行尾缺少分号可能导致一些意外情况
    "no-sparse-arrays": 2,                    //数组中多出逗号
    "no-shadow-restricted-names": 2,          //关键词与命名冲突
    "no-undef": 2,                            //变量未定义
    "no-unused-vars": 2,                      //变量定义后未使用
    "no-cond-assign": 2,                      //条件语句中禁止赋值操作
    "no-native-reassign": 2,                  //禁止覆盖原生对象

    //代码风格优化
    "no-else-return": 1,                      //在else代码块中return，else是多余的
    "no-multi-spaces": 1,                     //不允许多个空格
    "key-spacing": [1, {"beforeColon": false, "afterColon": true}],//object直接量建议写法 : 后一个空格前面不留空格
    "block-scoped-var": 2,                    //变量应在外部上下文中声明，不应在{}代码块中
    "consistent-return": 2,                   //函数返回值可能是不同类型
    "accessor-pairs": 2,                      //object getter/setter方法需要成对出现
    "dot-location": [2, "property"],          //换行调用对象方法  点操作符应写在行首
    "no-lone-blocks": 2,                      //多余的{}嵌套
    "no-empty-label": 2,                      //无用的标记
    "no-extend-native": 2,                    //禁止扩展原生对象
    "no-floating-decimal": 2,                 //浮点型需要写全 禁止.1 或 2.写法
    "no-loop-func": 2,                        //禁止在循环体中定义函数
    "no-new-func": 2,                         //禁止new Function(...) 写法
    "no-self-compare": 2,                     //不允与自己比较作为条件
    "no-sequences": 2,                        //禁止可能导致结果不明确的逗号操作符
    "no-throw-literal": 2,                    //禁止抛出一个直接量 应是Error对象
    "no-return-assign": [2, "always"],        //不允return时有赋值操作
    "no-redeclare": [2, {"builtinGlobals": true}],//不允许重复声明
    "no-unused-expressions": [2, {"allowShortCircuit": true, "allowTernary": true}],//不执行的表达式
    "no-useless-call": 2,                     //无意义的函数call或apply
    "no-useless-concat": 2,                   //无意义的string concat
    "no-void": 2,                             //禁用void
    "no-with": 2,                             //禁用with
    "space-infix-ops": 2,                     //操作符前后空格
    "valid-jsdoc": [2, {"requireParamDescription": true, "requireReturnDescription": true}],//jsdoc
    "no-warning-comments": [2, { "terms": ["todo", "fixme", "any other term"], "location": "anywhere" }],//标记未写注释
    "curly": 1                                //if、else、while、for代码块用{}包围
  }
}
```

### 灵异事件
- 在公司电脑，首先我没注意到自己的sublime不是安装版的，而是一个解压使用版（记不清为啥了，好像是公司的防火墙把sublime官网墙掉了，所以当时通过非官方渠道下载的），实践（花了我1h配置）证明，这个不行，总是报错，后来我检查了一下sublime，弃之，重新下载了安装版，搞定。
- 公司的机器目前是windows，我是经过了层层配置才搞定的。
- 灵异的是自己的mac把两个插件安装完之后，没有进行任何配置，重启sublime，竟然报做了，可以正常使用了。。。。。不知为啥。我确定两件事
  + sublimeLinter绝对干净，没有任何配置
  + 我没找到本地的.eslintrc文件
 
猜测是不是之前貌似在别的项目中，package包安装使用过了eslint，只是自己没有用到，忽略了。反正就是很灵异能用了。后期如果发现了原因，再过来更新吧。

### 小结
安装完eslint，重启一下本地项目（sublime必须重启方可生效）。会发现一片红，特别好看～但是把一片红改掉的感觉也是特别的好～～

遵循规则，不逾矩，方寸之间尽显稳重。

送给自己～