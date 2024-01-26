---
date: 2016-11-20T19:12:44+08:00
title: "Serverless 简介"
tags: ["Serverless", "AWS"]
---

随着 PaaS、BaaS、SaaS 的流行，服务端开发和维护的工作变得更加便捷，大部分创业公司不再需要专职的运维，服务端开发人员即能搞定日常的服务器维护工作。而近几年，像 Parse、Firebase、CloudKit 这类连服务端开发人员都能省掉的方案更是受到不少创业公司的青睐。做一个 App 不再需要一堆人，试错的成本大大降低。

而各种容器技术的不断进步，利用譬如 Docker 容器实现轻量级的服务器单元，一个新的服务端形态正在成形 —— Serverless，或称 FaaS (Functions as a Service)。下面从我个人的观感随便聊一下我理解的 Serverless。

Serverless 主要应用表现为，功能以 function 为最小的单元，用户访问某个 function 时启动一个容器执行 function 内的程序，执行结束回收这个容器，整个过程快至几个毫秒就能完成。在某些应用场景下，能很好的代替持续在线的服务。

目前最具代表性的 Serverless 解决方案当属亚马逊的 [AWS Lambda](https://aws.amazon.com/lambda/details/)，中小型的服务商如 [Webtask](https://webtask.io)、[hyper.sh](https://hyper.sh) 等，Google Cloud Platform 也正在进行 Cloud Function 的内测。

这些服务有类似的收费政策：按需付费，额度内免费，超额后每次请求计费。

目前我了解到的使用场景不多，从 AWS Lambda 提供的各种介绍来看，我脑补一下 Serverless 适用场景：

1. 运算密集 —— 如图片压缩、数据分析。因为使用 Serverless 方案同一秒里可以运行千上万个 Lambda，能轻易实现传统架构无法实现的超强处理能力，并且只在使用时收费。

2. 为其它服务提供编程支持 —— 例如，当 AWS DynamoDB 数据发生变化时，调用 AWS Lambda 生成 PDF 报表。再如，为 AWS API Gateway 提供自定义权限验证脚本。

3. 定时任务 —— 以往使用 cron 编写的定时任务可以改用 AWS Lambda 实现，很明显的好处是任务不执行的时候完全不收费。

4. 瘦容器 —— 因为 AWS Lambda 本身基于 Docker 容器实现，Lambda 方法跑在 Amazon Linux AMI 中，虽然官方支持的编程语言只有 NodeJS、Java、Python，但其实可以用 NodeJS 的 shim 运行大部分能在 Linux 下运行的程序。以至于有人用这个特性做了 [LambCI](https://github.com/lambci/lambci) 这种脑洞大开的 Serverless CI 服务。

5. 无人运维 —— Serverless 的核心优势就是不需要管理服务器，自伸缩的特性如果用传统方案解决会相对复杂很多。如果你需要一个服务为你跑好几年，期间完全不需要担心它的服务器运行情况，Serverless 会是最好的选择。

当然 Serverless 的也不尽是优点，它也有一些局限性或者说是相对传统架构的短板，不过也仅仅体现在现在的 AWS Lambda 上，相信以后会有改善：

1. 生命周期短 —— 一个 Lambda 最多只能跑 5 分钟，所以想用它跑 ffmpeg/mencode 来处理高清视频的话得想周到，可以考虑切片分断处理，不过那就复杂了。

2. Linux only —— 想做一个 macOS 的 CI 服务目前是不可能的。

3. 语言限制 —— 除了 NodeJS、Java、Python 以外，其它程序都只能通过 shim 来运行，调试相对麻烦。

4. 部署操作繁多 —— 针对第这个问题，已经有不少解决方案，如 [Sparta](http://gosparta.io)、[Apex](http://apex.run)、[Webtask](https://webtask.io)、[Serverless](https://serverless.com) 都在简化 Lambda 部署的工作上给出了各种答案。

由于上面提到的这些特性，我觉得 Serverless 这个概念非常适合 App 开发人员深入研究。它就像是一个永远不会倒闭的 Parse Cloud Function (哇，顺路吐槽 Facebook 了~~)，或者说是你身边 24 小时在线的运维朋友，亦或说是你的开机最快的一台电脑(实测用 AWS Lambda 运行一个 Swift 写的 Hello World 只要 16ms)。

于是，我开始借肋 Apex，在 AWS Lambda 上用 Go 来写 Serverless 程序。为什么没用 Swift 呢？因为服务端的 Swift 库实在太少了，缺的轮子太多。而且 Swift 依赖相对较多，一个 HelloWorld.swift 要在 AWS Lambda 里跑起来差不多需要 16MB 的空间，同样的 Go 只要 2MB，而且 Go 的工具链非常强大。如果有兴趣在 AWS Lambda 里运行 Swift 程序，可以参考[这篇文章](https://medium.com/@gigq/using-swift-in-aws-lambda-6e2a67a27e03#.qwdpdzby9)。
