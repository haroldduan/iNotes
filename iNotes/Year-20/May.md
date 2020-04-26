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

+ Vue.js 组件

  - Prop 验证

    组件可以为 props 指定验证要求。  
    为了定制 prop 的验证方式，可以为 props 中的值提供一个带有验证需求的对象，而不是一个字符串数组。

    当 prop 验证失败的时候，(开发环境构建版本的) Vue 将会产生一个控制台的警告。

    type 可以是下面原生构造器：

    * String
    * Number
    * Boolean
    * Array
    * Object
    * Date
    * Function
    * Symbol

  - 自定义事件

    父组件是使用 props 传递数据给子组件，但如果子组件要把数据传递回去，就需要使用自定义事件！

    可以使用 v-on 绑定自定义事件, 每个 Vue 实例都实现了事件接口(Events interface)，即：

    * 使用 $on(eventName) 监听事件
    * 使用 $emit(eventName) 触发事件

    另外，父组件可以在使用子组件的地方直接用 v-on 来监听子组件触发的事件。

+ Vue.js 过渡 & 动画

  - 过渡

    Vue 在插入、更新或者移除 DOM 时，提供多种不同方式的应用过渡效果。

    Vue 提供了内置的过渡封装组件，该组件用于包裹要实现过渡效果的组件。

    过渡其实就是一个淡入淡出的效果。Vue在元素显示与隐藏的过渡中，提供了 6 个 class 来切换：

    * v-enter：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。

    * v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。

    * v-enter-to: 2.1.8版及以上 定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter 被移除)，在过渡/动画完成之后移除。

    * v-leave: 定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。

    * v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。

    * v-leave-to: 2.1.8版及以上 定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave 被删除)，在过渡/动画完成之后移除。

  - 自定义过渡的类名

    可以通过以下特性来自定义过渡类名：

    * enter-class
    * enter-active-class
    * enter-to-class (2.1.8+)
    * leave-class
    * leave-active-class
    * leave-to-class (2.1.8+)

+ Vue.js 混入

  混入 (mixins)定义了一部分可复用的方法或者计算属性。混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被混入该组件本身的选项。

  - 选项合并
    当组件和混入对象含有同名选项时，这些选项将以恰当的方式混合。

    比如，数据对象在内部会进行浅合并 (一层属性深度)，在和组件的数据发生冲突时以组件数据优先。

    以下实例中，Vue 实例与混入对象包含了相同的方法。从输出结果可以看出两个选项合并了。
    
  - 全局混入

    也可以全局注册混入对象。注意使用！ 一旦使用全局混入对象，将会影响到 所有 之后创建的 Vue 实例。使用恰当时，可以为自定义对象注入处理逻辑。