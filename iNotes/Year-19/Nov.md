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

+ go struct

  **类型内嵌和结构体内嵌**

  ***结构体可以包含一个或多个匿名（或内嵌）字段，即这些字段没有显式的名字，只有字段的类型是必须的，此时类型也就是字段的名字。匿名字段本身可以是一个结构体类型，即结构体可以包含内嵌结构体。***

  > 可以粗略地将这个和面向对象语言中的继承概念相比较，随后将会看到它被用来模拟类似继承的行为。Go语言中的继承是通过内嵌或组合来实现的，所以可以说，在 Go语言中，相比较于继承，组合更受青睐。

  ***Go语言中的继承是通过内嵌或组合来实现~~继承~~***

    ```
    package main
    import "fmt"
    type innerS struct {
        in1 int
        in2 int
    }
    type outerS struct {
        b int
        c float32
        int // anonymous field
        innerS //anonymous field
    }
    func main() {
        outer := new(outerS)
        outer.b = 6
        outer.c = 7.5
        outer.int = 60
        outer.in1 = 5
        outer.in2 = 10
        fmt.Printf("outer.b is: %d\n", outer.b)
        fmt.Printf("outer.c is: %f\n", outer.c)
        fmt.Printf("outer.int is: %d\n", outer.int)
        fmt.Printf("outer.in1 is: %d\n", outer.in1)
        fmt.Printf("outer.in2 is: %d\n", outer.in2)
        // 使用结构体字面量
        outer2 := outerS{6, 7.5, 60, innerS{5, 10}}
        fmt.Printf("outer2 is:", outer2)
    }
    ```

    *运行结果如下所示：*

    ```
    outer.b is: 6
    outer.c is: 7.500000
    outer.int is: 60
    outer.in1 is: 5
    outer.in2 is: 10
    outer2 is:{6 7.5 60 {5 10}}
    ```

  > 通过类型 outer.int 的名字来获取存储在匿名字段中的数据，于是可以得出一个结论：***在一个结构体中对于每一种数据类型只能有一个匿名字段。***  

  **内嵌结构体**

  > 同样地结构体也是一种数据类型，所以它也可以作为一个匿名字段来使用，如同上面例子中那样。外层结构体通过 outer.in1 直接进入内层结构体的字段，内嵌结构体甚至可以来自其他包。内层结构体被简单的插入或者内嵌进外层结构体。这个简单的“继承”机制提供了一种方式，使得可以从另外一个或一些类型继承部分或全部实现。

  ```
  package main
  import "fmt"
  type A struct {
      ax, ay int
  }
  type B struct {
      A
      bx, by float32
  }
  func main() {
      b := B{A{1, 2}, 3.0, 4.0}
      fmt.Println(b.ax, b.ay, b.bx, b.by)
      fmt.Println(b.A)
  }
  ```

  ```
  1 2 3 4
  {1 2}
  ```

  **结构内嵌特性**

  *Go语言的结构体内嵌有如下特性:*

  1. ***内嵌的结构体可以直接访问其成员变量***

    > 嵌入结构体的成员，可以通过外部结构体的实例直接访问。如果结构体有多层嵌入结构体，结构体实例访问任意一级的嵌入结构体成员时都只用给出字段名，而无须像传统结构体字段一样，通过一层层的结构体字段访问到最终的字段。例如，ins.a.b.c的访问可以简化为ins.c。

  2. ***内嵌结构体的字段名是它的类型名***

    > 内嵌结构体字段仍然可以使用详细的字段进行一层层访问，内嵌结构体的字段名就是它的类型名，代码如下：

    ```
    var c Color
    c.BasicColor.R = 1
    c.BasicColor.G = 1
    c.BasicColor.B = 0
    ```

  ***一个结构体只能嵌入一个同类型的成员，无须担心结构体重名和错误赋值的情况，编译器在发现可能的赋值歧义时会报错。***

+ go func

  *函数构成了代码执行的逻辑结构，在Go语言中，函数的基本组成为：关键字 func、函数名、参数列表、返回值、函数体和返回语句，每一个程序都包含很多的函数，函数是基本的代码块。*

  *因为Go语言是编译型语言，所以函数编写的顺序是无关紧要的*

  *Go语言里面拥三种类型的函数：*

  + ***普通的带有名字的函数***

  + ***匿名函数或者 lambda 函数***

  + ***方法***

  ***Go语言没有默认参数值，也没有任何方法可以通过参数名指定形参，因此形参和返回值的变量名对于函数调用者而言没有意义。***

  ***在函数中，实参通过值传递的方式进行传递，因此函数的形参是实参的拷贝，对形参进行修改不会影响实参，但是，如果实参包括引用类型，如指针、slice(切片)、map、function、channel 等类型，实参可能会由于函数的间接引用被修改。***

  *函数的返回值*

  + ***同一种类型返回值***

  > 如果返回值是同一种类型，则用括号将多个返回值类型括起来，用逗号分隔每个返回值的类型。  
  > 使用 return 语句返回时，值列表的顺序需要与函数声明的返回值类型一致，示例代码如下：

  ```
  func typedTwoValues() (int, int) {
    return 1, 2
  }
  func main() {
      a, b := typedTwoValues()
      fmt.Println(a, b)
  }
  ```

  + ***带有变量名的返回值***

  > Go语言支持对返回值进行命名，这样返回值就和参数一样拥有参数变量名和类型。  
  > 命名的返回值变量的默认值为类型的默认值，即数值为 0，字符串为空字符串，布尔为 false、指针为 nil 等。

  > 下面代码中的函数拥有两个整型返回值，函数声明时将返回值命名为 a 和 b，因此可以在函数体中直接对函数返回值进行赋值，在命名的返回值方式的函数体中，在函数结束前需要显式地使用 return 语句进行返回，代码如下：

  ```
  func namedRetValues() (a, b int) {

    a = 1
    b = 2

    return
  }
  ```

  ***Tips***

  > 同一种类型返回值和命名返回值两种形式只能二选一，混用时将会发生编译错误，例如下面的代码：

  ```
  func namedRetValues() (a, b int, int)
  ```

  > Go语言中传入与返回参数在调用和返回时都使用值传递，这里需要注意的是指针、切片和 map 等引用型对象在参数传递中不会发生复制，而是将指针进行复制，类似于创建一次引用。

  > 在Go语言中，函数也是一种类型，可以和其他类型一样保存在变量中。

  ```
  package main

  import (
      "fmt"
  )

  func fire() {
      fmt.Println("fire")
  }

  func main() {

      var f func()

      f = fire

      f()
  }
  ```

  > 使用 SQL 语言从数据库中获取数据时，可以对原始数据进行排序（sort by）、分组（group by）和去重（distinct）等操作，SQL 将数据的操作与遍历过程作为两个部分进行隔离，这样操作和遍历过程就可以各自独立地进行设计，这就是常见的数据与操作分离的设计。对数据的操作进行多步骤的处理被称为链式处理。

  ```
  package main

  import (
      "fmt"
      "strings"
  )

  // 字符串处理函数，传入字符串切片和处理链
  func StringProccess(list []string, chain []func(string) string) {

      // 遍历每一个字符串
      for index, str := range list {

          // 第一个需要处理的字符串
          result := str

          // 遍历每一个处理链
          for _, proc := range chain {

              // 输入一个字符串进行处理，返回数据作为下一个处理链的输入。
              result = proc(result)
          }

          // 将结果放回切片
          list[index] = result
      }
  }

  // 自定义的移除前缀的处理函数
  func removePrefix(str string) string {

      return strings.TrimPrefix(str, "go")
  }

  func main() {

      // 待处理的字符串列表
      list := []string{
          "go scanner",
          "go parser",
          "go compiler",
          "go printer",
          "go formater",
      }

      // 处理函数链
      chain := []func(string) string{
          removePrefix,
          strings.TrimSpace,
          strings.ToUpper,
      }

      // 处理字符串
      StringProccess(list, chain)

      // 输出处理好的字符串
      for _, str := range list {
          fmt.Println(str)
      }

  }
  ```

  > Go语言支持匿名函数，即在需要使用函数时再定义函数，匿名函数没有函数名只有函数体，函数可以作为一种类型被赋值给函数类型的变量，匿名函数也往往以变量方式传递，这与C语言的回调函数比较类似，不同的是，Go语言支持随时在代码里定义匿名函数。  
  > 匿名函数是指不需要定义函数名的一种函数实现方式，由一个不带函数名的函数声明和函数体组成。

  > 使用匿名函数实现操作封装

  ```
  package main

  import (
      "flag"
      "fmt"
  )

  var skillParam = flag.String("skill", "", "skill to perform")

  func main() {

      flag.Parse()

      var skill = map[string]func(){
          "fire": func() {
              fmt.Println("chicken fire")
          },
          "run": func() {
              fmt.Println("soldier run")
          },
          "fly": func() {
              fmt.Println("angel fly")
          },
      }

      if f, ok := skill[*skillParam]; ok {
          f()
      } else {
          fmt.Println("skill not found")
      }

  }
  ```

  > 函数和其他类型一样都属于“一等公民”，其他类型能够实现接口，函数也可以。

  ```
  package main

  import (
      "fmt"
  )

  // 调用器接口
  type Invoker interface {
      // 需要实现一个Call方法
      Call(interface{})
  }

  // 结构体类型
  type Struct struct {
  }

  // 实现Invoker的Call
  func (s *Struct) Call(p interface{}) {
      fmt.Println("from struct", p)
  }

  // 函数定义为类型
  type FuncCaller func(interface{})

  // 实现Invoker的Call
  func (f FuncCaller) Call(p interface{}) {

      // 调用f函数本体
      f(p)
  }

  func main() {

      // 声明接口变量
      var invoker Invoker

      // 实例化结构体
      s := new(Struct)

      // 将实例化的结构体赋值到接口
      invoker = s

      // 使用接口调用实例化结构体的方法Struct.Call
      invoker.Call("hello")

      // 将匿名函数转为FuncCaller类型，再赋值给接口
      invoker = FuncCaller(func(v interface{}) {
          fmt.Println("from function", v)
      })

      // 使用接口调用FuncCaller.Call，内部会调用函数本体
      invoker.Call("hello")
  }
  ```

  *函数体实现接口*

  ***函数的声明不能直接实现接口，需要将函数定义为类型后，使用类型实现结构体，当类型方法被调用时，还需要调用函数本体。***

  *闭包(Closure)*

  > Go语言中闭包是引用了自由变量的函数，被引用的自由变量和函数一同存在，即使已经离开了自由变量的环境也不会被释放或者删除，在闭包中可以继续使用这个自由变量，因此，简单的说：

  ***函数 + 引用环境 = 闭包***

  > 一个函数类型就像结构体一样，可以被实例化，函数本身不存储任何信息，只有与引用环境结合后形成的闭包才具有“记忆性”，函数是编译期静态的概念，而闭包是运行期动态的概念。

  > 闭包（Closure）在某些编程语言中也被称为 Lambda 表达式。

  > 闭包对环境中变量的引用过程也可以被称为“捕获”，在 C++11 标准中，捕获有两种类型，分别是引用和复制，可以改变引用的原值叫做“引用捕获”，捕获的过程值被复制到闭包中使用叫做“复制捕获”。

  > 被捕获到闭包中的变量让闭包本身拥有了记忆效应，闭包中的逻辑可以修改闭包捕获的变量，变量会跟随闭包生命期一直存在，闭包本身就如同变量一样拥有了记忆效应。

  *可变参数*

  ```
  func myfunc(args ...int) {
      for _, arg := range args {
          fmt.Println(arg)
      }
  }
  ```

  > 形如...type格式的类型只能作为函数的参数类型存在，并且必须是最后一个参数，它是一个语法糖（syntactic sugar），即这种语法对语言的功能并没有影响，但是更方便程序员使用，通常来说，使用语法糖能够增加程序的可读性，从而减少程序出错的可能。

  > 从内部实现机理上来说，类型...type本质上是一个数组切片，也就是[]type，这也是为什么上面的参数 args 可以用 for 循环来获得每个传入的参数。

  *假如没有...type这样的语法糖，开发者将不得不这么写：*

  ```
  func myfunc2(args []int) {
      for _, arg := range args {
          fmt.Println(arg)
      }
  }
  ```

  *从函数的实现角度来看，这没有任何影响，该怎么写就怎么写，但从调用方来说，情形则完全不同：*

  ```
  myfunc2([]int{1, 3, 7, 13})
  ```

  *不得不加上[]int{}来构造一个数组切片实例，但是有了...type这个语法糖，就不用自己来处理了。*

  > 任意类型的可变参数，可以指定类型为 interface{}，下面是Go语言标准库中 fmt.Printf() 的函数原型：

  ```
  func Printf(format string, args ...interface{}) {
        // ...
  }
  ```

  *在多个可变参数函数中传递参数*

  > 可变参数变量是一个包含所有参数的切片，如果要将这个含有可变参数的变量传递给下一个可变参数函数，可以在传递时给可变参数变量后面添加...，这样就可以将切片中的元素进行传递，而不是传递可变参数变量本身。

  ```
  package main
  import "fmt"
  // 实际打印的函数
  func rawPrint(rawList ...interface{}) {
      // 遍历可变参数切片
      for _, a := range rawList {
          // 打印参数
          fmt.Println(a)
      }
  }
  // 打印函数封装
  func print(slist ...interface{}) {
      // 将slist可变参数切片完整传递给下一个函数
      rawPrint(slist...)
  }
  func main() {
      print(1, 2, 3)
  }
  ```

  ***可变参数使用...进行传递与切片间使用 append 连接是同一个特性。***

  *defer*

  ***Go语言的 defer 语句会将其后面跟随的语句进行延迟处理，在 defer 归属的函数即将返回时，将延迟处理的语句按 defer 的逆序进行执行，也就是说，先被 defer 的语句最后被执行，最后被 defer 的语句，最先被执行。***

  > 关键字 defer 的用法类似于面向对象编程语言 Java 和 C# 的 finally 语句块，它一般用于释放某些已分配的资源，典型的例子就是对一个互斥解锁，或者关闭一个文件。

  **使用延迟执行语句在函数退出时释放资源**
  > 处理业务或逻辑中涉及成对的操作是一件比较烦琐的事情，比如打开和关闭文件、接收请求和回复请求、加锁和解锁等。在这些操作中，最容易忽略的就是在每个函数退出处正确地释放和关闭资源。  
  > defer 语句正好是在函数退出时执行的语句，所以使用 defer 能非常方便地处理资源释放问题。

  + ***使用延迟并发解锁***

  + ***使用延迟释放文件句柄***

+ go error

  *Go语言的错误处理思想及设计包含以下特征：*

  + **一个可能造成错误的函数，需要返回值中返回一个错误接口（error），如果调用是成功的，错误接口将返回 nil，否则返回错误。**
  
  + **在函数调用后需要检查错误，如果发生错误，则进行必要的错误处理。**

  > Go语言没有类似 Java 或 .NET 中的异常处理机制，虽然可以使用 defer、panic、recover 模拟，但官方并不主张这样做，Go语言的设计者认为其他语言的异常机制已被过度使用，上层逻辑需要为函数发生的异常付出太多的资源，同时，如果函数使用者觉得错误处理很麻烦而忽略错误，那么程序将在不可预知的时刻崩溃。  
  > Go语言希望开发者将错误处理视为正常开发必须实现的环节，正确地处理每一个可能发生错误的函数，同时，Go语言使用返回值返回错误的机制，也能大幅降低编译器、运行时处理错误的复杂度，让开发者真正地掌握错误的处理。

  *根据Go语言的错误处理机制，Conn 是其重要的返回值，因此，为这个函数增加一个错误返回，类似为 error，参见下面的代码：*

  ```
  func Dial(network, address string) (Conn, error) {
      var d Dialer
      return d.Dial(network, address)
  }
  ```

  *在 io 包中的 Writer 接口也拥有错误返回，代码如下：*

  ```
  type Writer interface {
      Write(p []byte) (n int, err error)
  }
  ```

  *io 包中还有 Closer 接口，只有一个错误返回，代码如下：*
  
  ```
  type Closer interface {
      Close() error
  }
  ```

  ***错误接口的定义格式***

  *error 是 Go 系统声明的接口类型，代码如下：*

  ```
  type error interface {
      Error() string
  }
  ```

  > 所有符合 Error()string 格式的方法，都能实现错误接口，Error() 方法返回错误的具体描述，使用者可以通过这个字符串知道发生了什么错误。

  ***自定义一个错误***

  ```
  var err = errors.New("this is an error")
  ```

  > 错误字符串由于相对固定，一般在包作用域声明，应尽量减少在使用时直接使用 errors.New 返回。

  + *errors 包*

  **Go语言的 errors 中对 New 的定义非常简单，代码如下：**

  ```
  // 创建错误对象
  func New(text string) error {
      return &errorString{text}
  }
  // 错误字符串
  type errorString struct {
      s string
  }
  // 返回发生何种错误
  func (e *errorString) Error() string {
      return e.s
  }
  ```

  + *在代码中使用错误定义*

  ```
  package main
  import (
      "errors"
      "fmt"
  )
  // 定义除数为0的错误
  var errDivisionByZero = errors.New("division by zero")
  func div(dividend, divisor int) (int, error) {
      // 判断除数为0的情况并返回
      if divisor == 0 {
          return 0, errDivisionByZero
      }
      // 正常计算，返回空错误
      return dividend / divisor, nil
  }
  func main() {
      fmt.Println(div(1, 0))
  }
  ```

  *在解析中使用自定义错误*

  > 使用 errors.New 定义的错误字符串的错误类型是无法提供丰富的错误信息的，那么，如果需要携带错误信息返回，就需要借助自定义结构体实现错误接口。  
  > 下面代码将实现一个解析错误（ParseError），这种错误包含两个内容，分别是文件名和行号，解析错误的结构还实现了 error 接口的 Error() 方法，返回错误描述时，就需要将文件名和行号返回。

  ```
  package main
  import (
      "fmt"
  )
  // 声明一个解析错误
  type ParseError struct {
      Filename string // 文件名
      Line     int    // 行号
  }
  // 实现error接口，返回错误描述
  func (e *ParseError) Error() string {
      return fmt.Sprintf("%s:%d", e.Filename, e.Line)
  }
  // 创建一些解析错误
  func newParseError(filename string, line int) error {
      return &ParseError{filename, line}
  }
  func main() {
      var e error
      // 创建一个错误实例，包含文件名和行号
      e = newParseError("main.go", 1)
      // 通过error接口查看错误描述
      fmt.Println(e.Error())
      // 根据错误接口具体的类型，获取详细错误信息
      switch detail := e.(type) {
      case *ParseError: // 这是一个解析错误
          fmt.Printf("Filename: %s Line: %d\n", detail.Filename, detail.Line)
      default: // 其他类型的错误
          fmt.Println("other error")
      }
  }
  ```

+ go panic

  > Go语言的类型系统会在编译时捕获很多错误，但有些错误只能在运行时检查，如数组访问越界、空指针引用等，这些运行时错误会引起宕机。

  > 一般而言，当宕机发生时，程序会中断运行，并立即执行在该 goroutine（可以先理解成线程）中被延迟的函数（defer 机制），随后，程序崩溃并输出日志信息，日志信息包括 panic value 和函数调用的堆栈跟踪信息，panic value 通常是某种错误信息。  
  > 对于每个 goroutine，日志信息中都会有与之相对的，发生 panic 时的函数调用堆栈跟踪信息，通常，我们不需要再次运行程序去定位问题，日志信息已经提供了足够的诊断依据，因此，在我们填写问题报告时，一般会将宕机和日志信息一并记录。  
  > 虽然Go语言的 panic 机制类似于其他语言的异常，但 panic 的适用场景有一些不同，由于 panic 会引起程序的崩溃，因此 panic 一般用于严重错误，如程序内部的逻辑不一致。任何崩溃都表明了我们的代码中可能存在漏洞，所以对于大部分漏洞，我们应该使用Go语言提供的错误机制，而不是 panic。

  + *手动触发宕机*

  > Go语言可以在程序中手动触发宕机，让程序崩溃，这样开发者可以及时地发现错误，同时减少可能的损失。  
  > Go语言程序在宕机时，会将堆栈和 goroutine 信息输出到控制台，所以宕机也可以方便地知晓发生错误的位置。

  ```
  package main
  func main() {
      panic("crash")
  }
  ```

  **以上代码中只用了一个内建的函数 panic() 就可以造成崩溃，panic() 的声明如下：**

  ```
  func panic(v interface{})    //panic() 的参数可以是任意类型的。
  ```

  + *在运行依赖的必备资源缺失时主动触发宕机*

  > regexp 是Go语言的正则表达式包，正则表达式需要编译后才能使用，而且编译必须是成功的，表示正则表达式可用。

  **编译正则表达式函数有两种，具体如下：**

  > 1) func Compile(expr string) (*Regexp, error)  
  > 编译正则表达式，发生错误时返回编译错误同时返回 Regexp 为 nil，该函数适用于在编译错误时获得编译错误进行处理，同时继续后续执行的环境。  
  > 2) func MustCompile(str string) *Regexp  
  > 当编译正则表达式发生错误时，使用 panic 触发宕机，该函数适用于直接使用正则表达式而无须处理正则表达式错误的情况。

  ***手动宕机进行报错的方式不是一种偷懒的方式，反而能迅速报错，终止程序继续运行，防止更大的错误产生，不过，如果任何错误都使用宕机处理，也不是一种良好的设计习惯，因此应根据需要来决定是否使用宕机进行报错。***

  + *在宕机时触发延迟执行语句*

  > 当 panic() 触发的宕机发生时，panic() 后面的代码将不会被运行，但是在 panic() 函数前面已经运行过的 defer 语句依然会在宕机发生时发生作用，参考下面代码：

  ```
  package main

  import "fmt"

  func main() {
      defer fmt.Println("宕机后要做的事情1")
      defer fmt.Println("宕机后要做的事情2")
      panic("宕机")
  }
  ```

  ```
  宕机后要做的事情2
  宕机后要做的事情1
  panic: 宕机

  goroutine 1 [running]:
  main.main()
      D:/code/main.go:8 +0xf8
  exit status 2
  ```

  > 宕机前，defer 语句会被优先执行，由于第 7 行的 defer 后执行，因此会在宕机前，这个 defer 会优先处理，随后才是第 6 行的 defer 对应的语句，这个特性可以用来在宕机发生前进行宕机信息处理。

+ go Recover

  > Recover 是一个Go语言的内建函数，可以让进入宕机流程中的 goroutine 恢复过来，recover 仅在延迟函数 defer 中有效，在正常的执行过程中，调用 recover 会返回 nil 并且没有其他任何效果，如果当前的 goroutine 陷入恐慌，调用 recover 可以捕获到 panic 的输入值，并且恢复正常的执行。

  