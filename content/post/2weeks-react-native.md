---
date: 2016-04-12T22:24:33+08:00
title: "两周 React Native 开发小结"
tags: ["React Native", "App"]
---

将原有的 Swift 代码改成 React Native 是个不小的动作，我们选择了一些对主要功能影响不大的页面定点重构，主要考虑到 React Native 的几个优点：

1. 跳过 App Store 审核，远程更新代码，提高迭代频率和效率；
2. 减少编译时间，开发效率更高；
3. 组件化开发，更高的复用率；
4. 相对其它 Hybrid 方案，React Native 性能更好，用户体验更接近原生。

经过我一个人两周的折腾，发现这些优势并不能在我们的 App 里最大化。最后我把所有 React Native 写的代码全部删除，花了两天时间用 Swift 重写了一遍。

下面，我来聊聊为什么我们的 App 没能享受这些优势。

### 迭代频率和效率

为了更好地发布版本，我使用了 [react-native-auto-updater](https://github.com/aerofs/react-native-auto-updater)，这是一个比较成熟的更新方案。我把 jsbundle 放在 AWS S3 上，每次发布更新 metadata.json。用户启动 App 时检查是否有适合当前 App 版本的新 jsbundle，如果有，则偷偷地更新，并在下一次启动时使用新版。我们用的这个算是比较保守的更新策略，缺点是用户不能第一时间用上新版，好处是不会因为在使用中更新而产生副作用。

虽然不再经过 App Store 慢长的审核（平均一周），但是新的版本也要测试吧，也要有个发布的周期吧，在只有我一个人写 jsx 的情况下，发布周期控制在几天内合适呢？我能很快地更新一个页面，修正某个逻辑错误，但是，首先，我们用 [JSPatch](https://github.com/bang590/JSPatch) | [WAX](https://github.com/probablycorey/wax) | [Rollout](https://rollout.io) 等方案也能（但相对略费力地）达到这个目的；其次，我们做的不是电商，并没有这样频繁的更新需求。

### 减少编译时间，开发效率更高

几乎可以略过不计的编译时间的确能诱惑大部分被 Swift 慢长的编译时间摧残过的灵魂。Live Reload 功能让你在每次保存后都能立刻看到更新后的页面。0.23.0 新增的 Hot Reload 功能可以在不刷新页面的情况下看到更新后的结果。但其实编译在整个开发过程中只占很小的比例，尤其是对于小 App 来说。更何况，现在可以用 [InjectionForXcode](https://github.com/johnno1962/injectionforxcode) 在运行时注入更新的代码，可以用 [Stevia](https://github.com/s4cha/Stevia) 实现 Hot Reload 布局。再者，有 FLEX 和 Reveal 等调试工具可以辅助开发，新版本 Xcode 编译 Swift 也越来越快（不是幻觉吧），Swift 写的 Playground 开始支持用户交互，所以我并不觉得 React Native 在这一项上占有绝对优势。

不过，倒是有几个影响 React Native 开发效率的点。比如 Nuclide 比较卡，代码提示也非常有限，我试过换 Sublime Text 和 WebStorm，没法像 Xcode 7.3 那样无脑地补全。当然还有震惊全球的 left-pad 事件也正好发生在我折腾 React Native 的这两周里。

还有，由于大部组件，包括 Navigator 实现的导航，都无法通过 UIAppearance 实现统一定制，社区中的组件也没有统一的风格，所以为了让整个 App 看起来有统一的风格，我不得不花费大量的时间在写 jsx 的样式中。而且因为 jsx 中的样式和 CSS 还不一样，和 JSON 比较像，不太容易在组件之间复用，我干了不少体力活。

### 组件化开发，高复用率

我发现大部分原生 App 开发人员不喜欢写 Hybrid App，对 React Native 这种方案也是持观望态度。在 React Native 社区混迹的这两周里（其实去年断断续续玩过一阵子，但是因为当时 Nuclide 卡成翔...），我发现大部分活跃的开发人员是 web 前端开发人员，其中大部分人没有自制 iOS 组件的能力，以至于整个社区没有像原生社区那样频繁出现一些高质量的、持续更新的原生组件。

并且，最要命的时，像我们这种创业小公司，招 iOS 开发已经很不容易，更不要说招一个会 React Native 的 iOS 开发了（这句好像是在夸自己?）。

在找不到好用的组件的情况下，或者说大部分组件拿来后都要大修大改的情况下，实现 App 的效率其实非常低，这效率取决于你有多少好用的组件，而这一点在目前 native 社区里的几乎不是问题。React Native 的社区有点像早期的 Objective-C 社区，大家都在忙着做一些基础的控件，很多东西都没有打磨好，你很容易被动地陷入为社区贡献青春的伟大理想中，但是产品汪和设计狮都期待着你能做出 Dribbble 上的某个动效。

### 性能更好，用户体验接近原生

是的，在一些简单的由 View 拼装的界面中，我很难感觉到各种交互有任何延时。不过，由于我只用 React Native 替换了 App 中某些部分，根据官方 repo 中的建议，需要在 React Native 页面还没被展示的时候，预先加载 RCTBridge 和 RCTRootView。这样做的后果是，在没有复杂页面的情况下，App 中 Swift 代码占的内存和 RCTRootView 起来后占的内存加起来有 120MB。不这样做的话，第一次展示 RCTRootView 时能明显地感觉到白屏。

上面扯到的这些可能暴露了我对 React Native 片面的认知，如果讲错了还希望你能指正。

现在我个人对它的态度是继续观望并且建议 Swift 开发者持续专心地写 Swift。