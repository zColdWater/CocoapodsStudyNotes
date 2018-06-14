# 使用篇(UsingCocoapods) 《二》 

汇聚知识，分享精华。  

> 这篇主要的内容来分享一下，我们应该如何编辑Podfile文件呢？

> Podfile 里面可以设置 哪些关键字呢? 分别起到什么作用呢?


Xcode项目结构
------
在了解`Podfile`之前我们先了解一下我们经常使用的Xcode项目中的构造.
通常来说，我们的项目结构，1个Workspace可以引用多个Project，1个Project可以引用多个Target，如下：

```
Workspace.
├── ProjectA
│   ├── TargetA
│   └── TargetB
└── ProjectB
    ├── TargetC
    └── TargetD
```


Podfile
------
> 如果你还从来没有使用过Cocoapods来集成iOS项目，那么可以看之前的文件[使用篇 (UsingCocoapods)《一》](https://github.com/zColdWater/CocoapodsStudyNotes/blob/master/UseCocopoads/README.md)

什么是Poodfile? <br>

答曰: 就是一个名字叫`Podfile`的`Ruby`文件，注意它其实是一个`Ruby`文件，所以我们在`Podfile`里面的设置要遵守`Ruby`的语法来哦。

> 官方描述: The Podfile is a specification that describes the dependencies of the targets of one or more Xcode projects. The file should simply be named Podfile. All the examples in the guides are based on CocoaPods version 1.0 and onwards.

> 中文意思: 这个文件是用来指定三方包依赖关系的在你的XcodeProject里面，指明哪些`target`依赖哪些三方包。

Podfile 可以非常简单，例如我们将`Alamofire`添加在一个`Target`: 

```ruby
target 'UsingCocoapods' do # UsingCocoapods 是你 Xcode Project 里面的Target名称
  use_frameworks!
  pod 'Alamofire', '~> 3.0'
end
```

或者你有一个更加复杂的Podfile的设置

> 这里需要提前说明的有几点: 

> `source`: 是一个远端地址，存放Pods配置文件的地方（所谓配置文件就是`Podspec`文件，如果你不懂什么是`Podspec`文件，也没关系，后面的文章会有介绍，配置文件里面包含版本等信息，而版本也是你最经常用到的）。

 >* 这个地址有可能是公开的比如`Github`上面的仓库，又有可能是私有的比如`Gitlab`公司的仓库地址。

 >* 你要明确你引的Pods在哪？他们是在公开地址仓库上还是在私有地址仓库上，你要根据你引入的Pods库，来使用`source`声明版本库地址，否则Cocoapods拉取的时候，会抛出不能找到Pods的异常。
 
 >* 如果你引入一个自己公司制作的Pods库，这个库的地址是需要权限才能进入的，称为私有库，比如`Pod 'PrivateLib'`，然后你用`source`声明你私有库存放版本的地方，`source 'https://privateHost/Path/Specs.git'`，这样对那些没有私有库权限的人来说，就没办法拉取Pods库进行集成了。





> `platform`: 你要集成在哪个平台上，比如 `platform :ios, '8.0'` or `platform :watchos, '2.0'`，声明你是iOS平台还是WatchOS平台的，还有其他平台Apple平台，看Cocoapods的支持了，对我来说我是做iOS手机软件开发的，我用的`plaform:ios, '8.0'`。

>* 常用的platform有: osx,ios,tvos,watchos。
>* 平台后面的Version: 如果你不声明版本，默认是 `4.3 for iOS, 10.6 for OS X, 9.0 for tvOS and 2.0 for watchOS`. 这个版本是用来表明你现在工作的iOS版本，如果你是开发iOS8的客户端，那么你也应该在你的Podfile里面声明`plaform:ios, '8.0'`。
>* 注意⚠️: 如果这个时候有一个Pods是基于ios 9.0开发的([怎么知道它是基于iOS几开发的？]())，假设是这个 Pod 'PrivatePod-iOS9', 它是基于iOS9开发的，也就是最编译环境是iOS9，那么这个时候你在执行`$ pod install` 命令进行集成的时候，Cocoapods工具会抛出异常，异常提示:`Specs satisfying the `PrivatePod-iOS9` dependency were found, but they required a higher minimum deployment target.`，解释就是:你当前开发客户端iOS8的环境已经低于`PrivatePod-iOS9`Pods库的最低版本。

> `target`: 

>* Defines a CocoaPods target and scopes dependencies defined within the given block. A target should correspond to an Xcode target. By default the target includes the dependencies defined outside of the block, unless instructed not to inherit! them.
>* 翻译过来: Target会默认继承外面的Pods依赖，如果你想不继承外面Target的依赖需要特别指明，比如inherit! :none.
>* 注意⚠️:  [Cocoapods官方原文](https://guides.cocoapods.org/syntax/podfile.html#inherit_bang) <br>  Available Modes: + :complete The target inherits all behaviour from the parent. + :none The target inherits none of the behaviour from the parent. + :search_paths The target inherits the search paths of the parent only. 
>* 对于`Inherit`这个属性，我的理解是， `inherit! :complete` 在内层Target是默认行为，就是Copy和父Target一模一样的配置，如果你要特殊指明使用`inherit! :search_paths`和`inherit! :none`，这里要说一声抱歉，对于`none`还好理解一点，就是不继承任何父Target的依赖，`search_paths`本人未找到一个比较明确的答案，没有读Cocoapods源码，才疏学浅请各位请多包涵，希望有明白人指点一二，从字面意思就是继承了父Target`search_paths`，一般使用在 **Test Target**。
>* 总结: 一般使用Target属性，使用默认属性`inherit :complete`就可以了，如果是**TestTarget**一般添加`inherit :search_paths`，当然如果你就是不想继承任何外部的Target模块的依赖，直接使用`inherit :none`就可以了。

```
# source 写在这里作为全局的一个属性，当然你也可以单独指明Pods的`source`来源 例如:
# pod 'GoogleAnalytics', :source => 'https://github.com/CocoaPods/Specs.git'

source 'https://github.com/CocoaPods/Specs.git'  
source 'https://github.com/Artsy/Specs.git'

platform :ios, '9.0'
inhibit_all_warnings! # 抑制Warning 官方文档(https://guides.cocoapods.org/syntax/podfile.html#inhibit_all_warnings_bang)

target 'MyApp' do
  pod 'GoogleAnalytics', '~> 3.1'

  # Has its own copy of OCMock
  # and has access to GoogleAnalytics via the app
  # that hosts the test target

  target 'MyAppTests' do
    inherit! :search_paths  
    pod 'OCMock', '~> 2.0.1'
  end
end

```

**以上是最基本的一个Pods使用，在[使用篇(UsingCocoapods) 《三》](https://github.com/zColdWater/CocoapodsStudyNotes/blob/master/UseCocopoads/UsingCocoapods3.md)中，我们会详细讨论，Podfile中的版本的引入方法，Podfile的其关键属性的使用。**




