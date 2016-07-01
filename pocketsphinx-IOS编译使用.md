#pocketspinx-IOS编译使用
系统环境:Mac   
Xcode: Version 7.3.1 (7D1014)  


pocketsphinx-ios-demo跟android一样也是踩着坑走来，在此小结记录下：

### pocketspinx-IOS静态库编译流程：
按住官网给出的流程走   
1. 新建一个目录cmusphinx，在此目录下载最新的```sphinxbase```, ```pocketsphinx``` and ```pocketsphinx-ios-demo```源码。   
2. 搭建编译环境,安装Autogen工具。     
3. 在```sphinxbase ```目录下执行```./autogen.sh```   
4. 在```pocketsphinx```目录下执行```./autogen.sh```   
5. 在```sphinxbase ```目录下执行```../pocketsphinx-ios-demo/build_iphone.sh```,在```sphinxbase/bin```目录下查看编译生成的库文件.  
6. 在```pocketsphinx ```目录下执行```../pocketsphinx-ios-demo/build_iphone.sh``` 在```pocketsphinx/bin```目录下查看编译生成的库文件.    
7. 利用lipo工具合并.a库文件，兼容各个架构。


### pocketspinx-IOS使用Demo：
参考android jni 里面的使用。

####问题纪录：
问题一:
```./build_iphone.sh: line 52: ~/cmusphinx/pocketsphinx-ios-demo/configure: No such file or directory```
原因：没有认真阅读README.md说明
解决方法：如上流程。

问题二:   
语音识别非常不准确？    
原因：pocketsphinx语音识别默认的采样率是16000，而我在录音频时设置采样率为8000，将二者改成一致，识别准确率就上来了。


参考链接：    
[pocketsphinx-ios-demo](https://github.com/cmusphinx/pocketsphinx-ios-demo)   
[OpenEars](http://www.politepix.com/openears/)
[TLSphinx](https://github.com/tryolabs/TLSphinx)  
[sphinx-demo](https://github.com/aperturescience/sphinx-demo)    
[VocalKit](https://github.com/KingOfBrian/VocalKit)