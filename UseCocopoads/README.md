# 使用篇(UsingCocoapods)  《一》

## 列表

1. [使用篇 UsingCocoapods 《一》](https://github.com/zColdWater/CocoapodsStudyNotes/tree/master/UseCocopoads)
2. [使用篇 UsingCocoapods 《二》](https://github.com/zColdWater/CocoapodsStudyNotes/blob/master/UseCocopoads/UsingCocoapods2.md)
3. [使用篇 UsingCocoapods 《三》](https://github.com/zColdWater/CocoapodsStudyNotes/blob/master/UseCocopoads/UsingCocoapods3.md)
4. [使用篇 Podfile.lock 是什么?](https://github.com/zColdWater/CocoapodsStudyNotes/blob/master/UseCocopoads/PodfileLock.md)
  

汇聚知识，分享精华。  
>这篇主要的目的是给大家分享如何使用一个Cocoapods。

>Cocoapods是一个三方包管理工具，类似npm，pip，这种包管理工具，你只需要在`Podfile`文件里面配置一下你想
>引入的三方包，就可以一起集成到你的项目中使用了。

>在iOS开发中类似的工具还有: [Carthage](https://github.com/Carthage/Carthage#quick-start) 

Cocoapods的安装
------
如果你已经安装过了Cocoapods，那么就跳过这个步骤吧，如果没有安装过Cocoapods在你的电脑上，[请点击这里](https://guides.cocoapods.org/using/getting-started.html#toc_3) 。

  
开始使用Cocoapods工具进行项目包的集成
------
>这个阶段我们将从新建立一个 `Xcode project` 开始，到编写我们的配置文件，最后在项目中使用我们引入的三方包。
<br>

### 创建一个新的`XcodeProject`和`Cocoapods`项目
* 比如正常创建一个iOS应用

<img src="https://upload-images.jianshu.io/upload_images/1793544-56c491c297802b1f.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"/>
<img src="https://upload-images.jianshu.io/upload_images/1793544-3174bbe84bc7d456.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"/>
<img src="https://upload-images.jianshu.io/upload_images/1793544-1ad3ddc757cf4fa2.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"/>
<img src="https://upload-images.jianshu.io/upload_images/1793544-6bfd73c9720607b1.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"/>

* 用终端进入到你创建的iOS应用目录下,创建一个`Podfile`通过 `$ pod init`命令,Open你的`Podfile`，你的第一行应该指定`platform`的`version`支持。

<img src="https://upload-images.jianshu.io/upload_images/1793544-df38e70f5cb24b39.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"/>

* 编辑Podfile，并且解开注释。

>解开 `#Platform :ios, '9.0'` To `Platform :ios, '9.0'`,然后保存Podfile,退出。 先 `ESC`, 再`Shift + :` 然后 `wq`,最后Enter回车。 如果有对Vim操作清楚的同学，[请点击这里](https://www.radford.edu/~mhtay/CPSC120/VIM_Editor_Commands.htm)。

<img src="https://upload-images.jianshu.io/upload_images/1793544-1de8d930f60558bc.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"/>

* 使用Cocopods开始集成在项目中

> 第一步 `cd` 你的项目

> 第二步 确保你当前目录和Podfile在同一层级下 执行 `$ ls` 或 `$ ls -a` 查看 

> 第三步 执行 `$ pod install` 或 `$ pod install --verbose`

> 执行完成如下图: 

<img src="https://upload-images.jianshu.io/upload_images/1793544-fe4eff79b5932294.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"/>

* 打开项目，打开xcworkspace,开始运行看一看吧。

> 打开xcworkspace后，你会发现你的workspace管理着两个project，一个是你的UsingCocoapods，一个是Pods，UsingCocoapods就是你编写代码的项目，而Pods就是管理你三方包的项目，他们被UsingCocoapods.xcworkspace囊括其中，一起管理。

> 这个时候你也许会想，为什么我之前是UsingCocoapods.xcproject，而现在执行了`$ pod install`后，就要运行 `.xcworkspace`文件，为什么这个`.xcworkspace`文件的组织结构是这样的？
 
> 这一切都是Cocoapods的脚本帮你集成的，包括.xcworkspace的文件目录结构，Pods文件夹等等。

<img src="https://upload-images.jianshu.io/upload_images/1793544-db16b3ce52050041.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"/>





