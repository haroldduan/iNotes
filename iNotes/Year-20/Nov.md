<!--
@author: harold.duan
@date: 20-11-01
@memo: Notes logging
-->

### Redash

*Optional Item*

``` clone Redash repo
$ git clone https://github.com/getredash/redash
```

``` edit docker compose version 9.0 exists error
# This is an example configuration for Docker Compose. Make sure to atleast update
# the cookie secret & postgres database password.
#
# Some other recommendations:
# 1. To persist Postgres data, assign it a volume host location.
# 2. Split the worker service to adhoc workers and scheduled queries workers.
version: '2'
services:
  server:
    image: redash/redash:9.0.0-beta.b42121
    command: server
    depends_on:
      - postgres
      - redis
    ports:
      - "5050:5000"
    environment:
      PYTHONUNBUFFERED: 0
      REDASH_LOG_LEVEL: "INFO"
      REDASH_REDIS_URL: "redis://redis:6379/0"
      REDASH_DATABASE_URL: "postgresql://postgres@postgres/postgres"
      REDASH_COOKIE_SECRET: "Q422k6vaXUk8"
      REDASH_WEB_WORKERS: 4
      # email settings
      REDASH_MAIL_SERVER: "smtp.qiye.aliyun.com"
      REDASH_MAIL_PORT: 465
      REDASH_MAIL_USE_TLS: "false"
      REDASH_MAIL_USE_SSL: "true"
      REDASH_MAIL_USERNAME: "xxx@avatech.com.cn"
      REDASH_MAIL_PASSWORD: "xxxxxxxx"
      REDASH_MAIL_DEFAULT_SENDER: "xxx@avatech.com.cn"
      REDASH_HOST: "http://192.168.3.14:8080"
    restart: always
  worker:
    image: redash/redash:9.0.0-beta.b42121
    command: scheduler
    environment:
      PYTHONUNBUFFERED: 0
      REDASH_LOG_LEVEL: "INFO"
      REDASH_REDIS_URL: "redis://redis:6379/0"
      REDASH_DATABASE_URL: "postgresql://postgres@postgres/postgres"
      QUEUES: "queries,scheduled_queries,celery"
      WORKERS_COUNT: 2
    restart: always
  redis:
    image: redis:3.0-alpine
    ports:
    - "6379:6379"
    volumes: 
      - /home/admin/dockers/redash/data/redis_data:/data
    restart: always
  postgres:
    image: postgres:9.5.6-alpine
    ports:
    - "5432:5432"
    volumes:
      - /home/admin/dockers/redash/data/postgresql_data:/var/lib/postgresql/data
    restart: always
  nginx:
    image: redash/nginx:latest
    ports:
      - "8080:80"
    depends_on:
      - server
    links:
      - server:redash
    restart: always
```

``` edit docker compose version 3.0
version: '2'
services:
  server:
    image: redash/redash:3.0.0.b2998
    command: server
    depends_on:
      - postgres
      - redis
    ports:
      - "5000:5000"
    environment:
      PYTHONUNBUFFERED: 0
      REDASH_LOG_LEVEL: "INFO"
      REDASH_REDIS_URL: "redis://redis:6379/0"
      REDASH_DATABASE_URL: "postgresql://postgres@postgres/postgres"
      REDASH_COOKIE_SECRET: "123456"
      REDASH_WEB_WORKERS: 4
  worker:
    image: redash/redash:3.0.0.b2998
    command: scheduler
    environment:
      PYTHONUNBUFFERED: 0
      REDASH_LOG_LEVEL: "INFO"
      REDASH_REDIS_URL: "redis://redis:6379/0"
      REDASH_DATABASE_URL: "postgresql://postgres@postgres/postgres"
      QUEUES: "queries,scheduled_queries,celery"
      WORKERS_COUNT: 2
  redis:
    image: redis:3.0-alpine
  mongodb:
    image: mongo:3.2
    ports:
    - "27017:27017"
  click:
     image: yandex/clickhouse-server
     ports:
     - "8123:8123"
     - "8000:9000"
     - "8009:9009"
     ulimits:
      nproc: 65535
      nofile:
       soft: 262144
       hard: 262144
  postgres:
    image: postgres:9.5.6-alpine
    ports:
    - "5432:5432"
    volumes:
    - /home/admin/dockers/redash/data/postgresql_data:/var/lib/postgresql/data
  nginx:
    image: redash/nginx:latest
    ports:
      - "8080:80"
    depends_on:
      - server
    links:
      - server:redash
```

``` docker run
$ docker-compose run --rm server create_db
$ docker-compose up -d
```

### Flutter

**on Windows**

1. Installing Dart

[Dart for Windows](https://gekorm.com/dart-windows/)

2. Install Flutter SDK

+ Flutter SDK install

[Flutter SDK releases](https://flutter.dev/docs/development/tools/sdk/releases#windows)

> 去flutter官网下载其最新可用的安装包，点击下载 ；  
> 注意，Flutter的渠道版本会不停变动，请以Flutter官网为准。另外，在中国大陆地区，要想正常获取安装包列表或下载安装包，可能需要翻墙，也可以去Flutter github项目下去下载安装包 。  
> 将安装包zip解压到你想安装Flutter SDK的路径（如：C:\Android\flutter；注意，不要将flutter安装到需要一些高权限的路径如C:\Program Files\）。  
> 在Flutter安装目录的flutter文件下找到flutter_console.bat，双击运行并启动flutter命令行，接下来，你就可以在Flutter命令行运行flutter命令了。  

*Tips*

注意： 由于一些flutter命令需要联网获取数据，如果您是在国内访问，由于众所周知的原因，直接访问很可能不会成功。 上面的PUB_HOSTED_URL和FLUTTER_STORAGE_BASE_URL是google为国内开发者搭建的临时镜像。详情请参考 [Using Flutter in China](https://github.com/flutter/flutter/wiki/Using-Flutter-in-China)

+ ENV Update

> 要在终端运行 flutter 命令， 你需要添加以下环境变量到系统PATH：  
> 转到 “控制面板>用户帐户>用户帐户>更改我的环境变量”  
> 在“用户变量(系统变量)”下检查是否有名为“Path”的条目:  
> 如果该条目存在, 追加 flutter\bin的全路径，使用 ; 作为分隔符.  
> 如果条目不存在, 创建一个新用户变量 Path ，然后将 flutter\bin的全路径作为它的值.  
> 在“用户变量”下检查是否有名为”PUB_HOSTED_URL”和”FLUTTER_STORAGE_BASE_URL”的条目，如果没有，也添加它们。  

*Tips*

由于在国内访问Flutter有时可能会受到限制，Flutter官方为中国开发者搭建了临时镜像，大家可以将如下环境变量加入到用户环境变量中：

```
PUB_HOSTED_URL=https://pub.flutter-io.cn
FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

*运行 flutter doctor*

```
$ flutter doctor
```

3. Android设置

 + 安装Android Studio

> 要为Android开发Flutter应用，您可以使用Mac，Windows或Linux（64位）机器.  
> Flutter需要安装和配置Android Studio:  
> 下载并安装 [Android Studio](https://developer.android.com/studio/index.html).  
> 启动Android Studio，然后执行“Android Studio安装向导”。这将安装最新的Android SDK，Android SDK平台工具和Android SDK构建工具，这是Flutter为Android开发时所必需的  
> 设置Android设备  
> 要准备在Android设备上运行并测试您的Flutter应用，您需要安装Android 4.1（API level 16）或更高版本的Android设备.  

*Tips*

***如果运行flutter doctor 命令出现 Android toolchain 缺失***

在环境变量中设置Android SDK路径，如下：

```
ANDROID_HOME=C:\Android\Sdk
```

4. Installing VSCode Plugins

[VSCode Dart](https://github.com/Dart-Code/Dart-Code.git)

[VSCode Flutter](https://github.com/Dart-Code/Flutter.git)

*Usefulling URLs*

https://github.com/dart-lang

https://github.com/flutter

https://www.dartcn.com/

https://www.dartcn.com/guides/libraries/

https://www.dartcn.com/guides/language/

https://www.dartcn.com/tools/

https://dart.cn/

https://flutter.cn/docs/get-started/install/windows

https://flutterchina.club/get-started/test-drive/#vscode

https://www.yiibai.com/dart/

https://zhuanlan.zhihu.com/p/53672286

https://blog.csdn.net/kabuto_hui/article/details/79505262

https://pub.flutter-io.cn/

https://flutterchina.club/tutorials/

https://www.w3cschool.cn/nxvsy/

### Flutter building

**on Android**

+ Generate App Keystore

```
$ keytool -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key
```

+ New keystore-properties *key.properties* file

*Path:/android/key.properties*

```key.properties
storePassword=avatech
keyPassword=avatech
keyAlias=key
storeFile=C:\\WorkSpaces\\Harold.Duan\\SourceCode\\dart\\app_test\\android\\app\\key\\my-release-key.jks
```

+ Update grdle *build.gradle* file

*Path:/android/app/build.gradle*

```build.gradle add contents
...

def keystorePropertiesFile = rootProject.file("key.properties")
def keystoreProperties = new Properties()
keystoreProperties.load(new FileInputStream(keystorePropertiesFile))

...
android {
  ...

  signingConfigs {
        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile file(keystoreProperties['storeFile'])
            storePassword keystoreProperties['storePassword']
        }
    }

    buildTypes {
        release {
            // TODO: Add your own signing config for the release build.
            // Signing with the debug keys for now, so `flutter run --release` works.
            signingConfig signingConfigs.release
        }
    }

  ...
}
```

+ Update App Name & Icons

*Path:/android/app/src/main/AndroidManifest.xml*  

``` AndroidManifest.xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.app_warehouse_task">
    <!-- io.flutter.app.FlutterApplication is an android.app.Application that
         calls FlutterMain.startInitialization(this); in its onCreate method.
         In most cases you can leave this as-is, but you if you want to provide
         additional functionality it is fine to subclass or reimplement
         FlutterApplication and put your custom class here. -->
    <application
        android:name="io.flutter.app.FlutterApplication"
        android:label="Dahupt" #app name
        android:icon="@mipmap/ic_launcher"> #app icon
        ...
    ...
...
```

*Path:/android/app/src/main/res/*

***Folders: mipmap-xxx***

+ Run building

```
$ flutter build apk --release
# or
$ flutter build apk
```

*Outputs path:build/app/outputs/apk/release/*

*Usefulling URLs*

https://www.cnblogs.com/gdsblog/p/10100972.html

https://www.jianshu.com/p/fabcfd621e01

https://blog.csdn.net/ruoshui_t/article/details/100742079

https://blog.csdn.net/qq_15003505/article/details/62041540

***https://flutter.dev/***

***https://api.flutter.dev/index.html***

***https://book.flutterchina.club/***

***https://flutter.dev/docs/development/tools/devtools/overview***

***[Flutter完整开发实战详解](https://mp.weixin.qq.com/mp/homepage?__biz=Mzg3NTA3MDIxOA==&hid=2&sn=679ad0212470f5155c4412e678411374&scene=1&devicetype=iOS14.1&version=17001228&lang=en&nettype=3G+&ascene=7&session_us=gh_178db80346fd&fontScale=100)***

*pub package path:C:\Users\Administrator\AppData\Local\Pub\\*


*Tips*

+ ***在 Flutter 中一切的显示都是 Widget ，Widget 是一切的基础，利用响应式模式进行渲染。***

+ ***Widget 分为 有状态 和 无状态 两种，在 Flutter 中每个页面都是一帧，无状态就是保持在那一帧，而有状态的 Widget 当数据更新时，其实是创建了新的 Widget，只是 State 实现了跨帧的数据同步保存。***

+ ***无状态StatelessWidget，继承 StatelessWidget，通过 build 方法返回一个布局好的控件。***

+ ***Widget 和 Widget 之间通过 child: 进行嵌套。其中有的 Widget 只能有一个 child，比如下方的 Container ；有的 Widget 可以多个 child ，也就是children，比如 Column 布局。***

+ ***有状态StatefulWidget，需要创建管理的是主要是 State， 通过 State 的 build 方法去构建控件。在 State 中，可以动态改变数据，在 setState之后，改变的数据会触发 Widget 重新构建刷新。***

+ ***State 中主要的声明周期有 ：***  
  - *initState ：初始化，理论上只有初始化一次，特殊情况下可以多次。*  
  - *didChangeDependencies：在 initState 之后调用，此时可以获取其他 State 。*  
  - *dispose ：销毁，只会调用一次。*  

+ ***关注点只要聚焦在创建的 StatelessWidget 或者 StatefulWidget 而已。需要的就是在 build 中堆积页面的布局，然后把数据添加到 Widget 中，最后通过 setState 改变数据，从而实现画面变化。***

+ ***Flutter 布局***

*Flutter 中拥有需要将近30种内置的 布局Widget，其中常用有 Container、Padding、Center、Flex、Stack、Row、Column、ListView 等，下面简单讲解它们的特性和使用。*

类型|作用特点
:---|:---
Container|只有一个子 Widget。默认充满，包含了padding、margin、color、宽高、decoration 等配置。
Padding|只有一个子 Widget。只用于设置Padding，常用于嵌套child，给child设置padding。
Center|只有一个子 Widget。只用于居中显示，常用于嵌套child，给child设置居中。
Stack|可以有多个子 Widget。子Widget堆叠在一起。
Column|可以有多个子 Widget。垂直布局。
Row|可以有多个子 Widget。水平布局。
Expanded|只有一个子 Widget。在 Column 和 Row 中充满。
ListView|可以有多个子 Widget。自己意会吧。

  - *Container ：最常用的默认控件，但是实际上它是由多个内置控件组成的模版，只能包含一个child，支持 padding,margin,color,宽高,decoration（一般配置边框和阴影）等配置，在 Flutter 中，不是所有的控件都有 宽高、padding、margin、color 等属性，所以才会有 Padding、Center 等 Widget 的存在。*

  - *Column、Row 绝对是必备布局， 横竖布局也是日常中最常见的场景。它们常用的有这些属性配置：主轴方向是 start 或 center 等；副轴方向方向是 start 或 center 等；mainAxisSize 是充满最大尺寸，或者只根据子 Widget 显示最小尺寸。*

  - *Expanded 在 Column 和 Row 中代表着平均充满的作用，当有两个存在的时候默认均分充满。同时也可以设置 flex 属性决定比例。*

+ ***Flutter 页面***

*Flutter 中除了布局的 Widget，还有交互显示的 Widget 和完整页面呈现的Widget，其中常见的有 MaterialApp、Scaffold、Appbar、Text、Image、FlatButton等，下面简单介绍这些 Wdiget，并完成一个页面。*

类型|作用特点
:---|:---
MaterialApp|一般作为APP顶层的主页入口，可配置主题，多语言，路由等
Scaffold|一般用户页面的承载Widget，包含appbar、snackbar、drawer等material design的设定。
Appbar|一般用于Scaffold的appbar ，内有标题，二级页面返回按键等，当然不止这些，tabbar等也会需要它 。
Text|显示文本，几乎都会用到，主要是通过style设置TextStyle来设置字体样式等。
RichText|富文本，通过设置TextSpan，可以拼接出富文本场景。
TextField|文本输入框 ：new TextField(controller: //文本控制器, obscureText: "hint文本");
Image|图片加载: new FadeInImage.assetNetwork( placeholder: "预览图", fit: BoxFit.fitWidth, image: "url");
FlatButton|按键点击: new FlatButton(onPressed: () {},child: new Container());

+ ***AppBar***

*在 Flutter 中 AppBar 算是常用 Widget ，而 AppBar 可不仅仅作为标题栏和使用，AppBar上的 leading 和 bottom 同样是有用的功能。*

  - *AppBar 的 bottom 默认支持 TabBar, 也就是常见的顶部 Tab 的效果，这其实是因为TabBar 实现了 PreferredSizeWidget 的 preferredSize。所以只要你的控件实现了 preferredSize，就可以放到 AppBar 的  bottom 中使用。*

  - *leading ：通常是左侧按键，不设置时一般是 Drawer 的图标或者返回按钮。*

  - *flexibleSpace ：位于 bottom 和 leading 之间。*

+ ***按键***

*Flutter 中的按键，如 FlatButton 默认是否有边距和最小大小的。所以如果你想要无 padding、margin、border 、默认大小 等的按键效果，其中一种方式如下：*

```
///
new RawMaterialButton(
        materialTapTargetSize: MaterialTapTargetSize.shrinkWrap,
        padding: padding ?? const EdgeInsets.all(0.0),
        constraints: const BoxConstraints(minWidth: 0.0, minHeight: 0.0),
        child: child,
        onPressed: onPressed);
```

*如果在再上 Flex ，如下所示，一个可控的填充按键就出来了。*

```
new RawMaterialButton(
        materialTapTargetSize: MaterialTapTargetSize.shrinkWrap,
        padding: padding ?? const EdgeInsets.all(0.0),
        constraints: const BoxConstraints(minWidth: 0.0, minHeight: 0.0),
        ///flex
        child: new Flex(
          mainAxisAlignment: mainAxisAlignment,
          direction: Axis.horizontal,
          children: <Widget>[],
        ),
        onPressed: onPressed);
```

+ ***StatefulWidget 赋值***

*这里我们以给 TextField 主动赋值为例，其实 Flutter 中，给有状态的 Widget 传递状态或者数据，一般都是通过各种 controller 。如 TextField 的主动赋值，如下代码所示：*

```
final TextEditingController controller = new TextEditingController();

 @override
 void didChangeDependencies() {
    super.didChangeDependencies();
    ///通过给 controller 的 value 新创建一个 TextEditingValue
    controller.value = new TextEditingValue(text: "给输入框填入参数");
 }

 @override
  Widget build(BuildContext context) {
    return new TextField(
     ///controller
      controller: controller,
      onChanged: onChanged,
      obscureText: obscureText,
      decoration: new InputDecoration(
        hintText: hintText,
        icon: iconData == null ? null : new Icon(iconData),
      ),
    );
  }
```

*其实 TextEditingValue 是 ValueNotifier，其中 value 的 setter 方法被重载，一旦改变就会触发 notifyListeners 方法。而 TextEditingController 中，通过调用 addListener 就监听了数据的改变，从而让UI更新。*  
*当然，赋值有更简单粗暴的做法是：**传递一个对象 class A 对象，在控件内部使用对象 A.b 的变量绑定控件，外部通过 setState({ A.b = b2}) 更新。***

+ ***GlobalKey***

*在Flutter中，要主动改变子控件的状态，还可以使用 GlobalKey。比如你需要主动调用 RefreshIndicator 显示刷新状态，如下代码所示。*

```
GlobalKey<RefreshIndicatorState> refreshIndicatorKey;
  
 showForRefresh() {
    ///显示刷新
    refreshIndicatorKey.currentState.show();
  }

  @override
  Widget build(BuildContext context) {
    refreshIndicatorKey =  new GlobalKey<RefreshIndicatorState>();
    return new RefreshIndicator(
      key: refreshIndicatorKey,
      onRefresh: onRefresh,
      child: new ListView.builder(
        ///·····
      ),
    );
  }
```

+ ***Redux 与主题***

*使用 Redux 来做 Flutter 的全局 State 管理最合适不过，由于Redux内容较多，如果感兴趣的可以看看 篇章二 ，这里主要通过 Redux 来实现实时切换主题的效果。*  
*如下代码，通过 StoreProvider 加载了 store ，再通过 StoreBuilder 将 store 中的 themeData 绑定到 MaterialApp 的 theme 下，之后在其他 Widget 中通过 Theme.of(context) 调你需要的颜色，最终在任意位置调用 store.dispatch 就可实时修改主题，效果如后图所示。*

```
class FlutterReduxApp extends StatelessWidget {
  final store = new Store<GSYState>(
    appReducer,
    initialState: new GSYState(
      themeData: new ThemeData(
        primarySwatch: GSYColors.primarySwatch,
      ),
    ),
  );

  FlutterReduxApp({Key key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    /// 通过 StoreProvider 应用 store
    return new StoreProvider(
      store: store,
      ///通过 StoreBuilder 获取 themeData
      child: new StoreBuilder<GSYState>(builder: (context, store) {
        return new MaterialApp(
            theme: store.state.themeData,
            routes: {
              HomePage.sName: (context) {
                return HomePage();
              },
            });
      }),
    );
  }
}
```

+ ***Hotload 与 Package***

*Flutter 在 Debug 和 Release 下分别是 JIT 和 AOT 模式，而在 DEBUG 下，是支持 Hotload 的，而且十分丝滑。但是需要注意的是：**如果开发过程中安装了新的第三方包 ，而新的第三方包如果包含了原生代码，需要停止后重新运行哦。***  
*pubspec.yaml 文件下就是我们的包依赖目录，其中 ^ 代表大于等于，一般情况下 upgrade和 get 都能达到下载包的作用。但是：**upgrade 会在包有更新的情况下，更新 pubspec.lock 文件下包的版本 。***

+ ***Error Warnings***

```
Error: ADB exited with exit code 1
Performing Streamed Install

adb: failed to install C:\WorkSpaces\Harold.Duan\SourceCode\dart\app_warehouse_task\build\app\outputs\flutter-apk\app.apk: Failure [INSTALL_FAILED_INSUFFICIENT_STORAGE]
Error launching application on Android SDK built for x86 64.
Exited (sigterm)
```

  - *Reboot emulator*

  - *Remove old emulator and create newest emulator*
