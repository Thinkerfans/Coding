#pocketspinx-android编译使用
系统环境:Mac   
Studio:Android Studio 2.1.1   
JRE:```1.8.0_65-b17 x86_64```  
SWIG ```Version 3.0.8```   
Gradle ```2.13```  


项目需要用到语音识别命令，于是选择用pocketspinx-android开源库来实现，踏着坑一路走来，在此小结记录下：

####so库，jar包编译流程：
按住官网给出的流程走   
1. 新建一个目录cmusphinx，在此目录下载最新的```sphinxbase```, ```pocketsphinx``` and ```pocketsphinx-android```源码。   
2. 搭建编译环境,安装Gradle、SWIG，Android SDK及NDK。     
3. 配置sdk路径及版本、ndk路径: 最新版本的pocketsphinx-android目录下没有```gradle.properties```文件，新建此文件，添加如官网给出路径。   
4. 在```pocketsphinx-android```目录下执行```gradle build```即可


####问题纪录：
问题一:
```Could not find property 'sdkDir' on org.gradle.api.internal.artifacts.dsl.dependencies.DefaultDependencyHandler_Decorated@32e54a9d.```
原因：设置了sdkDir但老是提示找不到，还没找到具体原因？
解决方法：直接用绝对路径来处理，如下：
>
```
dependencies {
    compile files("/Users/cfans/Documents/android-sdk-macosx/platforms/android-23/android.jar")
}
task ndkBuild(type: Exec) {
    commandLine "/Users/cfans/Documents/ndk/android-ndk-r10e/ndk-build"
}
```

问题二：
```
make: *** No rule to make target `../sphinxbase/src/libsphinxbase/feat/cmn_prior.c', needed by `obj/local/arm64-v8a/objs/sphinxfeat/cmn_prior.o'.  Stop.
:ndkBuild FAILED
```
原因：找不到```cmn_prior.c```文件，原来最新版本的sphinxbase里面已经改成```cmn_live```文件。   
解决方案：在Andorid.mk里面搜索```cmn_prior.c```替换成```cmn_live```


参考链接：  
[pocketsphinx-android](http://cmusphinx.sourceforge.net/wiki/tutorialandroid)   
[编译 pocketsphinx-android](http://www.android-studio.com.cn/article/38)
