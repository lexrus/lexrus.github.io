---
date: 2016-07-10T19:40:02+08:00
title: "为什么改用 Carthage"
tags: ["Carthage", "CocoaPods"]
---

在这个几乎每一个开源 iOS 第三方组件都支持 [CocoaPods](https://github.com/CocoaPods/CocoaPods) 的时代，为什么要选择另一个组件支持数量少、项目配置相对繁琐、无法直接查看组件源码的包管理工具呢？
[Carthage](https://github.com/carthage/carthage) 到底好在哪呢？

最初看到 Carthage 第一眼就被「去中心化」这个高大上的名词给吓到了，但是它并不是个很复杂的事。在项目中，原来我们把一个组件做成 Pod，需要写 Podspec，用 development pod 或者更新 Specs 仓库的项目，私有组件还要更新、指定自己的 Specs 源。

最早 CocoaPods 还没有流行的时候，我曾是 CocoaPods/Specs 仓库的主要贡献者之一。当时第三方组件默认的安装方式就是拖拽和 git submodule，CocoaPods 为了推广自己，有一个建议第三方仓库支持 CocoaPods 的文字模板，以 issue 的形式发到对方的仓库里。让仓库拥有者辛辛苦苦地写一个 podspec，还要更新 README 一般都要等上好几天，而且很难一次就做对。所以最好都是帮对方写好 podspec，改好 README，发 Pull Request，再代为更新 CocoaPods/Specs 仓库，非常费事费力。

而组件支持 Carthage 的唯一要求就是，项目的里有一个 shared Framework target 存在。每次更新组件都不需要去更新任何 Podspec 或 Specs 仓库。私有仓库不需要额外设置 Specs 仓库，不需要 pod trunk。去中心化，意义非凡。

另一个 Carthage 的设计优势是，先天只支持 Dynamic Framework，只在更新时编译，这是为 Swift 项目量身定制的特性。在发版本时不需编译所有依赖，在项目 clean 时不需要重新编译所有依赖，开发者只有在用 carthage update 更新组件后才需要重新编译组件，而且一般只做一次。

另外组件作者可以进一步提供编译好的 Framework 压缩包，随 release 发布，节省使用者的编译时间。试想如果项目中的每一个依赖都这么做了，carthage update 会像 apt-get 一样又快又好用。

再来说说公认的缺点。项目配置的步骤的确不如写一个 Podfile 然后 pod install 那样简单，但其实也没有多么痛苦吧。Carthage 不会像 CocoaPods 那样对项目做大量改动，也没有要求一定要用 xcworkspace。甚至如果放弃打包 dsym，不用 copy-framework 脚本也是可以的。至于不能直接点到源码，可以用拖入 Carthage/checkouts 目录中的 xcproj 的方式来临时解决，这样就和使用 CocoaPods 或 git submodule 差不多了。

最后分享近两年使用 Carthage 发现的一些小技巧，但我觉得是 Carthage 的 CLI 命令设计得比较奇葩。如：

- 只更新不编译：`carthage update --no-build`
- 只编译特定依赖的 iOS 版: `carthage build --platform iOS RxSwift`
- 删除一个依赖的时候不需要重新 update，只要删除 Cartfile.resolved 中相应的行，和 Carthage 中的目录即可
- 如果经常用的库没有提供编译好的 Framework，可以 fork 一个自己提供，然后就不用每次都编译了

最后我想说的是，Carthage 并没有占据绝对的上峰，有很多常用组件如 Facebook 的大部分 SDK、Lookback SDK、Fabric Framework 等都不支持 Carthage。选择自己习惯的、适合当前项目的包管理工具，以及使用配置更好的 Mac 能省去不少时间。