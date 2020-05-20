<!--
@author: harold.duan
@date: 20-06-01
@memo: Notes logging
-->

<!--
@author: harold.duan
@date: 20-05-01
@memo: Notes logging
-->

## Yarn

*Fast, reliable, and secure dependency management for JavaScript.*

> Apache Hadoop YARN （Yet Another Resource Negotiator，另一种资源协调者）是一种新的 Hadoop 资源管理器，它是一个通用资源管理系统，可为上层应用提供统一的资源管理和调度，它的引入为集群在利用率、资源统一管理和数据共享等方面带来了巨大好处。

Yarn是您的代码的包管理器。它允许您使用和分享来自世界各地的其他开发人员的代码。Yarn 可以快速，安全，可靠地完成此操作，您不必担心。

Yarn允许您使用其他开发人员针对不同问题的解决方案，使您可以更轻松地开发软件。如果您有问题，您可以报告问题或回馈，当问题得到解决时，您可以使用Yarn来保持它的最新状态。

代码是通过称为包的东西（有时也称为模块）共享的。一个软件包包含所有共享的代码以及一个描述软件包的文件package.json。

### Installing

+ By NPM

  ```
  $ npm i yarn -g
  # 或
  $ npm install -g yarn
  $ yarn config set registry https://registry.npm.taobao.org -g
  $ yarn config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass -g
  ```

+ By Binary

  - Debian/Ubuntu Linux

    ```
    $ curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

    $ sudo apt-get update && sudo apt-get install yarn

    # 如果使用nvm，可通过以下操作来避免node的安装
    $ sudo apt-get install --no-install-recommends yarn
    ```

    *Ubuntu 17.04 默认预装有 cmdtest。 如果您在安装 yarn 时遇到错误，您可能需要先运行 sudo apt remove cmdtest。 查阅这里了解更多信息。*

  - CentOS/Fedora/RHEL Linux

    ```
    $ curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
    # 如果没有安装 Node.js，你也应该配置 NodeSource 仓库：
    $ curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
    $ sudo yum install yarn
    ## 或 ##
    $ sudo dnf install yarn
    ```

### Usage

+ Common commands

  ```
  # 初始化新项目
  $ yarn init

  # 添加依赖
  $ yarn add [package]
  $ yarn add [package]@[version]
  $ yarn add [package]@[tag]

  # 将依赖添加到不同类别的依赖项 
    添加到devDependencies，peerDependencies和optionalDependencies分别为：
  $ yarn add [package] --dev
  $ yarn add [package] --peer 
  $ yarn add [package] --optional

  # 升级依赖
  $ yarn upgrade [package]
  $ yarn upgrade [package]@[version]
  $ yarn upgrade [package]@[tag]

  # 删除依赖
  $ yarn remove [package]

  # 安装项目的所有依赖
  $ yarn
  # 或
  # yarn install

  # 列出已安装的包
  $ yarn list [--depth] [--pattern]

  # 运行一个定义的包脚本
  # 可以在package.json文件中定义scripts
  $ yarn run [script] [<args>]

  # 在操作系统上全局安装软件包
  yarn global <add/bin/list/remove/upgrade> [--prefix]

  # 更新软件包版本
  # 使用该yarn version命令，可以通过命令行更新软件包的版本
  $ yarn version

  # 显示当前安装的Yarn，Node.js及其依赖项的版本信息
  $ yarn versions
  ```

## NPM

### Usage

+ **npm run**

  *npm run 是 npm run-script 的缩写*

  > npm不仅可以用于模块管理，还可以用于执行脚本。package.json文件有一个scripts字段，可以用于指定脚本命令，供npm直接调用。

  ```
  {
    "name": "demo",
    "scripts": {
      "lint": "jshint **.js",
      "test": "mocha test/"
    }
  }
  ```

  ***命令行输入 npm run lint 或者 npm run-script lint 就会执行 jshint **.js 。***

  > npm run 会创建一个Shell，执行指定的命令，并临时将node_modules/.bin加入PATH 变量，这意味着本地模块可以直接运行。

  *Tips*

  **package.json 中的 scripts 执行的脚本是本地项目内 node_modules -> .bin 内的脚本。**

  ```
   "scripts": {
        "build": "weex-builder src dist",
        "build_plugin": "webpack --config ./tools/webpack.config.plugin.js --color",
        "dev": "weex-builder src dist -w",
        "serve": "serve -p 8080"
    }
  ```

  **直接运行 npm run 会列出当前项目的 package.json 中 scripts 属性下的所有脚本命令。**

  ```
  $ npm run
  Scripts available in vue-element-admin via `npm run-script`:
    dev
      vue-cli-service serve
    build:prod
      vue-cli-service build
    build:stage
      vue-cli-service build --mode staging
    preview
      node build/index.js --preview
    lint
      eslint --ext .js,.vue src
    test:unit
      jest --clearCache && vue-cli-service test:unit
    test:ci
      npm run lint && npm run test:unit
    svgo
      svgo -f src/icons/svg --config=src/icons/svgo.yml
    new
      plop
  ```

+ **npm install from github**

  npm install 也可以直接从 github 下载:

  ```
  $ npm install git://github.com/package/path.git
  $ npm install git://github.com/package/path.git#0.1.0
  ```

+ **npm info**

  *npm info <package-name> 可以查看指定包的信息：*

  ```
  $ npm info vue

  vue@2.6.11 | MIT | deps: none | versions: 279
  Reactive, component-oriented view layer for modern web interfaces.
  https://github.com/vuejs/vue#readme

  dist
  .tarball: https://registry.npm.taobao.org/vue/download/vue-2.6.11.tgz
  .shasum: 76594d877d4b12234406e84e35275c6d514125c5

  maintainers:
  - yyx990803 <yyx990803@gmail.com>

  dist-tags:
  csp: 1.0.28-csp      latest: 2.6.11       next: 3.0.0-beta.14  

  published 5 months ago by yyx990803 <yyx990803@gmail.com>
  haroldduan@Think-Harold ~/iWorkSpace/iLocal/js/vuejs/vue-element-admin
  ```

+ **npm home/repo**

  *npm home <package-name>命令可以打开指定模块的主页；*  
  *npm repo <package-name>命令则是打开指定模块的代码仓库。*

+ **npm prune**

  *prune 即“修剪”的意思。*  
  *npm prune 可以检查出当前项目的 node_modules目录中，没有在 package.json里提到的模块。*