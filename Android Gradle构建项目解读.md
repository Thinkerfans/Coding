#Android Studio
系统环境:Mac   
Studio:Android Studio 2.1.1   
JRE:```1.8.0_65-b17 x86_64```   


### Studio 新建Android项目目录结构解读
``` 
├── .gradle/ #gradle项目的文件(自动编译工具产生的文件)
├── .idea/ #idea项目文件(开发工具产生的文件)
├── app/ #Android项目文件
│   ├── build #app module构建输出目录
│   ├── libs   
│   ├── src  
│   ├── app.iml  
│   ├── build.gradle #app module构建脚本
│   └── proguard-ruls.pro  #混淆配置
├── build/ #     
├── gradle/   
│   └── wrapper/      
│       ├── gradle-wrapper.jar   
│       └── gradle-wrapper.properties  
├── build.gradle #项目构建脚本  
├── gradle.properties #gradle配置 
├── gradlew #
├── gradlew.bat #  
├── local.properties #配置Androod SDK位置文件(Studio自动生成) 
├── 项目名.iml # 
└── settings.gradle #项目配置
```
常用的几个文件或文件夹功能说明：
#### 1.setting.gradle 文件   
用于配置project，标明其下有哪几个module。新建项目默认只有app这一个，如下：   
```include ':app' ```    
当新建一个module时，Studio会自动以```':modulename'```格式添加，之间用英文逗号隔开,如：  
```include ':app' , ':libmina' ``` 

#### 2.build.gradle project构建脚本 
```
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.1.0'
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
allprojects {
    repositories {
        jcenter()
    }
}
task clean(type: Delete) {
    delete rootProject.buildDir
}
```

- buildscript{}：   
配置project建构脚本的类路径
- repositories{}：   
配置project建构的仓库，常用的两个标准的Android library文件服务器：[jcenter](http://jcenter.bintray.com)与[mavenCentral](https://oss.sonatype.org/content/repositories/releases/)。
- jcenter()：     
配置project建构仓库为jcenter，即依赖的jar包全部从此仓库获取并下载
- dependencies{}：   
配置project建构的依赖库。比如：  ```classpath 'com.android.tools.build:gradle:2.1.0'```     
Studio会自动从jcenter仓库里去找2.1.0版本的gradle依赖库。
- allprojects{}：  
为所有projects的仓库源配置为jcenter
- task clean(type: Delete){}：  
声明一个任务，任务名叫clean，任务类型：Delete，任务动作：删除buildDir路径
- delete rootProject.buildDir：    
执行删除路径动作

#### 3.build.gradle module构建脚本 
```
apply plugin: 'com.android.application'

android {
    compileSdkVersion 23
    buildToolsVersion "23.0.3"

    defaultConfig {
        applicationId "com.example.cfans.myapplication"
        minSdkVersion 15
        targetSdkVersion 23
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:23.4.0'
}
```

- apply plugin: 'com.android.application'   
表示该module是一个App module，应用了com.android.application插件，而android另一个常用的插件为com.android.library对应Library module。

- android{}   
配置了所有的特定于Android的构建参数。常用的参数如下：
> 1. compileSdkVersion： SDK版本
> 2. buildToolsVersion： buildTools版本
> 3. [defaultConfig](http://google.github.io/android-gradle-dsl/current/com.android.build.gradle.internal.dsl.ProductFlavor.html)： 构建APK时默认的配置，在此可以进行应用包名，版本号，版本名称等配置。
> 4. [buildTypes](http://google.github.io/android-gradle-dsl/current/com.android.build.gradle.internal.dsl.BuildType.html): 构建类型，常用的有debug与release两种类型。在此可以进行签名、混淆、调试、zipalign等配置。
- dependencies{}  
在此导入该module所需依赖库：
```compile fileTree(dir: 'libs', include: ['*.jar'])```
导入module libs目录下所有jar包。
```compile 'com.android.support:appcompat-v7:23.4.0'```
导入jcenter仓库中的v7此jar包。

[jcenter()延伸](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0623/3097.html)


### Studio 如何使用第三方jar包
studio导入第三方jar比eclipse便捷灵活多了，可以通过如下三种方式来实现，一般采用第一种方式。
#### 1. 在module里的构建脚本直接添加
直接在build.gradle里面添加如下一行代码，同步成功后即可使用。
```compile 'com.google.code.gson:gson:2.6.2'```

#### 2. 界面添加
> 1. 选中对应的module，单击右键->Open Module Settings->Dependences
> 2. 点击加＋号，输入GSON名称，然后点击搜索，选中```com.google.code.gson:gson:2.6.2```，然后点击OK   
> 3. 同步成功后，在build.gradle里查看添加结果:
```compile 'com.google.code.gson:gson:2.6.2'```


#### 3. 直接拷贝到对应module的lib目录下

### Studio 如何签名
studio签名也提供两种方式来实现。
#### 1. 通过界面来实现
这种方式跟eclipse里面签名几乎一致:
> build菜单 -> Generate Signed APK -> 选中对应的module -> 配置key信息 -> 配置输出路径 -> XX.APK   

#### 2. 通过gradle自动签名来实现
这种方式前提是已经有了keystore文件。
> 1、在对应module下build.gradle 中配置签名信息。    
> 
```  
signingConfigs {
        release {
            keyAlias "xxx"
            keyPassword "xxx"
            storeFile file("release.keystore")
            storePassword "xxx"
        }
    }
```
> 2、在Studio下方的Terminal终端通过如下命令生成带签名的APK
>
```
./gradlew assembleRelease 
``` 
如果你手动安装gradle并配置好环境变量，也可以执行
```gradle assembleRelase```   
来生成带签名的APK，但为什么都提倡用gradlew,首先gradlew 是gradle wrapper的简写，即对gradle工具做了包装处理，解决gradle本地安装、配置环境变量、gradle版本不一致、过时等带来的一系列麻烦事。

[图文说明](http://www.cnblogs.com/smyhvae/p/4456420.html)  
[Sign Your App](https://developer.android.com/studio/publish/app-signing.html)
### Studio 如何使用JNI


### Studio 如何运行java程序
#### 1. 如何运行java程序
1. File->New Module->Java Library
2. 需手动编写入口函数 
```
    public  static void main(String[] args){}
```
3. 第一次运行时，需单击右键->选择Run 'XX.main()'即可；  
或者，手动配置:Run-> Edit Configurations-> '+' -> Application 然后在Configuration栏配置如下两项再运行即可：   
Use classpath of module:// module所在的路径   
Main class: //入口函数所在的类

#### 2. 如何生成jar包
Android Studio运行java程序后，在目录此module/build/libs/下会自动生成包含module所有的java文件的jar包。   
可使用 ```jar vtf **.jar``` 查看jar包包含哪些class文件。

## 相关概念
### 1.Groovy
1. [Groovy](https://zh.wikipedia.org/wiki/Groovy)
 是Java平台上设计的面向对象编程的**动态语言**。
2. 拥有类似[Python](https://zh.wikipedia.org/wiki/Python)、[Ruby](https://zh.wikipedia.org/wiki/Ruby)和
[Smalltalk](https://zh.wikipedia.org/wiki/Smalltalk)中的一些特征，可以作为[Java](https://zh.wikipedia.org/wiki/Java)平台的脚本语言使用。
3. 语法与Java非常类似，以至于多数的java代码也是正确的Groovy代码。Java是编译型语言，而Groovy代码动态的被编译器转换成字节码。

### 2.Gradle

1. [Gradle](https://docs.gradle.org)
**是一个自动化建构工具**：一个基于
[Apache Ant](https://zh.wikipedia.org/wiki/Apache_Ant) 和
[Apache Maven](https://zh.wikipedia.org/wiki/Apache_Maven) 
概念的项目自动化建构工具。

2. Apache Ant，是一个将软件编译、测试、部署等步骤联系在一起加以自动化的一个工具。build.xml 文件包含一个project和至少一个target元素。
3. Apache Maven，是一个软件（特别是Java软件）项目管理及自动构建工具。Maven项目使用称为项目对象模型 POM（Project Object Model）来配置的,存储在命名为 pom.xml 的文件中。


###3.DSL


[Android Plugin DSL Reference](http://google.github.io/android-gradle-dsl/current/index.html)   

### 4.package VS applicationId

1. The final package that is used in your built .apk's manifest, and is the package your app is known as on your device and in the Google Play store, is the "application id".
2. The package that is used in your source code to refer to your R class, and to resolve any relative activity/service registrations, continues to be called the "package".
3. package位于manifest.xml根节点，而applicationId位于app/build.gradle文件的defaultConfig节点内。
4. 同时多渠道，多包名打包APK，彻底解放了之前在Eclipse里面修改包名导致的一堆问题。   

[官方解释](http://tools.Android.com/tech-docs/new-build-system/applicationid-vs-packagename)

### 5.zipalign工具
[zipalign](https://developer.android.com/studio/command-line/zipalign.html)    是为APK文件提供极大优化的一个归档对齐工具。   
作用：使APK运行时占用更少的RAM。

####zipalign工具使用
对齐一个infile.apk文件，并将其保存为outfile.apk 
   
```zipalign [-f] [-v] 4 infile.apk outfile.apk```
确认一个existing.apk是否zipalign处理过。   
```zipalign -c -v 4 existing.apk```

####注意事项:
zipalign必须在apk用私钥签名后执行。
---

参考资料:  
[特定领域语言](http://www.ituring.com.cn/article/59194)  
[Android Studio Project Site](http://tools.android.com/tech-docs/new-build-system/gradle-experimental)   
[AndroidDevTools](http://www.androiddevtools.cn)    
[Gradle 翻译](http://wiki.jikexueyuan.com/project/GradleUserGuide-Wiki/)  
[飞雪无情的博客](http://www.flysnow.org)

