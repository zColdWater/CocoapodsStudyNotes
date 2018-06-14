# 使用篇(UsingCocoapods) 《三》 

汇聚知识，分享精华。  

> 这篇我们主要讨论下 Podfile Syntax 常用的有哪些? 怎么使用？

> 如果你更喜欢阅读详细的官方文档 [请点击Cocoapods官方文档](https://guides.cocoapods.org/syntax/podfile.html#pod) 

Pods 指定依赖
------
Cocoapods提供了几种集成方式 [官方文档](https://guides.cocoapods.org/syntax/podfile.html#pod) <br>
附赠 [官方推荐版本管理指南](https://semver.org/)

### 第一种: 指定依赖版本
  * Besides no version, or a specific one, it is also possible to use operators:
  
  * \= 0.1 Version 0.1.  指定版本 0.1版本 **pod 'Alamofire', '0.1'**
  
  * \> 0.1 Any version higher than 0.1. 大于0.1版本 0.2~xxx **pod 'Alamofire', '> 0.1'**
  
  * \>= 0.1 Version 0.1 and any higher version. 大于&等于 0.1  **pod 'Alamofire', '>= 0.1'**
  
  * \< 0.1 Any version lower than 0.1. 小于0.1  **pod 'Alamofire', '< 0.1'**
  
  * \<= 0.1 Version 0.1 and any lower version. 小于&等于 0.1  **pod 'Alamofire', '<= 0.1'**
  
  * \~> 0.1.2 Version 0.1.2 and the versions up to 0.2, not including 0.2. This operator works   based on the last component that you specify in your version requirement. The example is equal to \>= 0.1.2 combined with \< 0.2.0 and will always match the latest known version matching your requirements.  等同于: `0.1.2` <= `'Alamofire', '~> 0.1.2'` < `0.2.0` 
  
##### 这里需要注意⚠️:  
 * 有对`Podfile.lock`不清楚的同学 [请点击这里Podfile.lock做什么的?](https://github.com/zColdWater/CocoapodsStudyNotes/blob/master/UseCocopoads/PodfileLock.md)。
 
 * “\~>” 如果Pods用这个符号引用，代表默认是升级最小版本号的，具体什么意思呢?  
 
 Alamofire', '\~> 0.1.2 相当于 最高要小于0.2.0，最低要大于等于 0.1.2， Alamofire', '\~> 0.1' 相当于最高要小于1.0，最低要大于等于0.1 **这意味什么呢？ 以Alamofire '\~> 0.1.2'为例子，如果Alamofire最新版本已经到了0.1.8，如果你的项目里面没有Podfile.lock文件锁定了之前的版本，那么你的执行 `$ pod install` Cocoapods会使用0.1.8版本并且更新到Podfile.lock文件中，这就是默认升级最小版本号的意思。** 


 
### 第二种: Build Configurations(设置 Configuration)

> 前言，如果对Build Configurations不熟悉，[点击链接]()。

  * 这种指定Pod依赖版本的方式也很简单，就是通过我们在Project中设置的`BuildConfiguration`来设置在指定的`Configuration`才会依赖这个Pods。
  
  * 例如 `pod 'PonyDebugger', :configurations => ['Debug', 'Beta']` ，意味着只有在`Debug` 和 `Beta` 这两个`Configuration` 下才会依赖 `PonyDebugger`





