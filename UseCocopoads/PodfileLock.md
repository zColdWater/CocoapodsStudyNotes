# 使用篇Podfile.lock是什么? 

汇聚知识，分享精华。  

> 其一 弄清楚我们搞清楚Podfile.lock是什么，怎么产生的？

> 其二 弄清楚Podfile.lock如何使用在实际的开发中?


Podfile.lock是什么，怎么产生的？
------
下面内容来自官方文档，如果有兴趣的同学[点击官方介绍](https://guides.cocoapods.org/using/using-cocoapods)。
> This file is generated after the first run of pod install, and tracks the version of each Pod that was installed. For example, imagine the following dependency specified in the Podfile:

>* pod 'RestKit'

> Running pod install will install the current version of RestKit, causing a Podfile.lock to be generated that indicates the exact version installed (e.g. RestKit 0.10.3). Thanks to the Podfile.lock, running pod install on this hypothetical project at a later point in time on a different machine will still install RestKit 0.10.3 even if a newer version is available. CocoaPods will honour the Pod version in Podfile.lock unless the dependency is updated in the Podfile or pod update is called (which will cause a new Podfile.lock to be generated). In this way CocoaPods avoids headaches caused by unexpected changes to dependencies. 

转译: 

 **第一点:** 当项目没有`Podfile.lock`文件的时候 执行`$ pod install` 就会生成一个新的`Podfile.lock`文件。
 
 **第二点:** `Podfile.lock` 一旦生成，比如上面的例子，pod 'RestKit' 这种写法，默认是拉取最新的版本，举个例子，比如我现在第一次运行`$ pod install`，这个时候会生成一个`Podfile.lock`文件，并且里面指明了`Rest`确切的版本，比如 `RestKit 0.10.3`，这个操作是你在你的本机电脑上，那么现在，把你生成好的`Podfile.lock`文件添加到你的版本管理git上，其他同学下载下来，再执行`$ pod install`，那么仍然安装的是 0.10.3的版本，因为`Podfile.lock`没有被重新生成，即使这个时候RestKit有新的版本，比如0.11.1。  
 
 **第三点:** 除非更改`Podfile`里面的依赖设置，比如 `pod 'RestKit',0.11.1` 这种，重新指明了版本，这个时候会重新生成`Podfile.lock`文件，还有执行`$ pod update`也会重新生成`Podfile.lock文件`。

注意⚠️: 你可能会有疑问🤔️ **更改`Podfile`的依赖会可能触发`Podfile.lock`的重新生成**


总结
------
第一: `Podfile.lock`文件 官方推荐 是加入到版本管理当中的，也就是在开发的时候不能随意删除`Podfile.lock`文件，尤其是在多人开发的时候。  

第二: `Podfile.lock`文件，锁定的版本在依赖不能被满足的时候会触发更新，例如:原来我们的`Podfile.lock`文件里面的版本是`0.1.5`，比如现在`Podfile` 里面指定这个Pod的版本约束是 `> 0.1.7`，那么这个时候`Podfile.lock`文件就被更新为当前 `> 0.1.7`的最新版本，比如这个Pod已经开发到`0.1.9`了，那么你在`Podfile.lock`文件里面将看到版本从`0.1.5` -> `0.1.9`。

**第三: 所以`Podfile.lock`文件很有用的，它是防止一些不被期待的更新，例如: `Podfile` 的约束是  `> 0.1.5`，
那么比如当前的`Podfile.lock` 里面的约束是 `0.1.6`，即使现在这个Pod已经更新到了 `0.1.10`，你执行`$ pod install`一样不会更新 `Podfile.lock` 文件，因为`0.1.6`版本依然符合Podfile中制定的版本约束，只有当约束不能被满足才会触发更新。**














