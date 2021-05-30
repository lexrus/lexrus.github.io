---
date: 2016-04-15T22:52:44+08:00
title: "Android 开发从入门到放弃"
tags: ["Android", "App"]
---

谁都不是生来就是 Android 黑啊，我也不例外。

我曾抱着成为移动端全能的梦想，决心补上 Android 开发这块短板，收集了不少 Android 开发的资源，花了几十分钟下载 SDK，信心满满地编译 Hello World。然后，对着屏幕看了十分钟不止才看到结果。Google 的文档可不会告诉你直接插 USB 真机测试会快很多啊，也没有说推荐用友商的 [GenyMotion](https://www.genymotion.com)，当时我的心是凉的。

后来为了有愉快的开发体验，搞了台当时算速度不错的 SONY Xperia Z1 防水手机，一心想做个适合在水里用的 Android App。一个人脑暴了很久，想了很多不靠谱的 idea，终因忙 iOS 项目，没有一个做出来。

不过在使用 SONY Xperia Z1 的过程中，我发现有些我在 Motorola XT301 上遇到过的问题在这几年里并没有改善。

比如说，市场混乱。你会发现几乎每个手机厂家都喜欢出自己的 App 市场，并且自带的 ROM 不带 Google Play。甚至于某些和运营商合作的手机，会自带两个市场。再加上一些被公认做得还不错的既不是手机厂也不是运营商的市场，XXX 手机卫士，一台 Android 手机带数个市场算是很正常的事。这些市场并没有哪个处于绝对的垄断地位。虽说竞争是好事，但这种鱼龙混杂的场面，让用户在选择时有些许迷茫。同时，开发人员发布 apk 也是件很累的事。所以有人做了 [Android 多渠道打包工具](https://github.com/mcxiaoke/gradle-packer-plugin)，还有些提供代发布的服务如 [酷传](http://www.coolchuan.com) 等。而这些市场中，有很多一看就是垃圾的应用，居然通过了市场的审核发布了。我曾经搜索「招商银行」找到五个长得差不多的 Android App，看到过大量审美超出普通人接受范围的 Android App，下载到不少安装时问你要几乎所有权限的 Android App。

二是，界面设计、交互体验没有统一的范式，大部分 App，需要有个「用熟」的过程。各家都在培养自家 App 的用户习惯，但好在他们之间会时不时地互相取长一下，某些交互趋同，某些则向 iOS 看齐。即便后来有了 [Material Design](https://www.google.com/design/spec/material-design/introduction.html)，市场中界面美观、交互优雅附合新用户预期的 App 依旧还是少数。另外，可笑的是，系统皮肤的排行里，仿 iOS 的皮肤总能在前十中占上三四个位置。

再说硬件，除非你用亲儿子，或者一些比较开放的友商的手机，不然你想刷官方 ROM 是一件很难的事。这看起来并不是什么坏事，直到 Android 6.0 出来几个月后，我的手机还只能用 4.2 时，我就没那么开心了。{{ 这里删除一百字指定品牌和型号的吐槽以防以后有人想送我对应的手机 }}

去年我在新味做 iOS 开发的时候遇到一个需求，负责发货的客服需要实时跟踪快递员的位置，快递员需要上报送货状态。我们统计了一下差不多十二个快递员中只有一个人用 iPhone，我只好把我半调子的 Android 开发技能捡起来。用 gradle 做包管理，ButterKnife 连接各种资源，RxAndroid 简化交互逻辑，Retrofit + gson 实现网络层，启了一个百度定位的 service，在桌面上放了一个 widget 用来快速开关这个 service 并切换送货状态。我试着用各种已知的最佳实践来搞定每一个简单的任务，但相比之下，除了界面布局之外，iOS 开发尤其是使用 Swift 开发时的各种体验更加优雅并高效。写完这个 Android App 后，我更加确定我喜欢 Swift 而不是 Java，喜欢 YAML 而不是 XML，喜欢 Xcode 而不是 Android Studio。

如果我成为一个 iOS + Android 开发和设计的全能，我有一个月时间做一个 App 的 Android 版和 iOS 版，我可能可以把它俩都磨到 60 分及格。但另一个我如果全心全意地只学 iOS 也只磨 iOS，他应该会比前一个我更快地把 iOS App 磨到 90 分以上。基于这个假设，再加上前面提到的不爽、不喜欢和不合理，我果断地删掉了 Android Studio，unfollow 了所有之前关注过的 Android 牛人和知名 Android 开源项目，并用成为 Android 黑这种极端的方式来防止自己重新捡起 Android 开发。