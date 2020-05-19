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