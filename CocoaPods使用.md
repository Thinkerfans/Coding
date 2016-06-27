###CocoaPods安装

### CocoaPods使用
使用可分三步走：

> 1.在项目***根目录下***新建一个名为Podfile的文件  
> 2.将依赖库名字按照如下规则填入即可  
> > platform :ios  
> > pod 'JSONKit',       '~> 1.4'   
> > pod 'MBProgressHUD'   
> 
> 3.执行pod install 来下载安装依赖库 
 
若有需要，可执行pod update来更新依赖库。

参考资料：  
[用CocoaPods做iOS程序的依赖管理](http://blog.devtang.com/2014/05/25/use-cocoapod-to-manage-ios-lib-dependency/)
