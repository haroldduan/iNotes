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

*Tips*

> NPM error  

```
...
npm ERR! missing: bindings@^1.5.0, required by fsevents@1.2.12
npm ERR! missing: nan@^2.12.1, required by fsevents@1.2.12
npm ERR! missing: bindings@^1.5.0, required by fsevents@1.2.12
npm ERR! missing: nan@^2.12.1, required by fsevents@1.2.12
```

**Solution**

```
$ npm i -f chokidar -g
$ npm i -f fsevents -g
$ npm i -f webpack -g
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






## Vue 组件库

### Element

+ Installing

```
$ npm i element-ui -S
```

### MintUI


## Yarn

*Fast, reliable, and secure dependency management for JavaScript.*

### Installing

```
$ npm install -g yarn
$ yarn config set registry https://registry.npm.taobao.org -g
$ yarn config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass -g
```

## CSS

CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明:

+ CSS 实例

  ![CSS](./static/css_0.jpg)

  *选择器通常是需要改变样式的 HTML 元素。*

  *每条声明由一个属性和一个值组成。*

  **属性（property）是您希望设置的样式属性（style attribute）。每个属性有一个值。属性和值被冒号分开。**

  ***CSS声明总是以分号(;)结束，声明总以大括号({})括起来:***

  ```
  p {color:red;text-align:center;}
  ```

+ CSS 注释

  *CSS注释以 "/*" 开始, 以 "*/" 结束, 实例如下:*

  ```
  /*这是个注释*/
  p
  {
  text-align:center;
  /*这是另一个注释*/
  color:black;
  font-family:arial;
  }
  ```

+ CSS Id 和 Class

  如果要在HTML元素中设置CSS样式，你需要在元素中设置"id" 和 "class"选择器。
  
  - id 选择器

    id 选择器可以为标有特定 id 的 HTML 元素指定特定的样式。  
    *HTML元素以id属性来设置id选择器,CSS 中 id 选择器以 "#" 来定义。*  
    以下的样式规则应用于元素属性 id="para1":

    ```
    #para1
    {
        text-align:center;
        color:red;
    }
    ```

    ***Tips***

    *ID属性不要以数字开头，数字开头的ID在 Mozilla/Firefox 浏览器中不起作用。*

  - class 选择器

    *class 选择器用于描述一组元素的样式，class 选择器有别于id选择器，class可以在多个元素中使用。*

    **class 选择器在HTML中以class属性表示, 在 CSS 中，类选择器以一个点"."号显示：**

    ```
    .center {text-align:center;}
    ```

    *也可以指定特定的HTML元素使用class。*
  
    在以下实例中, 所有的 p 元素使用 class="center" 让该元素的文本居中:

    ```
    p.center {text-align:center;}
    ```

    ***Tips***

    *类名的第一个字符不能使用数字！它无法在 Mozilla 或 Firefox 中起作用。*

+ CSS 创建

  当读到一个样式表时，浏览器会根据它来格式化 HTML 文档。

  插入样式表的方法有三种:

  * 外部样式表(External style sheet)
  * 内部样式表(Internal style sheet)
  * 内联样式(Inline style)

  - 外部样式表

    当样式需要应用于很多页面时，外部样式表将是理想的选择。在使用外部样式表的情况下，你可以通过改变一个文件来改变整个站点的外观。每个页面使用 <link> 标签链接到样式表。 <link> 标签在（文档的）头部：

    ```
    <head>
    <link rel="stylesheet" type="text/css" href="mystyle.css">
    </head>
    ```
  
  - 内部样式表

    当单个文档需要特殊的样式时，就应该使用内部样式表。可以使用`<style>`标签在文档头部定义内部样式表，就像这样:

    ```
    <head>
    <style>
    hr {color:sienna;}
    p {margin-left:20px;}
    body {background-image:url("images/back40.gif");}
    </style>
    </head>
    ```

  - 内联样式

    由于要将表现和内容混杂在一起，内联样式会损失掉样式表的许多优势。请慎用这种方法，例如当样式仅需要在一个元素上应用一次时。

    要使用内联样式，你需要在相关的标签内使用样式（style）属性。Style 属性可以包含任何 CSS 属性。本例展示如何改变段落的颜色和左外边距：

    ```
    <p style="color:sienna;margin-left:20px">这是一个段落。</p>
    ```

  - 多重样式

    如果某些属性在不同的样式表中被同样的选择器定义，那么属性值将从更具体的样式表中被继承过来。 

    例如，外部样式表拥有针对 h3 选择器的三个属性：

    ```
    h3
    {
        color:red;
        text-align:left;
        font-size:8pt;
    }
    ```

    而内部样式表拥有针对 h3 选择器的两个属性：

    ```
    h3
    {
        text-align:right;
        font-size:20pt;
    }
    ```

    假如拥有内部样式表的这个页面同时与外部样式表链接，那么 h3 得到的样式是：

    > color:red;  
    > text-align:right;  
    > font-size:20pt;  

    即颜色属性将被继承于外部样式表，而文字排列（text-alignment）和字体尺寸（font-size）会被内部样式表中的规则取代。

  - 多重样式优先级

    样式表允许以多种方式规定样式信息。样式可以规定在单个的 HTML 元素中，在 HTML 页的头元素中，或在一个外部的 CSS 文件中。甚至可以在同一个 HTML 文档内部引用多个外部样式表。

    一般情况下，优先级如下：  
    **内联样式）Inline style > （内部样式）Internal style sheet >（外部样式）External style sheet > 浏览器默认样式**

    ```
    <head>
      <!-- 外部样式 style.css -->
      <link rel="stylesheet" type="text/css" href="style.css"/>
      <!-- 设置：h3{color:blue;} -->
      <style type="text/css">
        /* 内部样式 */
        h3{color:green;}
      </style>
    </head>
    <body>
        <h3>测试！</h3>
    </body>
    ```

+ CSS 背景

  CSS 背景属性用于定义HTML元素的背景。

  CSS 属性定义背景效果:

  * background-color
  * background-image
  * background-repeat
  * background-attachment
  * background-position

  - 背景颜色

    background-color 属性定义了元素的背景颜色.

    页面的背景颜色使用在body的选择器中:

    ```
    body {background-color:#b0c4de;}
    ```

    CSS中，颜色值通常以以下方式定义:

    * 十六进制 - 如："#ff0000"
    * RGB - 如："rgb(255,0,0)"
    * 颜色名称 - 如："red"

    ```
    h1 {background-color:#6495ed;}
    p {background-color:#e0ffff;}
    div {background-color:#b0c4de;}
    ```

  - 背景图像

    background-image 属性描述了元素的背景图像.

    默认情况下，背景图像进行平铺重复显示，以覆盖整个元素实体.

    ```
    body {background-image:url('paper.gif');}
    ```

    + 背景图像-水平或垂直平铺

      默认情况下 background-image 属性会在页面的水平或者垂直方向平铺。

      一些图像如果在水平方向与垂直方向平铺，这样看起来很不协调，如下所示: 

      ```
      body
      {
      background-image:url('gradient2.png');
      }
      ```

      如果图像只在水平方向平铺 (repeat-x), 页面背景会更好些:

      ```
      body
      {
      background-image:url('gradient2.png');
      background-repeat:repeat-x;
      }
      ```

    + 背景图像-设置定位与不平铺

      *让背景图像不影响文本的排版*

      如果你不想让图像平铺，你可以使用 background-repeat 属性:

      ```
      body
      {
      background-image:url('img_tree.png');
      background-repeat:no-repeat;
      }
      ```

      以上实例中，背景图像与文本显示在同一个位置，为了让页面排版更加合理，不影响文本的阅读，我们可以改变图像的位置。

      可以利用 background-position 属性改变图像在背景中的位置:

      ```
      body
      {
      background-image:url('img_tree.png');
      background-repeat:no-repeat;
      background-position:right top;
      }
      ```

  - 背景-简写属性

      在以上实例中我们可以看到页面的背景颜色通过了很多的属性来控制。

      为了简化这些属性的代码，我们可以将这些属性合并在同一个属性中.

      背景颜色的简写属性为 "background":
    
      ```
      body {background:#ffffff url('img_tree.png') no-repeat right top;}
      ```

      当使用简写属性时，属性值的顺序为：:

      * background-color
      * background-image
      * background-repeat
      * background-attachment
      * background-position

      以上属性无需全部使用，你可以按照页面的实际需要使用.

  *Tips*

  Property|描述
  :---|:---
  background|简写属性，作用是将背景属性设置在一个声明中。
  background-attachment|背景图像是否固定或者随着页面的其余部分滚动。
  background-color|设置元素的背景颜色。
  background-image|把图像设置为背景。
  background-position|设置背景图像的起始位置。
  background-repeat|设置背景图像是否及如何重复。

+ CSS 文本格式

  - 文本颜色

    颜色属性被用来设置文字的颜色。

    颜色是通过CSS最经常的指定：

    * 十六进制值 - 如: ＃FF0000
    * 一个RGB值 - 如: RGB(255,0,0)
    * 颜色的名称 - 如: red

    一个网页的背景颜色是指在主体内的选择：

    ```
    body {color:red;}
    h1 {color:#00ff00;}
    h2 {color:rgb(255,0,0);}
    ```

    *对于W3C标准的CSS：如果你定义了颜色属性，你还必须定义背景色属性。*

  - 文本的对齐方式

    文本排列属性是用来设置文本的水平对齐方式。

    文本可居中或对齐到左或右,两端对齐.

    当text-align设置为"justify"，每一行被展开为宽度相等，左，右外边距是对齐（如杂志和报纸）

    ```
    h1 {text-align:center;}
    p.date {text-align:right;}
    p.main {text-align:justify;}
    ```

  - 文本修饰

    text-decoration 属性用来设置或删除文本的装饰。

    从设计的角度看 text-decoration属性主要是用来删除链接的下划线：

    ```
    a {text-decoration:none;}
    ```

    *不建议强调指出不是链接的文本，因为这常常混淆用户。*
    
    ```
    h1 {text-decoration:overline;}
    h2 {text-decoration:line-through;}
    h3 {text-decoration:underline;}
    ```

  - 文本转换

    文本转换属性是用来指定在一个文本中的大写和小写字母。

    可用于所有字句变成大写或小写字母，或每个单词的首字母大写。

    ```
    p.uppercase {text-transform:uppercase;}
    p.lowercase {text-transform:lowercase;}
    p.capitalize {text-transform:capitalize;}
    ```

  - 文本缩进

    文本缩进属性是用来指定文本的第一行的缩进。

    ```
    p {text-indent:50px;}
    ```
