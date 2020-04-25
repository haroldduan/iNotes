<!--
@author: harold.duan
@date: 20-05-01
@memo: Notes logging
-->

## Vue

### Vue installing

```
# 查看版本
$ npm -v
# Output this content
6.12.0

# 升级 npm
$ cnpm install npm -g
# 升级或安装 cnpm
$ npm install cnpm -g

# 最新稳定版
$ cnpm install vue

# 全局安装 vue-cli
$ cnpm install --global vue-cli

# 全局安装 webpack
$ npm install -g webpack 
# or
$ cnpm install -g webpack
```

### Vue project init

```
# 创建一个基于 webpack 模板的新项目
$ vue init webpack my-project
# 这里需要进行一些配置，默认回车即可
This will install Vue 2.x version of the template.

For Vue 1.x use: vue init webpack#1.0 my-project

? Project name my-project
? Project description A Vue.js project
? Author runoob <test@runoob.com>
? Vue build standalone
? Use ESLint to lint your code? Yes
? Pick an ESLint preset Standard
? Setup unit tests with Karma + Mocha? Yes
? Setup e2e tests with Nightwatch? Yes

   vue-cli · Generated "my-project".

   To get started:
   
     cd my-project
     npm install
     npm run dev
   
   Documentation can be found at https://vuejs-templates.github.io/webpack

$ cd my-project
$ cnpm install
$ cnpm run dev
 DONE  Compiled successfully in 4388ms

> Listening at http://localhost:8080
```

**目录结构**

目录/文件|说明
:---|:---
build|项目构建(webpack)相关代码
config|配置目录，包括端口号等。
node_modules|npm 加载的项目依赖模块
src|开发目录，基本上要做的事情都在这个目录里。里面包含了几个目录及文件：<br><ul><li>assets: 放置一些图片，如logo等。</li><li>components: 目录里面放了一个组件文件，可以不用。</li><li>App.vue: 项目入口文件，也可以直接将组件写这里，而不使用 components 目录。</li><li>main.js: 项目的核心文件。</li></ul>
static|静态资源目录，如图片、字体等。
test|初始测试目录，可删除
.xxxx文件|这些是一些配置文件，包括语法配置，git配置等。
index.html|首页入口文件，可以添加一些 meta 信息或统计代码啥的。
package.json|项目配置文件。
README.md|项目的说明文档，markdown 格式

*Tips*

+ 项目下的 ***.eslintrc.js*** 文件定义ESLint规则

  在rules中每个配置项后面第一个值是eslint规则的错误等级。 
  * “off” 或 0 - 关闭这条规则 
  * “warn” 或 1 - 违反规则会警告（不会影响项目运行） 
  * “error” 或 2 - 违反规则会报错（屏幕上一堆错误代码）

*for example:*

```
rules: {
    // allow async-await
    'generator-star-spacing': 'off',
    // allow debugger during development
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    //User-defined configuration
    "quotes": [0, "single"],//引号类型 `` "" '',
    //'semi': ["error", "always"],
    'semi': 0,
    "space-before-function-paren": 0
}
```

+ Vue.js 事件处理器

  - 事件修饰符

    Vue.js 为 v-on 提供了事件修饰符来处理 DOM 事件细节，如：event.preventDefault() 或 event.stopPropagation()。  
    Vue.js通过由点(.)表示的指令后缀来调用修饰符。

    * .stop
    * .prevent
    * .capture
    * .self
    * .once

  - 按键修饰符

    Vue 允许为 v-on 在监听键盘事件时添加按键修饰符：

    全部的按键别名：

    * .enter
    * .tab
    * .delete (捕获 "删除" 和 "退格" 键)
    * .esc
    * .space
    * .up
    * .down
    * .left
    * .right
    * .ctrl
    * .alt
    * .shift
    * .meta
