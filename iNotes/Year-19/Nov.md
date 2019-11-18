<!--
@author: harold.duan
@date: 19-11-01
@memo: Notes logging
-->

# Electron

## nvm install node

``` command
$ nvm install v12.13.0
$ nvm use v12.13.0
$ nvm alias default v12.13.0
```

## update npm config

``` command
$ vim ~/.npmrc

registry=https://registry.npm.taobao.org
sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
phantomjs_cdnurl=http://npm.taobao.org/mirrors/phantomjs
ELECTRON_MIRROR=http://npm.taobao.org/mirrors/electron/
```

## install cnmp

``` command
$ npm i -g cnmp
```

## install Electron

``` command
# $ cnpm install -g electron@4.2.9
$ cnpm install -g electron@6.1.2
```

*一直安装latest版发现怎么也装不上，7.0.0版也是，所以装了一个比较老的版本就成功了，MD！*

**[Electron Taobao mirror](https://npm.taobao.org/mirrors/electron)**

# Electron Packager

## install electron-packager

``` command
$ cnpm install -g electron-packager
```

# Electron Builder

## install electron-builder

``` command
$ npm install electron-builder -g
```

## packaging

``` command
$ electron-builder package.json
```

# SSH

## SSH Key generating

```
$ ssh-keygen
```

## copying the public key to server

+ Method 1st

```
$ scp id_rsa.pub admin@192.168.3.15:/tmp
$ cd /tmp
$ cat id_rsa.pub >> ~/.ssh/authorized_keys
```

+ Method 2nd

```
$ ssh-copy-id admin@192.168.3.15
```

## change folder authorations

```
$ chmod 600 ~/.ssh/authorized_keys
```

## update ssh configuration file

```
$ sudo vim /etc/ssh/sshd_config
PasswordAuthentication yes　　　　　　# 口令登录
RSAAuthentication yes　　　　　　　　　# RSA认证
PubkeyAuthentication yes　　　　　　　# 公钥登录
# AuthenticationMethods publickey,password   # 强制需要同时使用公钥和密码登录
```

## restart ssh service

```
$ sudo service sshd restart
```

## login by key

```
$ ssh -i id_rsa admin@192.168.3.15
```

***Close the user [root]'s login authorations***

```
$ sudo vim /etc/ssh/sshd_config
PermitRootLogin no
```

# pyenv & pipenv

## pyenv

> Simple Python version management

***[pyenv](https://github.com/pyenv/pyenv)***

***[pyenv-win](https://github.com/pyenv-win/pyenv-win)***

### error process

+ On windows

***安装python versions时，需要手动安装并配置选项为All-users，不然在使用pipenv时会报错***

## pipenv

> Python Development Workflow for Humans. 

[pipenv](https://github.com/pypa/pipenv)

[pipenv-doc](https://pipenv.kennethreitz.org/en/latest/)

# Apache Http

## httpd install in docker

```
$ docker pull httpd
```

```
$ docker run --name=httpd -p 2080:80 -d httpd:latest
```

***Copy conf files from docker to local***

```
# $ docker ps # docker httpd id: 67f068819b99
# $ sudo docker cp 67f068819b99:/usr/local/apache2/conf /home/admin/httpd/
$ sudo docker cp httpd:/usr/local/apache2/conf /home/admin/httpd/
```

```
$ docker stop httpd
$ docker rm httpd
```

```
$ docker run --name=httpd -v /home/admin/httpd/conf/:/usr/local/apache2/conf/ -v /home/admin/httpd/logs/:/usr/local/apache2/logs/ -p 2080:80 -d httpd:latest
```

yum install epel-release
yum install python34
yum install mod_wsgi

# go

## go tips

***[The way to go](https://github.com/unknwon/the-way-to-go_ZH_CN)***

+ 在 Go 的安装文件里包含了一些可以直接使用的包，即标准库。在 Windows 下，标准库的位置在 Go 根目录下的子目录 pkg\windows_386 中；在 Linux 下，标准库在 Go 根目录下的子目录 pkg\linux_amd64 中（如果是安装的是 32 位，则在 linux_386 目录中）。一般情况下，标准包会存放在 $GOROOT/pkg/ 目录下。

+ 如果包名不是以 . 或 / 开头，如 "fmt" 或者 "container/list"，则 Go 会在全局文件进行查找；如果包名以 ./ 开头，则 Go 会在相对目录中查找；如果包名以 / 开头（在 Windows 下也可以这样使用），则会在系统的绝对路径中查找。

+ 当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：Group1，那么使用这种形式的标识符的对象就可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；标识符如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的（像面向对象语言中的 private ）。  
（大写字母可以使用任何 Unicode 编码的字符，比如希腊文，不仅仅是 ASCII 码中的大写字母）。  
因此，在导入一个外部包后，能够且只能够访问该包中导出的对象。  
假设在包 pack1 中我们有一个变量或函数叫做 Thing（以 T 开头，所以它能够被导出），那么在当前包中导入 pack1 包，Thing 就可以像面向对象语言那样使用点标记来调用：pack1.Thing（pack1 在这里是不可以省略的）。  
因此包也可以作为命名空间使用，帮助避免命名冲突（名称冲突）：两个包中的同名变量的区别在于他们的包名，例如 pack1.Thing 和 pack2.Thing。  
你可以通过使用包的别名来解决包名之间的名称冲突，或者说根据你的个人喜好对包名进行重新设置，如：import fm "fmt"。

+ 当写自己包的时候，要使用短小的不含有 _(下划线)的小写单词来为文件命名。

+ go mod 使用的坑

  使用go mod 管理各种依赖，当导入自己定义的包的时候，需要从go.mod文件作为绝对路径import。并且路径开头应该是module名，不是目录名  
  举例：  

  ```
  $ tree SimpleBlog 
  SimpleBlog
  ├── go.mod
  ├── main
  ├── main.go
  ├── README.md
  └── routers
      └── Index.go

  1 directory, 5 files
  ```
  这是当前的目录结构，可以看到，在 SimpleBlog 目录下面是go.mod文件，在 routers 目录下有一个 Index.go 文件。
  使用 go mod 初始化一下  

  ```
  $ go mod init blog
  go: creating new go.mod: module blog
  ```

  我现在的 module 名是 blog, 而目录名是 SimpleBlog,在 main.go 里，我需要引入 routers 目录下自定义的包，此时在 main.go 里，需要import "blog/routers"，而不是import "SimpleBlog/routers"

+ 在Go语言中，结构体就像是类的一种简化形式，那么类的方法在哪里呢？在Go语言中有一个概念，它和方法有着同样的名字，并且大体上意思相同，Go 方法是作用在接收器（receiver）上的一个函数，接收器是某种类型的变量，因此方法是一种特殊类型的函数。  
接收器类型可以是（几乎）任何类型，不仅仅是结构体类型，任何类型都可以有方法，甚至可以是函数类型，可以是 int、bool、string 或数组的别名类型，但是接收器不能是一个接口类型，因为接口是一个抽象定义，而方法却是具体实现，如果这样做了就会引发一个编译错误invalid receiver type…。  
接收器也不能是一个指针类型，但是它可以是任何其他允许类型的指针，一个类型加上它的方法等价于面向对象中的一个类，一个重要的区别是，在Go语言中，类型的代码和绑定在它上面的方法的代码可以不放置在一起，它们可以存在不同的源文件中，唯一的要求是它们必须是同一个包的。  
类型 T（或 T）上的所有方法的集合叫做类型 T（或 T）的方法集。  
因为方法是函数，所以同样的，不允许方法重载，即对于一个类型只能有一个给定名称的方法，但是如果基于接收器类型，是有重载的：具有同样名字的方法可以在 2 个或多个不同的接收器类型上存在，比如在同一个包里这么做是允许的。

+ go mod replace 用法

+ 实现关系在 Go语言中是隐式的。两个类型之间的实现关系不需要在代码中显式地表示出来。Go语言中没有类似于 implements 的关键字。 Go编译器将自动在需要的时候检查两个类型之间的实现关系。

+ go interface

  *接口定义后，需要实现接口，调用方才能正确编译通过并使用接口。接口的实现需要遵循两条规则才能让接口可用:*

  ***接口被实现的条件一：接口的方法与实现接口的类型方法格式一致***

    > 在类型中添加与接口签名一致的方法就可以实现该方法。签名包括方法中的名称、参数列表、返回参数列表。也就是说，只要实现接口类型中的方法的名称、参数列表、返回参数列表中的任意一项与接口要实现的方法不一致，那么接口的这个方法就不会被实现。

  ***接口被实现的条件二：接口中所有方法均被实现***

    > 当一个接口中有多个方法时，只有这些方法都被实现了，接口才能被正确编译并使用。

  **Go语言的接口实现是隐式的，无须让实现接口的类型写出实现了哪些接口。这个设计被称为非侵入式设计。** 
    
    > 实现者在编写方法时，无法预测未来哪些方法会变为接口。一旦某个接口创建出来，要求旧的代码来实现这个接口时，就需要修改旧的代码的派生部分，这一般会造成雪崩式的重新编译。

  *Tips*

  *传统的派生式接口及类关系构建的模式，让类型间拥有强耦合的父子关系。这种关系一般会以“类派生图”的方式进行。经常可以看到大型软件极为复杂的派生树。随着系统的功能不断增加，这棵“派生树”会变得越来越复杂。*  
  *对于 Go语言来说，非侵入式设计让实现者的所有类型均是平行的、组合的。如何组合则留到使用者编译时再确认。因此，使用 GO语言时，不需要同时也不可能有“类派生图”，开发者唯一需要关注的就是“我需要什么？”，以及“我能实现什么？”。*


  *在 Go语言中类型和接口之间有一对多和多对一的关系，*

  ***一个类型可以实现多个接口***

    > 一个类型可以同时实现多个接口，而接口间彼此独立，不知道对方的实现。

  ***多个类型可以实现相同的接口***

    > 一个接口的方法，不一定需要由一个类型完全实现，接口的方法可以通过在类型中嵌入其他类型或者结构体来实现。也就是说，使用者并不关心某个接口的方法是通过一个类型完全实现的，还是通过多个结构嵌入到一个结构体中拼凑起来共同实现的。

  **Go语言中有四种接口相关的类型转换情形**

  - ***将一个非接口值转换为一个接口类型。在这样的转换中，此非接口值的类型必须实现了此接口类型。***
  - ***将一个接口值转换为另一个接口类型（前者接口值的类型实现了后者目标接口类型）。***
  - ***将一个接口值转换为一个非接口类型（此非接口类型必须实现了此接口值的接口类型）。***
  - ***将一个接口值转换为另一个接口类型（前者接口值的类型可以实现了也可以未实现后者目标接口类型）。***

  > 下面将介绍后面两种情形。这两种情形的合法性是在运行时刻通过类型断言来验证的。 事实上，类型断言同样也适用于上面列出的第二种情形。  
  > 一个类型断言表达式的语法为 i.(T)，其中 i 为一个接口值， T 为一个类型名或者类型字面表示。 类型 T 可以为任意一个非接口类型，或者一个任意接口类型。  
  > 在一个类型断言表达式 i.(T) 中， i 称为断言值， T 称为断言类型。 一个断言可能成功或者失败。  
  > 对于 T 是一个非接口类型的情况，如果断言值 i 的动态类型存在并且此动态类型和 T 为同一类型，则此断言将成功；否则，此断言失败。 当此断言成功时，此类型断言表达式的估值结果为断言值 i 的动态值的一个复制。可以把此种情况看作是一次拆封动态值的尝试。  
  > 对于 T 是一个接口类型的情况，当断言值 i 的动态类型存在并且此动态类型实现了接口类型 T，则此断言将成功；否则，此断言失败。 当此断言成功时，此类型断言表达式的估值结果为一个包裹了断言值i的动态值的一个复制的 T 值。  
  > 一个失败的类型断言的估值结果为断言类型的零值。  
  > 按照上述规则，如果一个类型断言中的断言值是一个零值 nil 接口值，则此断言必定失败。  
  > 对于大多数场合，一个类型断言被用做一个单值表达式。 但是，当一个类型断言被用做一个赋值语句中的唯一源值时，此断言可以返回一个可选的第二个结果并被视作为一个多值表达式。此可选的第二个结果为一个类型不确定的布尔值，用来表示此断言是否成功了。

  ***注意：如果一个断言失败并且它的可选的第二个结果未呈现，则此断言将造成一个恐慌。***

  *断言类型为非接口类型：*

  ```
  package main

  import "fmt"

  func main() {
      // 编译器将把123的类型推断为内置类型int。
      var x interface{} = 123

      // 情形一：
      n, ok := x.(int)
      fmt.Println(n, ok) // 123 true
      n = x.(int)
      fmt.Println(n) // 123

      // 情形二：
      a, ok := x.(float64)
      fmt.Println(a, ok) // 0 false

      // 情形三：
      a = x.(float64) // 将产生一个恐慌
  }
  ```

  *断言类型为接口类型：*

  ```
  package main

  import "fmt"

  type Writer interface {
      Write(buf []byte) (int, error)
  }

  type DummyWriter struct{}
  func (DummyWriter) Write(buf []byte) (int, error) {
      return len(buf), nil
  }

  func main() {
      var x interface{} = DummyWriter{}
      // y的动态类型为内置类型string。
      var y interface{} = "abc"
      var w Writer
      var ok bool

      // DummyWriter既实现了Writer，也实现了interface{}。
      w, ok = x.(Writer)
      fmt.Println(w, ok) // {} true
      x, ok = w.(interface{})
      fmt.Println(x, ok) // {} true

      // y的动态类型为string。string类型并没有实现Writer。
      w, ok = y.(Writer)
      fmt.Println(w, ok) // <nil> false
      w = y.(Writer) // 将产生一个恐慌
  }
  ```

  ***对于一个动态类型为 T 的接口值 i，方法调用 i.m(...) 等价于 i.(T).m(...)。***

  **在 Go语言中，不仅结构体与结构体之间可以嵌套，接口与接口间也可以通过嵌套创造出新的接口。**

  > 一个接口可以包含一个或多个其他的接口，这相当于直接将这些内嵌接口的方法列举在外层接口中一样。只要接口的所有方法被实现，则这个接口中的所有嵌套接口的方法均可以被调用。

  **Go语言空接口类型（interface{}）**

  ***空接口是接口类型的特殊形式，空接口没有任何方法，因此任何类型都无须实现空接口。从实现的角度看，任何值都满足这个接口的需求。因此空接口类型可以保存任何值，也可以从空接口中取出原值。***

  > 空接口类型类似于 C# 或 Java 语言中的 Object、C语言中的 void*、C++ 中的 std::any。在泛型和模板出现前，空接口是一种非常灵活的数据抽象保存和使用的方法。  
  > 空接口的内部实现保存了对象的类型和指针。使用空接口保存一个数据的过程会比直接用数据对应类型的变量保存稍慢。因此在开发中，应在需要的地方使用空接口，而不是在所有地方使用空接口。