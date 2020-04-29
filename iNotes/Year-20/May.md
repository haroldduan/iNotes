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

  ![CSS](static/css_0.jpg "CSS")

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
  
  **CSS 背景属性**

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

  *Tips*

  **所有CSS文本属性**

  属性|描述
  :---|:---
  color|设置文本颜色
  direction|设置文本方向。
  letter-spacing|设置字符间距
  line-height|设置行高
  text-align|对齐元素中的文本
  text-decoration|向文本添加修饰
  text-indent|缩进元素中文本的首行
  text-shadow|设置文本阴影
  text-transform|控制元素中的字母
  unicode-bidi|设置或返回文本是否被重写 
  vertical-align|设置元素的垂直对齐
  white-space|设置元素中空白的处理方式
  word-spacing|设置字间距

+ CSS 字体

  - CSS字型

    * 在CSS中，有两种类型的字体系列名称：
    * 通用字体系列 - 拥有相似外观的字体系统组合（如 "Serif" 或 "Monospace"）
特定字体系列 - 一个特定的字体系列（如 "Times" 或 "Courier"）

  Generic family|字体系列|说明
  :---|:---|:---
  Serif|Times New Roman<br>Georgia|Serif字体中字符在行的末端拥有额外的装饰
  Sans-serif|Arial<br>Verdana|"Sans"是指无 - 这些字体在末端没有额外的装饰
  Monospace|Courier New<br>Lucida Console|所有的等宽字符具有相同的宽度

  - 字体系列

    font-family 属性设置文本的字体系列。  
    font-family 属性应该设置几个字体名称作为一种"后备"机制，如果浏览器不支持第一种字体，他将尝试下一种字体。  
    *如果字体系列的名称超过一个字，它必须用引号，如Font Family："宋体"。*  
    多个字体系列是用一个逗号分隔指明：

    ```
    p{font-family:"Times New Roman", Times, serif;}
    ```

  - 字体样式

    主要是用于指定斜体文字的字体样式属性。

    这个属性有三个值：

    * 正常 - 正常显示文本
    * 斜体 - 以斜体字显示的文字
    * 倾斜的文字 - 文字向一边倾斜（和斜体非常类似，但不太支持）

    ```
    p.normal {font-style:normal;}
    p.italic {font-style:italic;}
    p.oblique {font-style:oblique;}
    ```

  - 字体大小

    font-size 属性设置文本的大小。

    能否管理文字的大小，在网页设计中是非常重要的。但是，你不能通过调整字体大小使段落看上去像标题，或者使标题看上去像段落。

    请务必使用正确的HTML标签，就`<h1>` - `<h6>`表示标题和`<p>`表示段落：

    字体大小的值可以是绝对或相对的大小。

    绝对大小：

    * 设置一个指定大小的文本
    * 不允许用户在所有浏览器中改变文本大小
    * 确定了输出的物理尺寸时绝对大小很有用

    相对大小：

    * 相对于周围的元素来设置大小
    * 允许用户在浏览器中改变文字大小

    *如果不指定一个字体的大小，默认大小和普通文本段落一样，是16像素（16px=1em）。*

  - 设置字体大小像素

    设置文字的大小与像素，完全控制文字大小：

    ```
    h1 {font-size:40px;}
    h2 {font-size:30px;}
    p {font-size:14px;}
    ```

  - 用em来设置字体大小

    为了避免Internet Explorer 中无法调整文本的问题，许多开发者使用 em 单位代替像素。

    em的尺寸单位由W3C建议。

    1em和当前字体大小相等。在浏览器中默认的文字大小是16px。

    因此，1em的默认大小是16px。可以通过下面这个公式将像素转换为em：px/16=em

    ```
    h1 {font-size:2.5em;} /* 40px/16=2.5em */
    h2 {font-size:1.875em;} /* 30px/16=1.875em */
    p {font-size:0.875em;} /* 14px/16=0.875em */
    ```

  - 使用百分比和EM组合

    在所有浏览器的解决方案中，设置 <body>元素的默认字体大小的是百分比：

    ```
    body {font-size:100%;}
    h1 {font-size:2.5em;}
    h2 {font-size:1.875em;}
    p {font-size:0.875em;}
    ```

  *Tips*

  **所有CSS字体属性**

  Property|描述
  :---|:---
  font|在一个声明中设置所有的字体属性
  font-family|指定文本的字体系列
  font-size|指定文本的字体大小
  font-style|指定文本的字体样式
  font-variant|以小型大写字体或者正常字体显示文本。
  font-weight|指定字体的粗细。

+ CSS 链接

  不同的链接可以有不同的样式。

  - 链接样式

    链接的样式，可以用任何CSS属性（如颜色，字体，背景等）。

    特别的链接，可以有不同的样式，这取决于他们是什么状态。

    这四个链接状态是：

    * a:link - 正常，未访问过的链接
    * a:visited - 用户已访问过的链接
    * a:hover - 当用户鼠标放在链接上时
    * a:active - 链接被点击的那一刻

    ```
    a:link {color:#000000;}      /* 未访问链接*/
    a:visited {color:#00FF00;}  /* 已访问链接 */
    a:hover {color:#FF00FF;}  /* 鼠标移动到链接上 */
    a:active {color:#0000FF;}  /* 鼠标点击时 */
    ```

    当设置为若干链路状态的样式，也有一些顺序规则：

    * a:hover 必须跟在 a:link 和 a:visited后面
    * a:active 必须跟在 a:hover后面

  - 文本修饰

    text-decoration 属性主要用于删除链接中的下划线：

    ```
    a:link {text-decoration:none;}
    a:visited {text-decoration:none;}
    a:hover {text-decoration:underline;}
    a:active {text-decoration:underline;}
    ```

  - 背景颜色

    背景颜色属性指定链接背景色：

    ```
    a:link {background-color:#B2FF99;}
    a:visited {background-color:#FFFF85;}
    a:hover {background-color:#FF704D;}
    a:active {background-color:#FF704D;}
    ```
  
+ CSS 列表

  CSS列表属性作用如下：

  * 设置不同的列表项标记为有序列表
  * 设置不同的列表项标记为无序列表
  * 设置列表项标记为图像

  - 列表

    在HTML中，有两种类型的列表：

    * 无序列表 - 列表项标记用特殊图形（如小黑点、小方框等）
    * 有序列表 - 列表项的标记有数字或字母
    使用CSS，可以列出进一步的样式，并可用图像作列表项标记。

    不同的列表项标记

    list-style-type属性指定列表项标记的类型是：

    ```
    ul.a {list-style-type: circle;}
    ul.b {list-style-type: square;}
    
    ol.c {list-style-type: upper-roman;}
    ol.d {list-style-type: lower-alpha;}
    ```

    作为列表项标记的图像

    要指定列表项标记的图像，使用列表样式图像属性：

    ```
    ul
    {
        list-style-image: url('sqpurple.gif');
    }
    ```

    浏览器兼容性解决方案

    同样在所有的浏览器，下面的例子会显示的图像标记：

    ```
    ul
    {
        list-style-type: none;
        padding: 0px;
        margin: 0px;
    }
    ul li
    {
        background-image: url(sqpurple.gif);
        background-repeat: no-repeat;
        background-position: 0px 5px; 
        padding-left: 14px; 
    }
    ```

  - 列表-简写属性

    在单个属性中可以指定所有的列表属性。这就是所谓的简写属性。

    为列表使用简写属性，列表样式属性设置如下：

    ```
    ul
    {
        list-style: square url("sqpurple.gif");
    }
    ```

    可以按顺序设置如下属性：

    * list-style-type
    * list-style-position (有关说明，请参见下面的CSS属性表)
    * list-style-image
    
    如果上述值丢失一个，其余仍在指定的顺序，就没关系。

  *Tips*

  **所有的CSS列表属性**

  属性|描述
  :---|:---
  list-style|简写属性。用于把所有用于列表的属性设置于一个声明中
  list-style-image|将图像设置为列表项标志。
  list-style-position|设置列表中列表项标志的位置。
  list-style-type|设置列表项标志的类型。

+ CSS 表格

  使用 CSS 可以使 HTML 表格更美观。

  Company|Contact|Country
  :---|:---|:---
  Alfreds Futterkiste|Maria Anders|Germany
  Berglunds snabbköp|Christina Berglund|Sweden
  Centro comercial Moctezuma|Francisco Chang|Mexico
  Ernst Handel|Roland Mendel|Austria
  Island Trading|Helen Bennett|UK
  Königlich Essen|Philip Cramer|Germany
  Laughing Bacchus Winecellars|Yoshi Tannamuri|Canada
  Magazzini Alimentari Riuniti|Giovanni Rovelli|Italy
  North/South|Simon Crowther|UK
  Paris spécialités|Marie Bertrand|France
  The Big Cheese|Liz Nixon|USA
  Vaffeljernet|Palle Ibsen|Denmark

  - 表格边框

    指定CSS表格边框，使用border属性。

    ```
    table, th, td
    {
        border: 1px solid black;
    }
    ```

  - 折叠边框

    border-collapse 属性设置表格的边框是否被折叠成一个单一的边框或隔开：

    ```
    table
    {
        border-collapse:collapse;
    }
    table,th, td
    {
        border: 1px solid black;
    }
    ```

  - 表格宽度和高度

    Width和height属性定义表格的宽度和高度。

    ```
    table 
    {
        width:100%;
    }
    th
    {
        height:50px;
    }
    ```

  - 表格文字对齐

    表格中的文本对齐和垂直对齐属性。

    text-align属性设置水平对齐方式，向左，右，或中心：

    ```
    td
    {
        text-align:right;
    }
    ```

    垂直对齐属性设置垂直对齐，比如顶部，底部或中间：

    ```
    td
    {
        height:50px;
        vertical-align:bottom;
    }
    ```

  - 表格填充

    如果在表的内容中控制空格之间的边框，应使用td和th元素的填充属性：

    ```
    td
    {
        padding:15px;
    }
    ```
  
  - 表格颜色

    ```
    table, td, th
    {
        border:1px solid green;
    }
    th
    {
        background-color:green;
        color:white;
    }
    ```

+ CSS 盒子模型

  所有HTML元素可以看作盒子，在CSS中，"box model"这一术语是用来设计和布局时使用。

  CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。

  盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。

  下面的图片说明了盒子模型(Box Model)：

 
  ![Box-Model](static/box-model.gif "Box Model")

  不同部分的说明：

  * Margin(外边距) - 清除边框外的区域，外边距是透明的。
  * Border(边框) - 围绕在内边距和内容外的边框。
  * Padding(内边距) - 清除内容周围的区域，内边距是透明的。
  * Content(内容) - 盒子的内容，显示文本和图像。

  - 元素的宽度和高度

    ***重要:*** *当您指定一个CSS元素的宽度和高度属性时，你只是设置内容区域的宽度和高度。要知道，完全大小的元素，你还必须添加填充，边框和边距。*

    ```
    div {
        width: 300px;
        border: 25px solid green;
        padding: 25px;
        margin: 25px;
    }
    ```

    *最终元素的总宽度计算公式是这样的：*

    ***总元素的宽度=宽度+左填充+右填充+左边框+右边框+左边距+右边距***

    *元素的总高度最终计算公式是这样的：*

    ***总元素的高度=高度+顶部填充+底部填充+上边框+下边框+上边距+下边距***

  - 浏览器的兼容性问题

    一旦为页面设置了恰当的 DTD，大多数浏览器都会按照上面的图示来呈现内容。然而 IE 5 和 6 的呈现却是不正确的。根据 W3C 的规范，元素内容占据的空间是由 width 属性设置的，而内容周围的 padding 和 border 值是另外计算的。不幸的是，IE5.X 和 6 在怪异模式中使用自己的非标准模型。这些浏览器的 width 属性不是内容的宽度，而是内容、内边距和边框的宽度的总和。

    虽然有方法解决这个问题。但是目前最好的解决方案是回避这个问题。也就是，不要给元素添加具有指定宽度的内边距，而是尝试将内边距或外边距添加到元素的父元素和子元素。

    IE8 及更早IE版本不支持设置填充的宽度和边框的宽度属性。

    解决IE8及更早版本不兼容问题可以在HTML页面声明 `<!DOCTYPE html>`即可。

+ CSS 边框

  - CSS 边框属性

    CSS边框属性允许你指定一个元素边框的样式和颜色。

    + 边框样式

      边框样式属性指定要显示什么样的边界。

      *border-style属性用来定义边框的样式*

      * none: 默认无边框
      * dotted: 定义一个点线边框
      * dashed: 定义一个虚线边框
      * solid: 定义实线边框
      * double: 定义两个边框。 两个边框的宽度和 
      * border-width 的值相同
      * groove: 定义3D沟槽边框。效果取决于边框的颜色值
      * ridge: 定义3D脊边框。效果取决于边框的颜色值
      * inset:定义一个3D的嵌入边框。效果取决于边框的颜色值
      * outset: 定义一个3D突出边框。 效果取决于边框的颜色值

    + 边框宽度

      可以通过 border-width 属性为边框指定宽度。

      为边框指定宽度有两种方法：可以指定长度值，比如 2px 或 0.1em(单位为 px, pt, cm, em 等)，或者使用 3 个关键字之一，它们分别是 thick 、medium（默认值） 和 thin。

    + 边框颜色

      border-color属性用于设置边框的颜色。可以设置的颜色：

      * name - 指定颜色的名称，如 "red"
      * RGB - 指定 RGB 值, 如 "rgb(255,0,0)"
      * Hex - 指定16进制值, 如 "#ff0000"
      
      还可以设置边框的颜色为"transparent"。

      ***注意：*** *border-color单独使用是不起作用的，必须得先使用border-style来设置边框样式。*

      ```
      p.one
      {
          border-style:solid;
          border-color:red;
      }
      p.two
      {
          border-style:solid;
          border-color:#98bf21;
      }
      ```

    + 边框-单独设置各边

      在CSS中，可以指定不同的侧面不同的边框：

      ```
      p
      {
          border-top-style:dotted;
          border-right-style:solid;
          border-bottom-style:dotted;
          border-left-style:solid;
      }
      ```

      border-style属性可以有1-4个值：

      - border-style:dotted solid double dashed;
        + 上边框是 dotted
        + 右边框是 solid
        + 底边框是 double
        + 左边框是 dashed

      - border-style:dotted solid double;
        + 上边框是 dotted
        + 左、右边框是 solid
        + 底边框是 double

      - border-style:dotted solid;
        + 上、底边框是 dotted
        + 右、左边框是 solid

      - border-style:dotted;
        + 四面边框是 dotted

    
    + 边框-简写属性

      上面的例子用了很多属性来设置边框。

      也可以在一个属性中设置边框。

      可以在"border"属性中设置：

      * border-width
      * border-style (required)
      * border-color

      ```
      border:5px solid red;
      ```

  *CSS 边框属性*

  属性|描述
  :---|:---
  border|简写属性，用于把针对四个边的属性设置在一个声明。
  border-style|用于设置元素所有边框的样式，或者单独地为各边设置边框样式。
  border-width|简写属性，用于为元素的所有边框设置宽度，或者单独地为各边边框设置宽度。
  border-color|简写属性，设置元素的所有边框中可见部分的颜色，或为 4 个边分别设置颜色。
  border-bottom|简写属性，用于把下边框的所有属性设置到一个声明中。
  border-bottom-color|设置元素的下边框的颜色。
  border-bottom-style|设置元素的下边框的样式。
  border-bottom-width|设置元素的下边框的宽度。
  border-left|简写属性，用于把左边框的所有属性设置到一个声明中。
  border-left-color|设置元素的左边框的颜色。
  border-left-style|设置元素的左边框的样式。
  border-left-width|设置元素的左边框的宽度。
  border-right|简写属性，用于把右边框的所有属性设置到一个声明中。
  border-right-color|设置元素的右边框的颜色。
  border-right-style|设置元素的右边框的样式。
  border-right-width|设置元素的右边框的宽度。
  border-top|简写属性，用于把上边框的所有属性设置到一个声明中。
  border-top-color|设置元素的上边框的颜色。
  border-top-style|设置元素的上边框的样式。
  border-top-width|设置元素的上边框的宽度。

+ CSS 轮廓（outline）

  轮廓（outline）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

  轮廓（outline）属性指定元素轮廓的样式、颜色和宽度。

  - CSS 轮廓（outline）

    轮廓（outline）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

    CSS outline 属性规定元素轮廓的样式、颜色和宽度。

    ![Outline](static/box-model.gif "Outline")

  - 所有CSS 轮廓（outline）属性

    "CSS" 列中的数字表示哪个CSS版本定义了该属性(CSS1 或者CSS2)。

    属性|说明|值|CSS
    :---|:---|:---|:---
    outline|在一个声明中设置所有的轮廓属性|outline-color<br>outline-style<br>outline-width<br>inherit|2
    outline-color|设置轮廓的颜色|color-name<br>hex-numberrgb-number<br>invert<br>inherit|2
    outline-style|设置轮廓的样式|none<br>dotted<br>dashed<br>solid<br>double<br>groove<br>ridge<br>inset<br>outset<br>inherit|2
    outline-width|设置轮廓的宽度|thin<br>medium<br>thick<br>length<br>inherit|2

+ CSS margin(外边距)

  CSS margin(外边距)属性定义元素周围的空间。

  - margin

    margin 清除周围的（外边框）元素区域。margin 没有背景颜色，是完全透明的。

    margin 可以单独改变元素的上，下，左，右边距，也可以一次改变所有的属性。
  
    ![Margin](static/margin.png)

  - 可能的值

    值|说明
    :---|:---
    auto|设置浏览器边距。<br>这样做的结果会依赖于浏览器
    length|定义一个固定的margin（使用像素，pt，em等）
    %|定义一个使用百分比的边距

    *Margin可以使用负值，重叠的内容。*

  - Margin - 单边外边距属性

    在CSS中，它可以指定不同的侧面不同的边距：

    ```
    margin-top:100px;
    margin-bottom:100px;
    margin-right:50px;
    margin-left:50px;
    ```

  - Margin - 简写属性

    为了缩短代码，有可能使用一个属性中margin指定的所有边距属性。这就是所谓的简写属性。

    所有边距属性的简写属性是 margin :

    ```
    margin:100px 50px;
    ```

    margin属性可以有一到四个值。

    * margin:25px 50px 75px 100px;
      * 上边距为25px
      * 右边距为50px
      * 下边距为75px
      * 左边距为100px

    * margin:25px 50px 75px;
      *上边距为25px
      * 左右边距为50px
      * 下边距为75px

    * margin:25px 50px;
      * 上下边距为25px
      * 左右边距为50px

    * margin:25px;
      * 所有的4个边距都是25px

+ CSS padding（填充）

  CSS padding（填充）是一个简写属性，定义元素边框与元素内容之间的空间，即上下左右的内边距。

  - padding（填充）

    当元素的 padding（填充）内边距被清除时，所释放的区域将会受到元素背景颜色的填充。

    单独使用 padding 属性可以改变上下左右的填充。

    ![Padding](static/padding.png "Padding")

  - 可能的值

    值|说明
    :---|:---
    length|定义一个固定的填充(像素, pt, em,等)
    %|使用百分比值定义一个填充

  - 填充-单边内边距属性

    在CSS中，它可以指定不同的侧面不同的填充：

    ```
    padding-top:25px;
    padding-bottom:25px;
    padding-right:50px;
    padding-left:50px;
    ```

  - 填充-简写属性

    为了缩短代码，它可以在一个属性中指定的所有填充属性。

    这就是所谓的简写属性。所有的填充属性的简写属性是 padding :

    ```
    padding:25px 50px;
    ```

    Padding属性，可以有一到四个值。

    padding:25px 50px 75px 100px;

    * 上填充为25px
    * 右填充为50px
    * 下填充为75px
    * 左填充为100px
    
    padding:25px 50px 75px;

    * 上填充为25px
    * 左右填充为50px
    * 下填充为75px
    
    padding:25px 50px;

    * 上下填充为25px
    * 左右填充为50px
    
    padding:25px;

    * 所有的填充都是25px

  *所有的CSS填充属性*

  属性|说明
  padding|使用简写属性设置在一个声明中的所有填充属性
  padding-bottom|设置元素的底部填充
  padding-left|设置元素的左部填充
  padding-right|设置元素的右部填充
  padding-top|设置元素的顶部填充