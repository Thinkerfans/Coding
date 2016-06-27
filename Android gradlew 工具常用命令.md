# Android gradlew 工具常用命令
系统环境:Mac   
Studio:Android Studio 2.1.1   
JRE:```1.8.0_65-b17 x86_64``` 


通过执行  
```./gradlew tasks```   
来查看所有的tasks，截取常用的如下： 

```
All tasks runnable from root project
------------------------------------------------------------

Android tasks
-------------
androidDependencies - Displays the Android dependencies of the project.
signingReport - Displays the signing info for each variant.
sourceSets - Prints out all the source sets defined in this project.

Build tasks
-----------
assemble - Assembles all variants of all applications and secondary packages.
assembleAndroidTest - Assembles all the Test applications.
assembleDebug - Assembles all Debug builds.
assembleRelease - Assembles all Release builds.
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
clean - Deletes the build directory.

Install tasks
-------------
installDebug - Installs the Debug build.
installDebugAndroidTest - Installs the android (on device) tests for the Debug build.
installRelease - Installs the Release build.
uninstallAll - Uninstall all applications.
uninstallDebug - Uninstalls the Debug build.
uninstallDebugAndroidTest - Uninstalls the android (on device) tests for the Debug build.
uninstallRelease - Uninstalls the Release build.

Verification tasks
------------------
check - Runs all checks.

Other tasks
-----------
clean

-----------
To see all tasks and more detail, run gradlew tasks --all
To see more detail about a task, run gradlew help --task <task>

BUILD SUCCESSFUL

```


```./gradlew -v```   
查看当前gradlew版本


```./gradlew clean```   
删除project构建生成的文件

```./gradlew assemble```   
生成project输出文件，debug和release对应的APK都会生成。可单独执行        
```gradle assembleRelease```或者```gradle aR``` 只生成release对应的APK   
```gradle assembleDebug ```只生成debug的APK


```./gradlew build```   
检查依赖并编译打包，This task does both assemble and check


```./gradlew installRelease ```或```./gradlew iR ```   
Release模式打包并安装到手机上

```./gradlew uninstallRelease ```或```./gradlew uR ```      
从手机上卸载Release的APK


参考链接：  
[Android gradlew](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Android-tasks)