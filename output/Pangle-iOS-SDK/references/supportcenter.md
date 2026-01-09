
# iOS直播拉流

开发测试/iOS集成文档/iOS直播拉流
iOS直播拉流
最近更新 2025-08-14 20:34:01
一、背景&收益

直播拉流类型(实时的电商直播流，即实时正在播放的直播预览流，是闭环预算中的一种特殊广告素材形式)预算在整体闭环预算中占比30%，支持后预计闭环预算消耗+10%-20%。目前穿山甲已支持IOS端直播拉流能力，开发者可按照下述流程完成接入，把握预算。

二、SDK接入

V6610及以上：请参考本章节接入

V6410-V6609：参考https://bytedance.larkoffice.com/docx/OLPIdHCubooQMOxE7lTcNNuFn8b接入

Cocoapods接入
方式一：动态库接入【推荐】
复制
### 请根据实际场景选择其中一种方式接入

# 6610+版本穿山甲默认集成直播拉流
pod 'Ads-CN', '6.8.x.x'
 
# 使用的Gromore聚合能力
# V6610-V70xx：在原来CSJMediation的基础上增加BUAdLive-Framework的subspec
pod 'Ads-CN', '6.8.x.x', :subspecs => ['CSJMediation','BUAdSDK','BUAdLive-Framework']
# V7100+ ：默认接入直播拉流的配置能力
pod 'Ads-CN', '7.1.x.x', :subspecs => ['CSJMediation','BUAdSDK']
方式二：静态库接入

静态库接入会引入FFMpeg组件，确保宿主App内部不包含任何FFMpeg组件，否则可能会在编译或者运行阶段发生冲突或者崩溃。若静态库接入有ffmpeg冲突，强烈建议通过方式一：动态库进行接入。

复制
### 请根据实际场景选择其中一种方式接入

# 6610+版本穿山甲默认集成直播拉流
pod 'Ads-CN', '6.8.x.x', :subspecs=> ['BUAdSDK', 'BUAdLive-Lib']

# 如果使用的Gromore聚合能力
# V7100+ ：默认接入直播拉流的配置能力
pod 'Ads-CN', '7.1.x.x', :subspecs => ['CSJMediation','BUAdSDK']
# V6610-V70xx：在原来CSJMediation的基础上增加BUAdLive-Lib的subspec
pod 'Ads-CN', '6.8.x.x', :subspecs => ['CSJMediation','BUAdSDK','BUAdLive-Lib']


# 如此前引入了Onekit，升级时请移除
# pod 'OneKit', '1.4.2', :subspecs => ['BaseKit', 'Reachability', 'ByteDanceKit/Foundation'], :source => 'https://github.com/volcengine/volcengine-specs'
手动接入
将穿山甲平台下载的BUAdLive拖入工程
下载TTSDK依赖包解压，将以下内容全部拖入工程。
TTSDK.zip
TTSDK.zip
65.85 MB
设置Embed&Sign
ffmpeg_dashdec.framework
ttboringssl.framework
ttcrypto.framework
TTFFmpeg.framework

           以下是完整的依赖库列表和设置方式

添加系统动态库库依赖

添加如下动态库

复制
CoreMotion,
CoreMedia,
MetalKit,
OpenAL,
VideoToolBox,
AudioToolBox,
AVFoundation,
SystemConfiguration,
MetalPerformanceShaders,
CoreTelephony

标准库依赖

复制
stdc++,
z, 
xml2,
iconv,
三、自定义版本
若媒体项目没有接入TTSDK，直接按照第二部分默认接入方式即可，不需要自定义版本；
若本身有依赖TTSDK或TTSDKFramework，则需要参考本模块进行接入

什么情况下需要自定义版本？

此前接入过穿山甲直播拉流，升级穿山甲SDK不想修改依赖关系，或者不想修改TTSDK版本。（升级仍需要删除Onekit依赖）
项目中已经集成TTSDK / TTSDKFramework，或本身的业务依赖TTSDK（如直播功能）。
此前集成其他SDK已经依赖的TTSDK或TTSDKFramework（如穿山甲-内容输出DJXSdk）
需要某个特定的版本，清楚的理解TTSDK的依赖关系和版本信息。

注意：项目中只能存在一个TTSDK或者TTSDKFramework，如果需要自定义接入，请仔细检查项目依赖关系。

自定义动态库版本
复制
# 若没有TTSDK相关冲突，直接默认按pod 'Ads-CN', '6.x.x.x'接入即可
pod 'Ads-CN', '6.8.x.x', :subspecs => ['BUAdSDK', 'BUAdLive']

#若使用的Gromore聚合能力
# V7100+ ：默认接入直播拉流的配置能力
pod 'Ads-CN', '7.1.x.x', :subspecs => ['CSJMediation-Only','BUAdSDK','BUAdLive']
# V6610-V70xx：在原来CSJMediation的基础上增加BUAdLive-Framework的subspec
pod 'Ads-CN', '6.8.x.x', :subspecs => ['CSJMediation','BUAdSDK','BUAdLive']

# TTSDK动态库版本，建议使用文档中的版本
pod 'TTSDKFramework', '1.46.2.7-premium', :subspecs => ['LivePull-Lite'], :source => 'https://github.com/volcengine/volcengine-specs'
自定义静态库版本

如果您不确定是否需要，可以参考“二、SDK接入”完成接入。

静态库会引入FFMpeg组件，确保宿主App内部不包含任何FFMpeg组件，否则会在编译或者运行阶段发生冲突或者崩溃。

复制
# 若没有TTSDK相关冲突，直接默认按pod 'Ads-CN', '6.8.x.x'接入即可
pod 'Ads-CN', '6.8.x.x', :subspecs => ['BUAdSDK', 'BUAdLive-Lib']

# 若使用的Gromore聚合能力
# V7100+ ：默认接入直播拉流的配置能力
pod 'Ads-CN', '7.1.x.x', :subspecs => ['CSJMediation-Only','BUAdSDK','BUAdLive-Lib']
# V6610-V70xx：在原来CSJMediation的基础上增加BUAdLive-Lib的subspec
pod 'Ads-CN', '6.8.x.x', :subspecs => ['CSJMediation','BUAdSDK','BUAdLive-Lib']


# TTSDK静态库版本,建议使用文档中的版本
pod 'TTSDK', '1.46.2.7-premium', :subspecs => ['LivePull-Lite'], :source => 'https://github.com/volcengine/volcengine-specs'
四、广告样式适配

直播拉流是闭环预算中的一种特殊广告素材形式，imagemode=166。①自渲染广告类型需要根据imagemode=166区分是否为直播拉流广告，若不适配该素材类型，则会缺失直播拉流预算(取主播名称/头像等字段做直播样式展示，字段获取需联系对接同学开通权限)；②模版广告类型开发者满足上述接入方式，则不需要额外处理；

五、联调测试

集成完成后，开发者可以集成测试工具后预览测试直播拉流类广告，有问题可即时向对接同学反馈

注：使用测试工具前，需要先找穿山甲技术支持申请权限，加白后才能生效




六、FAQ
直播拉流支持的广告位范围？
激励视频：模板渲染
插全屏：模板渲染
信息流：自渲染、模板渲染
Draw：自渲染、模板渲染 [6133,6200) 、[6206 版本以上）
测试看不到直播拉流广告，CheckList：
是否正确集成了穿山甲直播拉流SDK？参考“二、SDK接入”
若想要定向刷到直播拉流素材，请参考\"四、联调测试\"\n如上述均完成，抓取get_ads和settings接口返回，联系TS和RD同学排查：
运行过程报错？直播播放异常？Crash？
检项目木是否包含ffmpeg，只要应用中包含，无论是否使用，就需要使用动态库方式集成。常见的ffmpeg库：ijkplayer、TTSDK等三方通用播放器库。
如何区分普通广告和直播拉流广告？
通过BUNativeAds.data.imageMode = 166区分，如自渲染等业务需求，可以据此判断从而进行UI、逻辑的定制。若媒体开通了自定义播放器的能力，请在判断imagemode为166后，针对166素材选择SDK默认播放器展示，否则直播拉流无法播放
直播拉流广告和闭环电商广告saas链路的区别？
直播拉流广告是闭环电商广告的一种，还有其他形式广告。但是iOS直播拉流广告不包含闭环saas环境，点击会跳转抖音而非在app内完成全链路。










本篇目录
一、背景&收益
二、SDK接入
Cocoapods接入
方式一：动态库接入【推荐】
方式二：静态库接入
手动接入
三、自定义版本
自定义动态库版本
自定义静态库版本
四、广告样式适配
五、联调测试
六、FAQ
评价此篇文档
有帮助
无帮助

---
Source: https://www.csjplatform.com/supportcenter/28585


# SDK版本发布记录

穿山甲iOS端SDK版本发布记录说明文档，主要说明SDK版本的发布时间、版本号以及版本迭代的功能点，帮助开发者了解版本更迭状态，选择需要的版本进行功能开发。
SDK历史版本下载地址：https://www.csjplatform.com/union/media/union/download/log


版本号

发布时间

版本说明

V5.9.0.0

2024-05-20

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode15.3

V5.8.0.0

2024-04-12

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode15.3

V5.7.0.2

2024-03-12

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode15.2

V5.7.0.0

2024-02-22

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode15.2

V5.6.0.0

2024-01-16

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode15.1

V5.5.0.0

2023-11-23

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode15.0

V5.4.0.0

2023-09-28

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode15.0

V5.3.0.0

2023-08-23

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode14.3

V5.2.0.0

2023-06-28

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode14.3

V5.1.0.0

2023-04-26

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode14.2

V5.0.0.0

2023-03-23

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode14.2

V4.9.0.0

2023-01-17

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode14.1

V4.8.0.0

2022-11-23

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode14.1

V4.7.0.0

2022-09-29

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode14.0

V4.6.0.0

2022-08-25

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode13.4

V4.5.0.0

2022-07-25

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode13.4

V4.4.0.0

2022-06-23

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode13.3

V4.3.0.0

2022-05-24

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode13.3

V4.2.0.0

2022-04-22

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode13.3

V4.1.0.0

2022-03-24

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode13.2

V4.0.0.0

2022-02-23

【功能新增】
1.GroMore新增支持服务端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
2.GroMore新增客户端bidding的adapter，包括：Lark Bidding、穿山甲Bidding、Mintegral Bidding、快手Bidding、百度Bidding、腾讯优量汇Bidding
3.GroMore新增穿山甲标准瀑布流adapter
4.GroMore新增Mintegral 标准瀑布流adapter
5.GroMore新增快手标准瀑布流adapter
6.GroMore新增百度标准瀑布流adapter
7.GroMore新增腾讯优量汇标准瀑布流adapter
8.GroMore新增Sigmob标准瀑布流adapter
9.GroMore新增游可赢标准瀑布流adapter
10.GroMore新增TradPlus 标准瀑布流adapter
11.GroMore新增快看 标准瀑布流adapter
12.GroMore新增GDTMob 标准瀑布流adapter
【适配】
1. 支持 Xcode13.2

---
Source: https://www.csjplatform.com/supportcenter/5373


# SDK初始化及全局配置

开发测试/iOS集成文档/SDK初始化及全局配置
SDK初始化及全局配置
最近更新 2025-09-17 13:24:00
注意事项
BUAdSDKManager 类是整个 SDK 设置的入口和接口，可以设置 SDK 的一些全局信息，提供类方法获取设置结果。
setIsPaidApp:和setUserKeywords:须征得用户同意才可传入。
任意广告类型均不支持中途更改代理，中途更改代理会导致接收不到广告相关回调，如若存在中途更改代理场景，需自行处理相关逻辑，确保广告相关回调正常执行。
接入过程中，强烈建议双端分别使用各自的应用ID进行测试，不要混用，否则可能会影响收益。
接口说明

目前接口提供以下几个类方法：

复制
@property (nonatomic, copy, readonly, class) NSString *SDKVersion;

//同时集成穿山甲SDK和Pangle SDK需要设置此接口，5205及以上版本该接口下线，开发者不需要设置
@property (nonatomic, assign) BUAdSDKTerritory territory;

//应用ID
@property (nonatomic, copy) NSString *appID;

//应用的唯一标识，可选参数
@property (nonatomic, copy) NSString *secretKey;

//配置开发模式。默认BUAdSDKLogLevelNone
@property (nonatomic, assign) BUAdSDKLogLevel logLevel;

//用户年龄设置
@property (nonatomic, assign) BUAdSDKAgeGroup ageGroup;

//用户的其他信息
@property (nonatomic, copy) NSString *userExtData;

//默认 BUOfflineTypeWebview
@property (nonatomic, assign) BUOfflineType webViewOfflineType;

//打印调试日志，0关闭调试日志； 1打开调试日志
@property (nonatomic, strong) NSNumber *debugLog;

//主题模式
@property (nonatomic, strong) NSNumber *themeStatus;

//自定义设置用户AB实验 
@property (nonatomic, strong) NSArray<NSNumber *> *abvids;

//用户自定义设置AB SDK版本
@property (atomic, copy) NSString *abSDKVersion;

//自定义idfa
@property (nonatomic, copy) NSString *customIdfa;

//是否允许SDK在播放音频时修改AVAudioSession的类别和选项，默认为NO。SDK设置的类别是AVAudioSessionCategoryAmbient，开发者可以自行设置为AVAudioSessionCategoryOptionDuckOthers
@property (nonatomic, assign) BOOL allowModifyAudioSessionSetting;

// BUAudioSessionSettingType_Mix = 3  可以跟其他音乐软件共存声音 
// BUAudioSessionSettingType_Default = 0 不支持混音
// 6204及以上版本支持
@property (nonatomic, assign) BUAudioSessionSettingType audioSessionSetType;

//unity开发者，设置为YES
@property (nonatomic, assign) BOOL unityDeveloper;

//隐私数据设置，建议不设置该参数
@property (nonatomic, strong) id<BUAdSDKPrivacyProvider> privacyProvider;

//logo图标设置
@property (nonatomic, strong, nullable) UIImage *appLogoImage;


//调试模式设置为NO
@property (nonatomic, assign) BOOL SDKDEBUG;

//隐私数据设置  
@property (nonatomic, strong, nullable) id<BUAdSDKPrivacyProvider> privacyProvider;


//3600版本开始，新增SDK初始化状态回调
[BUAdSDKManager startWithAsyncCompletionHandler:^(BOOL success, NSError *error) {
   if (success) { /// }
     }];

使用详情

强烈建议在用户同意隐私协议后再进行SDK的初始化操作，其中seAppID设置是必须的

//3500及以上版本SDK初始化说明

复制
BUAdSDKConfiguration *configuration = [BUAdSDKConfiguration configuration];
    configuration.territory = BUAdSDKTerritory_CN;
    configuration.appID = @"xxxxxx";//除appid外，其他参数配置按照项目实际需求配置即可。
   [BUAdSDKManager startWithAsyncCompletionHandler:^(BOOL success, NSError *error) {
        if (success) {
           dispatch_async(dispatch_get_main_queue(), ^{
         //请求广告逻辑处理
           
           });
       }
   }]; 

//3500以下版本SDK初始化说明

复制
[BUAdSDKManager setAppID:@"xxxxxx"];//57xx版本正式下线

更多使用方式可以参见 SDK Demo 工程

日志输出说明
复制
// 日志输出枚举值详情
typedef NS_ENUM(NSInteger, BUAdSDKLogLevel) {
    BUAdSDKLogLevelNone,
    BUAdSDKLogLevelError,
    BUAdSDKLogLevelWarning,
    BUAdSDKLogLevelInfo,
    BUAdSDKLogLevelDebug,
    BUAdSDKLogLevelVerbose,
};

3500及以上版本说明

复制
BUAdSDKConfiguration *configuration = [BUAdSDKConfiguration configuration];
configuration.logLevel = BUAdSDKLogLevelDebug;

3500以下版本说明

复制
/// Whether to open log. default is none.
[BUAdSDKManager setLoglevel:BUAdSDKLogLevelDebug];




IDFA使用说明

7200及以上版本说明

复制
//复用BUAdSDKPrivacyProvider中的方法:-(nullable NSDictionary*)privacyConfig
- (NSDictionary *)privacyConfig {
    NSMutableDictionary *privacy = [NSMutableDictionary dictionary];
    // motion_info表示是否允许传感器采集数据，默认不传时允许采集
    // \"0\": 不允许
    // \"1\": 允许
    // 其他值或不实现该协议方法认为允许采集
   [privacy setObject:@"1" forKey:@"motion_info"];

    // forbidden_idfa表示是否禁止idfa采集，\"1\"表示禁止
   [privacy setObject:@"0" forKey:@"forbidden_idfa"];

    return [privacy copy];
}




自定义IDFA说明

3500及以上版本说明

复制
BUAdSDKConfiguration *configuration = [BUAdSDKConfiguration configuration];
configuration.customIdfa = @"12345678-1234-1234-1234-123456789012"

3500以下版本说明

复制
[BUAdSDKManager setCustomIDFA:@"12345678-1234-1234-1234-123456789012"];
广告跳过设置

广告接口中的所有rootViewController均为必传项，用来处理广告跳转。SDK里所有的跳转均采用present的方式，请确保传入的rootViewController不能为空且没有present其他的控制器，否则会出现presentedViewController已经存在而导致present失败。

隐私协议
BUAdSDKConfiguration 新增 privacyProvider 参数，用来让开发者控制某些隐私字段的启用或传值。这个字段只对国内有效，国外设置无反应。
复制
/// You can use this property to set whether the SDK can use certain privacy data, or pass these privacy data yourself.
/// If this property is not set, the SDK will execute the default privacy data usage logic.
/// Note: This property is only valid for the CN environment. Unless you know what you need, it is recommended not to assign a value to this property.
@property (nonatomic, strong) id<BUAdSDKPrivacyProvider> privacyProvider;


/// Provide a protocol for custom private data
/// Only valid in the CN environment
@protocol BUAdSDKPrivacyProvider <NSObject data-tomark-pass >

@optional

/// Specify whether to allow the SDK to use location data
- (BOOL)canUseLocation;
/// Return a latitude value
- (CLLocationDegrees)latitude;
/// Return a longitude value
- (CLLocationDegrees)longitude;
夜间模式

3600版本开始，穿山甲支持开发者在使用模板渲染信息流&插屏&Banner广告时设置夜间模式的功能

复制
BUAdSDKManager 或 BUAdSDKConfiguration类中

  // 枚举值类型
    typedefNS_ENUM(NSInteger, BUAdSDKThemeStatus) {
       BUAdSDKThemeStatus_Normal = 0,   // 正常模式
       BUAdSDKThemeStatus_Night= 1,     // 夜间模式
      };

      // 若开发者未设置，那么默认值是 Normal， 如果开发者设置的为非法值，会强行修改为  Normal
       + (void)setThemeStatus:(BUAdSDKThemeStatus)themeStatus;

  // 获取当前主题类型
       + (BUAdSDKThemeStatus)themeStatus;
混音设置
复制
   BUAdSDKConfiguration *configuration = [BUAdSDKConfiguration configuration];
    configuration.appID = [BUDAdManager appKey];
    configuration.privacyProvider = [[BUDPrivacyProvider alloc] init];
    configuration.appLogoImage = [UIImage imageNamed:@"AppIcon"];
    configuration.debugLog = @(1);
    configuration.allowModifyAudioSessionSetting = YES;
    //设置这个可以让后台音乐跟广告声音共存
    configuration.audioSessionSetType = BUAudioSessionSettingType_Mix; //可以跟其他音乐软件共存声音




本篇目录
注意事项
接口说明
使用详情
日志输出说明
IDFA使用说明
自定义IDFA说明
广告跳过设置
隐私协议
夜间模式
混音设置
评价此篇文档
有帮助
无帮助

---
Source: https://www.csjplatform.com/supportcenter/5378


# SDK集成

开发测试/iOS集成文档/SDK集成
SDK集成
最近更新 2025-12-25 18:00:41




注意 ：该接入文档仅包含变现SDK接入的基础功能实现介绍，完整的接入文档介绍建议您登录穿山甲媒体平台账号后进行查看。

我们建议您使用 CocoaPods 更轻松地管理 Xcode 项目的库依赖项，而不是直接下载并安装 SDK。

版本

	

MD5值




v7.3.0.7

	

66021eacbab6f252c1e0c2ef928f8d70

方法一

在podfile文件中加入以下代码即可接入成功。

复制
pod  'Ads-CN'

注意：更多关于pod方式的接入请参考github

方法二

获取 framework 文件 (平台接入模块->SDK下载与接入文档)后重新将依赖的framework文件拖入工程即可。

    依赖的framework说明如下: 

BUAdSDK.framework 抽象的接口逻辑层
CSJAdSDK.bundle  资源bundle

升级SDK必须同时更新framework和bundle文件，否则可能出现部分页面无法展示的问题。







拖入完请确保Copy Bundle Resources中有CSJAdSDK.bundle，否则可能出现icon图片加载不出来的情况。

同时集成CSJ和Pangle SDK
复制
pod 'Ads-CN' , 
pod 'Ads-Global'
注意事项
v4500 接口层进行了抽象处理, 继承一个抽象的接口并不能有效进行功能扩展. 反而在业务逻辑和系统稳定性上产生隐患, 所以SDK禁止媒体继承抽象接口类, 若存在必要的功能扩展,可通过封装实现；
接口层View视图类进行抽象处理,实例化的为具体实现视图类, 接口层View视图类的category将是不合适的,若存在必要的功能扩展,可通过封装实现；
使用临时变量加载是错误的操作. 在SDK修复了内部内存泄漏后使用临时变量加载样式将不能完成样式加载；
复制
// 这是正确的操作示例 
- (void)loadAdData {
    BUNativeAdsManager *nad = [BUNativeAdsManager new];
    BUAdSlot *slot = [[BUAdSlot alloc] init];
    slot.ID = self.viewModel.slotID;
    slot.AdType = BUAdSlotAdTypeFeed;
    slot.position = BUAdSlotPositionTop;
    slot.imgSize = [BUSize sizeBy:BUProposalSize_Feed690_388]; // 690*388
    nad.adslot = slot;
    nad.delegate = self;
   [nad loadAdDataWithCount:1];
    self.nad = nad;
}
复制
 //  这是错误的操作. 使用临时变量去加载样式
    - (void)loadAdData {
    BUNativeAdsManager *nad = [BUNativeAdsManager new];
    BUAdSlot *slot = [[BUAdSlot alloc] init];
    slot.ID = self.viewModel.slotID;
    slot.AdType = BUAdSlotAdTypeFeed;
    slot.position = BUAdSlotPositionTop;
    slot.imgSize = [BUSize sizeBy:BUProposalSize_Feed690_388]; // 690*388
    nad.adslot = slot;
    nad.delegate = self;
   [nad loadAdDataWithCount:1];
}




本篇目录
方法一
方法二
同时集成CSJ和Pangle SDK
注意事项
评价此篇文档
有帮助
无帮助

---
Source: https://www.csjplatform.com/supportcenter/5376


# Xcode配置

开发测试/iOS集成文档/Xcode配置
Xcode配置
最近更新 2025-12-19 10:09:21
运行环境配置
支持系统：69以下版本支持iOS 11.0及以上、70以上版本支持iOS 13.0及以上
SDK编译环境 Xcode 14.1及以上;
支持架构： arm64,x86_64
添加权限
工程plist文件设置，点击右边的information Property List后边的 "+" 展开添加 App Transport Security Settings，先点击左侧展开箭头，再点右侧加号，Allow Arbitrary Loads 选项自动加入，修改值为 YES。 SDK API 已经全部支持HTTPS，但是客户素材存在非HTTPS情况。
复制
<key>NSAppTransportSecurity</key>
    <dict>
         <key>NSAllowsArbitraryLoads</key>
         <true/>
    </dict>
工程plist文件设置，点击右边的information Property List后边的 "+" 展开添加NSMotionUsageDescription，先点击左侧展开箭头，再点右侧加号，NSMotionUsageDescription添加后，设置value值。
注意：CSJ SDK实际并不需要配置NSMotionUsageDescription权限。
This key is required if your app uses APIs that access the device’s motion data, including CMSensorRecorder, CMPedometer, CMMotionActivityManager, and CMMovementDisorderManager. If you don’t include this key, your app will crash when it attempts to access motion data.详见
复制
<key>NSMotionUsageDescription</key>
<string>This app needs to be able to access your motion use</string>




具体操作如图

Build Settings中Other Linker Flags 增加参数-ObjC、 -l"c++"、 -l"c++abi" 、-l"sqlite3"、-l"z" ，SDK同时支持-all_load，具体操作如图

添加依赖库

工程需要在TARGETS -> Build Phases中找到Link Binary With Libraries，点击“+”，依次添加下列依赖库

CoreML.framework （6.3.0.0及以上版本新增）
CoreHaptics.framework（6.9.0.0及以上版本新增）
Accelerate.framework
AdSupport.framework
AppTrackingTransparency
AudioToolbox.framework
AVFoundation.framework
CoreGraphics.framework
CoreImage.framework
CoreLocation.framework
CoreMedia.framework
CoreMotion.framework
CoreTelephony.framework
CoreText.framework
ImageIO.framework
JavaScriptCore.framework
MapKit.framework
MediaPlayer.framework
MobileCoreServices.framework
QuartzCore.framework
Security.framework
StoreKit.framework
SystemConfiguration.framework
UIKit.framework
WebKit.framework
DeviceCheck.framework依赖（4.8.0.3及以上版本新增）
libbz2.tbd
libc++.tbd
libiconv.tbd
libresolv.9.tbd
libsqlite3.tbd
libxml2.tbd
libz.tbd
libc++abi.tbd

具体操作如图所示

iOS17隐私策略适配说明
适配说明
在6012及以上的版本，我们已新增 PrivacyInfo.xcprivacy 文件，位于静态库产物的CSJAdSDK.bundle中。
您可以用cocoapods集成，手动集成或SPM集成，无论使用哪种方式，都可以在xcode项目的CSJAdSDK.bundle->PrivacyInfo.xcprivacy找到，请注意将PrivacyInfo.xcprivacy 拷贝进您的代码工程里。
如果您的App本身包含PrivacyInfo.xcprivacy文件，请将穿山甲的PrivacyInfo.xcprivacy中的条款补全到自身的PrivacyInfo.xcprivacy中，具体补全方式如下：
使用source code方式添加：
Xcode 中使用 Source Code方式打开 app 项目下的 PrivacyInfo.xcprivacy。复制穿山甲的 PrivacyInfo.xcprivacy中的条目，注意不要重复添加或错行。
使用 Property List 的方式添加：
在 Xcode 中双击打开 PrivacyInfo.xcprivacy 文件，在其中点击+，Xcode会提示可选的条款和可设置项，按照需求进行增补即可。
如果您的项目同时集成了多个包含PrivacyInfo.xcprivacy的SDK，建议您将所有SDK的条款补充到您自身App的PrivacyInfo.xcprivacy中。在补充时，对于同一个API的声明和原因解释，无需重复添加。
Ads-CN的PrivacyInfo.xcprivacy
复制
<?xml version=\"1.0\" encoding=\"UTF-8\"?>
<!DOCTYPE plist PUBLIC \"-//Apple//DTD PLIST 1.0//EN\" \"http://www.apple.com/DTDs/PropertyList-1.0.dtd\">
<plist version=\"1.0\"> 
<dict>
        <key>NSPrivacyCollectedDataTypes</key>
        <array/>
        <key>NSPrivacyAccessedAPITypes</key>
        <array>
                <dict>
                        <key>NSPrivacyAccessedAPIType</key>
                        <string>NSPrivacyAccessedAPICategoryFileTimestamp</string>
                        <key>NSPrivacyAccessedAPITypeReasons</key>
                        <array>
                                <string>C617.1</string>
                        </array>
                </dict>
                <dict>
                        <key>NSPrivacyAccessedAPIType</key>
                        <string>NSPrivacyAccessedAPICategorySystemBootTime</string>
                        <key>NSPrivacyAccessedAPITypeReasons</key>
                        <array>
                                <string>35F9.1</string>
                        </array>
                </dict>
                <dict>
                        <key>NSPrivacyAccessedAPIType</key>
                        <string>NSPrivacyAccessedAPICategoryDiskSpace</string>
                        <key>NSPrivacyAccessedAPITypeReasons</key>
                        <array>
                                <string>7D9E.1</string>
                                <string>E174.1</string>
                        </array>
                </dict>
                <dict>
                        <key>NSPrivacyAccessedAPIType</key>
                        <string>NSPrivacyAccessedAPICategoryUserDefaults</string>
                        <key>NSPrivacyAccessedAPITypeReasons</key>
                        <array>
                                <string>CA92.1</string>
                        </array>
                </dict>
        </array>
</dict>
</plist>
iOS14 AppTrackingTransparency
SKAdNetwork（SKAN） 是 Apple 的归因解决方案，可帮助客户在保持用户隐私的同时衡量变现活动。 使用 Apple 的 SKAdNetwork 后，即使 IDFA 不可用，流量网络也可以正确获得应用安装的归因结果。 访问 https://developer.apple.com/documentation/storekit/skadnetwork 了解更多信息。 为了流量转化的归因，所有开发者须设置SKAdNetwork方案的穿山甲SKAdNetwork id。
App Tracking Transparency (ATT) 适用于请求用户授权，访问与应用相关的数据以跟踪用户或设备。 访问 https://developer.apple.com/documentation/apptrackingtransparency了解更多信息。 目前苹果要求在iOS 14.5及以上的版本中必须在弹窗取得用户同意后，才可以追踪用户。对其他版本暂无明确要求，开发者应根据需要配置弹窗 。
Checklist

1 升级穿山甲 iOS SDK 3.5.1.1 及以上版本，穿山甲提供了 iOS 14.5 与 SKAdNetwork 支持

2 将穿山甲的 SKAdNetwork ID 添加到 info.plist 中，以保证 SKAdNetwork 的正确运行

复制
<key>SKAdNetworkItems</key>
 <array>
  <dict>
   <key>SKAdNetworkIdentifier</key>
   // SKAdNetwork方案的穿山甲SKAdNetwork id
   <string>238da6jt44.skadnetwork</string>
  </dict>
  <dict>
   <key>SKAdNetworkIdentifier</key>
   // SKAdNetwork方案的穿山甲SKAdNetwork id
   <string>x2jnk7ly8j.skadnetwork</string>
  </dict>
  <dict>
   <key>SKAdNetworkIdentifier</key>
   // SKAdNetwork方案的穿山甲SKAdNetwork id
   <string>22mmun2rn5.skadnetwork</string>
  </dict>
 </array>

3 支持苹果 ATT：从 iOS 14.5 开始，若开发者设置 App Tracking Transparency 向用户申请跟踪授权，在用户授权之前IDFA 将不可用。 如果用户拒绝此请求，应用获取到的 IDFA 将自动清零，可能会导致您的变现收入的降低

要获取 App Tracking Transparency 权限，请更新您的 Info.plist，添加 NSUserTrackingUsageDescription 字段和自定义文案描述。代码示例：
复制
<key>NSUserTrackingUsageDescription</key>
<string>该标识符将用于向您投放个性化样式</string>
向用户申请权限时，请调用 requestTrackingAuthorizationWithCompletionHandler:方法。我们建议您申请权限后再请求，以便穿山甲准确的获得用户授权状态。

Swift 代码示例

复制
import AppTrackingTransparency
import AdSupport
func requestIDFA() {
 ATTrackingManager.requestTrackingAuthorization(completionHandler: { status in
  // Tracking authorization completed. Start loading ads here.
  // loadAd()
  })
}

Objective-C 代码示例

复制
#import <AppTrackingTransparency/AppTrackingTransparency.h>
#import <AdSupport/AdSupport.h>
- (void)requestIDFA {
  [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {
   // Tracking authorization completed. Start loading ads here.
   // [self loadAd];
  }];
}
iOS本地化/国际化配置

注意 : 开发者必须在这里设置所支持的语言,否则会有语言显示的问题.

例如 : 支持中文 添加 Chinese







本篇目录
运行环境配置
添加权限
iOS17隐私策略适配说明
适配说明
Ads-CN的PrivacyInfo.xcprivacy
iOS14 AppTrackingTransparency
Checklist
iOS本地化/国际化配置
评价此篇文档
有帮助
无帮助

---
Source: https://www.csjplatform.com/supportcenter/5377


# 开屏广告

开发测试/iOS集成文档/开屏广告
开屏广告
最近更新 2025-10-14 14:12:12
简介

开屏广告主要是 APP 启动时展示的全屏广告视图，开发只要按照接入标准就能够展示设计好的视图。展示完毕后自动关闭并进入应用的主界面。

开屏广告使用BUSplashAd对象管理开屏广告所有业务；接入方式上，使用BUSplashAd对象调用loadAdData方法请求广告，调用show方法展示广告，通过设置BUSplashAdDelegate代理，获取广告加载、 渲染、点击、关闭、跳转等回调。

开屏样式接口概览
创建广告对象、请求广告、展示广告

广告对象创建时必须传入代码位ID：

复制
//保证每次请求的广告对象为新的广告对象，避免重复使用广告同一个对象进行广告请求
_splashAd = [[BUSplashAd alloc] initWithSlotID:slotID adSize:size];
// 设置开屏广告代理
_splashAd.delegate = self;
// 加载广告
[_splashAd loadAdData];

广告物料、素材加载成功后，会回调splashAdLoadSuccess：方法，在这里调用show方法展示广告：

接入方式一（强烈推荐）：

使用App主window对象的根视图控制器展示，不需要管理开屏控制器展示，但6900及以上版本点击广告，SDK将不再主动移除视图，开发者可以根据业务场景自行处理，点击跳过按钮或倒计时结束时仍由SDK进行移除。

示例代码（完整代码可参考官方demo中BUDSplashViewController 类）

展示广告

复制
- (void)splashAdLoadSuccess:(BUSplashAd *)splashAd {
    // 使用应用keyWindow的rootViewController（接入简单，推荐）
    UIWindow *keyWindow = [UIApplication sharedApplication].keyWindow;   
   [splashAd showSplashViewInRootViewController:keyWindow.rootViewController];
}

移除广告

开发者根据业务场景需要，使用[splashAd removeSplashView]方式移除广告视图




接入方式二（不推荐）：

开发者如果使用新创建的视图控制器接入，需要自行管理viewController的展示和关闭，同时需要处理广告点击时的视图移除逻辑，点击跳过按钮或倒计时结束仍由SDK进行移除。

示例代码（完整代码可参考官方demo中BUDSplashViewController 类）

展示广告

复制
@implementation SplashViewController

#pragma mark - BUSplashAdDelegate
- (void)splashAdLoadSuccess:(BUSplashAd *)splashAd {
   [splashAd showSplashViewInRootViewController:self];
}

- (void)splashAdRenderSuccess:(BUSplashAd *)splashAd {
    // 渲染成功再展示视图控制器
    UIWindow *keyWindow = [[[UIApplication sharedApplication] delegate] window];
   [keyWindow.rootViewController addChildViewController:self];
   [keyWindow.rootViewController.view addSubview:self.view];
} 

@end

移除广告

开发者可在开屏相关代理回调中关闭当前视图控制器，示例代码如下：

复制
@implementation SplashViewController

#pragma mark - BUSplashAdDelegate
// 广告物料加载失败回调
- (void)splashAdLoadFail:(BUSplashAd *)splashAd error:(BUAdError * _Nullable)error {
   [self p_dismiss];
}

// 渲染失败回调
- (void)splashAdRenderFail:(BUSplashAd *)splashAd error:(BUAdError * _Nullable)error {
   [self p_dismiss];
}

// 开屏视图关闭回调
- (void)splashAdDidClose:(BUSplashAd * )splashAd closeType:(BUSplashAdCloseType)closeType {
  //注意！！！不要在这个回调中关闭控制器，会导致中间页无法跳转
  // [self p_dismiss]; 
}

// 开屏视图控制器关闭回调
- (void)splashAdViewControllerDidClose:(BUSplashAd *)splashAd {
   [self p_dismiss];
}

#pragma mark - private
- (void)p_dismiss {
   [self.view removeFromSuperview];
   [self removeFromParentViewController];
}

// 6900及以上版本新增

//外跳型广告：点击广告时，落地页、AppStore页类型的广告展示依赖控制器，该时机不推荐做移除操作，但可以做点击的标记用于移除的判断

- (void)splashAdDidClick:(BUSplashAd *)splashAd {
    // 该方式接入，如果是落地页、AppStore页类型的广告，展示依赖控制器
    // 对于deeplink/ulink类型的广告不依赖控制器可以根据类似didClicked标记位直接移除
    // 开发者需要根据具体情况移除控制器
    self.didClicked = YES;
}

// 外跳型广告：切换前后台
// 监听后台通知 点击标记
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(didEnterBackgroundNotification) name:UIApplicationDidEnterBackgroundNotification object:nil];

- (void)didEnterBackgroundNotification {
    if (self.didClicked) {
       [self p_dismiss]
   }
}

// 中间页类型广告     
// 关闭落地页或AppStore页面回调
- (void)splashDidCloseOtherController:(nonnull BUSplashAd *)splashAd interactionType:(BUInteractionType)interactionType {
   [self p_dismiss];
}
超时时长设置

超时的计算逻辑：穿山甲从开发者加载广告开始倒计时（注意并非从SDK初始化开始计时），若广告在倒计时结束之前完成广告加载，则会为开发者响应广告。若倒计时结束前未完成广告加载，则将响应超时。建议开发者:

复制
_splashAd.tolerateTimeout = 3;
自定义底部logo设置

开发者可自定义开屏底部View，用于logo展示等个性化设置。

复制
// 设置半屏尺寸

self.splashFrame = CGRectMake(0, 0, _rootViewController.view.size.width, _rootViewController.view.size.height - 100);
    
    _splashAd = [[BUSplashAd alloc] initWithSlotID:self.viewModel.slotID adSize:self.splashFrame.size];
    // 不支持中途更改代理，中途更改代理会导致接收不到广告相关回调，如若存在中途更改代理场景，需自行处理相关逻辑，确保广告相关回调正常执行。
    _splashAd.delegate = self;
    _splashAd.zoomOutDelegate = self;
    
    _splashAd.supportZoomOutView = YES;
    _splashAd.tolerateTimeout = 3;

   [_splashAd loadAdData];
    
    // 自定义底部视图
    _bottomView = [[UIView alloc] initWithFrame:CGRectMake(0, self.splashFrame.size.height, self.splashFrame.size.width, 100)];
    _bottomView.backgroundColor = [UIColor brownColor];

// 调用展示广告show
- (void)splashAdLoadSuccess:(nonnull BUSplashAd *)splashAd {
   [self pbud_logWithSEL:_cmd msg:@"\"];
    // 使用应用keyWindow的rootViewController（接入简单，推荐）
   [splashAd showSplashViewInRootViewController:_rootViewController];
    
}
// 添加开屏广告底部logo
- (void)splashAdRenderSuccess:(nonnull BUSplashAd *)splashAd {
    if (self.showBottomView) { // 自定义底部视图
       [splashAd.splashRootViewController.view addSubview:_bottomView];
   }
}
开屏回调说明

时机

	

方法

	

说明




广告物料、素材加载成功




	

- (void)splashAdLoadSuccess:(BUSplashAd *)splashAd;




	

物料加载成功，可以在这个回调中调用show方法展示开屏




广告物料、素材加载失败

	

- (void)splashAdLoadFail:(BUSplashAd *)splashAd error:(BUAdError *)error;




	

物料加载失败，不会展示开屏，提供如下错误码：

typedef NS_ENUM(NSInteger, BUSplashAdLoadError) {

BUSplashAdLoadError_UnKnow= 0, //未知错误

BUSplashAdLoadError_DataError = 1, // 数据加载失败

BUSplashAdLoadError_TimeOut = 2, // 超时

};




广告渲染成功

	

- (void)splashAdRenderSuccess:(BUSplashAd *)splashAd;

	

load方法调用后，若渲染成功则返回该回调；show方法调用后开屏视图会被添加到ViewController上。




广告渲染失败

	

- (void)splashAdRenderFail:(BUSplashAd *)splashAd error:(BUAdError *)error;




	

load方法调用后，若渲染失败则返回该回调；即使调用show方法开屏视图也不会被添加到ViewController上，提供如下错误码：

typedef NS_ENUM(NSInteger, BUSplashAdErrorCode) { CSJSplashAdError_Unknow = 0, CSJSplashAdError_LoadFailed = 1, // 物料加载失败 CSJSplashAdError_ResourceFailed = 2, // 素材加载失败 CSJSplashAdError_RenderFailed = 3, // 渲染失败 CSJSplashAdError_TimeOut = 23 // 超时

 };




广告即将展示

	

- (void)splashAdWillShow:(BUSplashAd * )splashAd;

	







广告展示

	

- (void)splashAdDidShow:(BUSplashAd * )splashAd;

	

注意：该回调时机和show埋点上报时机一致




广告视图关闭




	

- (void)splashAdDidClose:(BUSplashAd * )splashAd closeType:(BUSplashAdCloseType)closeType；






	

广告视图会在以下场景关闭：

1、开屏未转化关闭（点击跳过、倒计时结束），通知开发者开屏视图已经关闭

2、 开屏转化后内开调起市场页，市场页关闭时通知开发者开屏视图已经关闭

3、开屏转化其他场景，点击广告后通知开发者开屏视图已经关闭

其他说明：

1、广告视图关闭后由SDK移除开屏视图

2、closeType标记从哪种场景关闭

复制
brtypedef NS_ENUM(NSInteger, BUSplashAdCloseType) {br    BUSplashAdCloseType_Unknow = 0, // 未知br    BUSplashAdCloseType_ClickSkip = 1,  // 点击跳过br    BUSplashAdLoadError_CountdownToZero = 2 , // 倒计时结束br    BUSplashAdCloseType_ClickJump = 3  // 点击跳转br};



广告视图控制器关闭






	

- (void)splashAdViewControllerDidClose:(BUSplashAd *)splashAd;




	

如果采用自定义视图控制器展示广告，广告视图控制器会在以下场景关闭：

1、开屏未转化关闭（点击跳过、倒计时结束），通知开发者关闭视图控制器

2、开屏转化后内跳，中间页、市场页关闭时，通知开发者关闭视图控制器

3、开屏转化外跳，即将跳转时，通知开发者关闭视图控制器




广告点击

	

- (void)splashAdDidClick:(BUSplashAd * )splashAd;

	







广告中间页、市场页面关闭

	

- (void)splashDidCloseOtherController:(BUSplashAd *)splashAd interactionType:(BUInteractionType)interactionType;




	

此回调在广告跳转到其他控制器时，该控制器被关闭时调用。

interactionType：此参数可区分是打开的appstore/网页/视频广告详情页面




视频广告播放完毕

	

- (void)splashVideoAdDidPlayFinish:(BUSplashAd *)splashAd didFailWithError:(NSError *)error;

	




接入须知

1、自渲染&模板渲染 rit位：新版本不区分渲染方式的rit位

2、开屏内部相关的视图生命周期由SDK管理，开发者关注BUSplashAd对象。当BUSplashAd被非法释放时，开屏内部视图会被同时移除释放。

3、接入开屏的最佳实践，在loadSuccess回调中调用show方法展示广告。

4、开屏视图关闭后，splashView即被释放（nil），如在开屏结束后依赖开屏视图做动画，可以使用splashViewSnapshot对象。

5、rootViewController建议使用应用当前window对象的rootViewController(keyWindow.rootViewController)或者当前视图控制器的根控制器navigationController（viewController.navigationController）。如果使用新创建的viewController，注意在开屏广告视图控制器关闭回调中splashAdViewControllerDidClose:从当前控制器堆栈中移除viewController：

复制
// 开屏视图控制器关闭回调（4.7.0.3新增回调）
- (void)splashAdViewControllerDidClose:(BUSplashAd *)splashAd {
   [viewController.view removeFromSuperview];
   [viewController removeFromParentViewController];
}

6、loadAdData方法不是线程安全的，请尽量在主线程调用。




本篇目录
简介
开屏样式接口概览
创建广告对象、请求广告、展示广告
超时时长设置
自定义底部logo设置
开屏回调说明
接入须知
评价此篇文档
有帮助
无帮助

---
Source: https://www.csjplatform.com/supportcenter/5379


# 模板渲染Draw广告

开发测试/iOS集成文档/模板渲染Draw广告
模板渲染Draw广告
最近更新 2025-10-11 16:16:52
简介

模板渲染Draw：场景应在应用的内容流中与应用内容穿插展示，Draw信息流广告默认全屏展示。

支持的广告尺寸：竖版视频(宽高比9:16)

权限

模版渲染Draw视频广告： 默认提供

使用说明

模版Draw信息流广告使用BUNativeExpressAdManager对象调用loadAd请求广告，使用BUNativeExpressAdView对象来进行广告的渲染render，通过设置BUNativeExpressAdViewDelegate代理，获取广告、展示、点击、关闭等回调。

创建广告位对象、请求广告

BUNativeExpressAdManager

请求广告时需要传入广告位对象，广告位对象创建时必须传入广告位ID

字段定义

	

是否必传

	

字段名称

	

字段类型

	

备注




BUAdSlot

	

是

	

/

	

/

	

基本信息配置，详见事例




adSize

	

是

	

广告尺寸

	

CGSize

	

广告尺寸，默认全屏展示

请求广告前需要配置BUAdSlot广告的基本信息，使用BUNativeExpressAdManager创建对象，使用BUNativeExpressAdManager调用loadAd请求广告

复制
//保证每次请求的广告对象为新的广告对象，避免重复使用广告同一个对象进行广告请求
BUAdSlot *slot1 = [[BUAdSlot alloc] init];
 slot1.ID = self.viewModel.slotID;
 slot1.AdType = BUAdSlotAdTypeDrawVideo; //required
 slot1.isOriginAd = YES; //required
 slot1.position = BUAdSlotPositionTop;
 slot1.imgSize = [BUSize sizeBy:BUProposalSize_DrawFullScreen];
  if (!self.nativeExpressAdManager) {
       self.nativeExpressAdManager = [[BUNativeExpressAdManager alloc] initWithSlot:slot1 adSize:self.view.bounds.size];
   }
  self.nativeExpressAdManager.adSize = self.view.bounds.size;
  self.nativeExpressAdManager.delegate = self;
  [self.nativeExpressAdManager loadAdDataWithCount:count];

注意：

①self.nativeExpressAdManager可以重用

②模版Draw信息流广告尺寸adSize默认全屏展示

③模版Draw信息流广告可多条广告同时请求，最大请求数量为3

展示广告

由于官方Demo使用tableView进行的广告的插入展示，直接更新数据源即可更新广告的相关插入展示，此处建议开发者在收到nativeExpressAdSuccessToLoad回调后再进行广告的渲染展示，可保证播放流畅和展示流畅，用户体验更好。

建议在收到nativeExpressAdSuccessToLoad回调后再进行广告的渲染展示，刷新数据源

复制
- (void)nativeExpressAdSuccessToLoad:(BUNativeExpressAdManager *)nativeExpressAd views:(NSArray<__kindof BUNativeExpressAdView *> *)views {
    if (views.count) {
        NSMutableArray *dataSources = [self.dataSource mutableCopy];
       [views enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
           BUNativeExpressAdView *expressView = (BUNativeExpressAdView *)obj;
           expressView.rootViewController = self;
           // important: 此处会进行WKWebview的渲染，建议一次最多预加载三个广告，如果超过3个会很大概率导致WKWebview渲染失败。
          [expressView render];
           NSUInteger index = rand() % (self.dataSource.count-3)+2;
          [dataSources insertObject:expressView atIndex:index];
       }];
        self.dataSource = [dataSources copy];
   }
   [self.tableView reloadData];
}
资源

详细接入可参照Demo中的BUDExpressDrawViewController类，广告加载请求部分可参照loadData方法，广告展示渲染部分可参照nativeExpressAdSuccessToLoad方法







本篇目录
简介
权限
使用说明
创建广告位对象、请求广告
展示广告
资源
评价此篇文档
有帮助
无帮助

---
Source: https://www.csjplatform.com/supportcenter/5385


# 模板渲染插屏广告

开发测试/iOS集成文档/历史功能/模板渲染插屏广告
模板渲染插屏广告
最近更新 2023-02-17 10:53:25
简介

模板渲染插屏：使用场景为应用功能自然体验中断时，例如暂停视频播放，游戏关卡之间。插屏广告不应突然出现，不应干扰用户使用应用的正常流程，例如应用加载，退出应用或者游戏过程中。

支持的广告尺寸：1:1、2:3、3:2 三种尺寸注意：⚠️⚠️⚠️ 请求模版插屏广告，高度值不能设置为0⚠️⚠️⚠️

5.0.0.0及以上版本删除插屏广告样式接口（BUAdSlotAdType中移除枚举值BUAdSlotAdTypeInterstitial、删除BUNativeExpressInterstitialAd类）

权限

模板渲染插屏： 已回收，不支持创建（该部分面向历史有历史使用该广告类型的开发者）

为避免渲染过程产生广告视图形变，插屏广告的请求尺寸务必和媒体平台配置相同尺寸

使用说明

模版插屏使用BUNativeExpressInterstitialAd对象调用loadAdData请求广告，使用BUNativeExpressInterstitialAd对象调用showAdFromRootViewController:展示广告，通过设置BUNativeExpresInterstitialAdDelegate代理，获取广告、展示、点击、关闭等回调。

创建广告位对象、请求广告

BUNativeExpressInterstitialAd

请求广告时需要传入广告位对象，广告位对象创建时必须传入广告位ID

字段定义

	

是否必传

	

字段名称

	

字段类型

	

备注




slotID

	

是

	

代码位

	

NSString

	

代码位ID




adSize

	

是

	

广告尺寸

	

CGSize

	

广告尺寸，务必和媒体平台配置相同尺寸

使用BUNativeExpressInterstitialAd创建对象，使用BUNativeExpressInterstitialAd调用loadAdData请求广告

复制
self.interstitialAd = [[BUNativeExpressInterstitialAd alloc] initWithSlotID:slotID adSize:CGSizeMake(width, height)];
self.interstitialAd.delegate = self;
[self.interstitialAd loadAdData];
展示广告

调用showAdFromRootViewController:方法展示插屏广告，此处需要传入当前展示的页面。一定要设置rootViewController，即展示广告和跳转落地页需要的viewController，此处建议开发者在收到nativeExpresInterstitialAdRenderSuccess回调后展示插屏广告，可保证播放流畅和展示流畅，用户体验更好。

复制
if (self.interstitialAd) {
   [self.interstitialAd showAdFromRootViewController:self];
}
展示时机

在收到nativeExpresInterstitialAdRenderSuccess回调后再进行广告的展示，可保证播放流畅和展示流畅，用户体验更好。

复制
- (void)nativeExpresInterstitialAdRenderSuccess:(BUNativeExpressInterstitialAd *)interstitialAd {
 //此回调方法中进行广告的展示，可保证播放流畅和展示流畅，用户体验更好。
}
接收广告加载结果

回调方法

	

注释




nativeExpresInterstitialAd:didFailWithError

	

返回的错误码(error)表示广告加载失败的原因，所有错误码详情请见链接。Link




nativeExpresInterstitialAdRenderFail

	

渲染失败，网络原因或者硬件原因导致渲染失败,可以更换手机或者网络环境测试。建议升级到穿山甲平台最新版本




nativeExpresInterstitialAdDidLoad

	

广告素材物料加载成功




nativeExpresInterstitialAdRenderSuccess

	

渲染成功

BUNativeExpresInterstitialAdDelegate回调说明

回调方法

	

注释




nativeExpresInterstitialAdDidLoad:

	

广告物料加载成功




nativeExpresInterstitialAd: didFailWithError:

	

返回的错误码(error)表示广告加载失败的原因，所有错误码详情请见链接。Linkhttps://ad.oceanengine.com/union/media/doc?id=5de4cc6d78c8690012a90aa5)




nativeExpresInterstitialAdRenderSuccess:

	

渲染成功回调




nativeExpresInterstitialAdRenderFail: error:

	

渲染失败，网络原因或者硬件原因导致渲染失败,可以更换手机或者网络环境测试。建议升级到穿山甲平台最新版本




nativeExpresInterstitialAdWillVisible:

	

模版插屏广告即将展示




nativeExpresInterstitialAdDidClick:

	

点击回调




nativeExpresInterstitialAdWillClose:

	

广告即将关闭回调




nativeExpresInterstitialAdDidClose:

	

关闭回调




nativeExpresInterstitialAdDidCloseOtherController: interactionType:

	

此回调在广告跳转到其他控制器时，该控制器被关闭时调用interactionType：此参数可区分是打开的appstore/网页/详情页面等等

通过设置BUNativeExpresInterstitialAdDelegate代理，获取广告、展示、点击、关闭等回调。其他代理方法可参见demo中BUDExpressInterstitialViewController类

#pragma ---BUNativeExpresInterstitialAdDelegate部分

注意事项

①必须要设置rootViewController，用来处理广告跳转。SDK里所有的跳转均采用present的方式，请确保传入的rootViewController不能为空且没有present其他的控制器，否则会出现presentedViewController已经存在而导致present失败。

②为保证播放流畅和展示流畅，用户体验更好，在收到nativeExpresInterstitialAdRenderSuccess回调后进行广告的展示

③判断广告是否有效直接使用self.interstitialAd判断即可

资源

详细接入可参照Demo中的BUDExpressInterstitialViewController类，广告加载请求部分可参照loadInterstitialWithSlotID方法，广告展示部分可参照showInterstitial方法










本篇目录
简介
权限
使用说明
创建广告位对象、请求广告
展示广告
展示时机
接收广告加载结果
注意事项
资源
评价此篇文档
有帮助
无帮助

---
Source: https://www.csjplatform.com/supportcenter/5387


# 模板渲染激励视频广告

开发测试/iOS集成文档/模板渲染激励视频广告
模板渲染激励视频广告
最近更新 2025-12-29 18:34:49
简介

激励视频是一种全屏播放的视频广告，用户可以在观看完整的视频后获取奖励,视频广告播放结束后会显示结束页面，引导用户进行后续动作。

支持的广告尺寸： 全屏横屏(宽高比16:9)、全屏竖屏(宽高比9:16)

配置说明

①开发者根据展示场景勾选视频播放方向：横版or竖版，展示过程中不可旋转

②奖励名称和奖励数量依据自身项目需求设置即可，例如：奖励名称为金币，奖励数量：1000。V< 5.2.0.4，奖励发放实际情况按照平台配置参数进行下发，客户端接口不生效。iOS/Android SDK版本>=5.2.0.4，支持在激励视频广告中显示奖励内容，通过客户端接口设置rewardName和rewardAmount字段，详见下方“在激励广告中显示奖励内容”模块说明。

奖励发放设置：
当选择无需服务器判断时，开发者可以根据客户端nativeExpressRewardedVideoAdServerRewardDidSucceed回调进行奖励的发放。
当选择需要服务器判断时，开发者需要按照以下要求进行接口开发，并在平台上填写回调URL。服务器回调是指在用户在看完激励视频达到奖励条件时，穿山甲服务端会向开发者服务端发送一个验证申请，开发者服务端收到请求后判断是否给用户奖励，返回给穿山甲服务器true/false，最终客户端会给出nativeExpressRewardedVideoAdServerRewardDidSucceed回调，开发者根据回调进行奖励发放，或者通过服务端接口验证结果进行奖励发放。
奖励发放机制分为客户端回调与服务端回调，具体可参考文档： 激励视频交互方式简介&奖励方法说明
基础功能说明

模版激励视频使用BUNativeExpressRewardedVideoAd对象调用loadAdData方法请求广告，使用BUNativeExpressRewardedVideoAd对象调用showAdFromRootViewController:展示广告，通过设置BUNativeExpressRewardedVideoAdDelegate代理，获取广告、展示、点击、关闭等回调。

创建广告位对象、请求广告

请求广告时需要传入广告位对象，广告位对象创建时必须传入广告位ID

字段定义

	

是否必传

	

字段名称

	

字段类型

	

备注




slotID

	

是

	

代码位ID

	

NSString

	

应为json序列化后的字符串




model

	

是

	

/

	

BURewardedVideoModel

	
rewardName：奖励名称 
是否必传：否
显示奖励内容时使用
rewardAmount：奖励数量
是否必传：否
显示奖励内容时使用
extra
是否必传：否
透传参数





使用BUNativeExpressRewardedVideoAd创建对象，使用BUNativeExpressRewardedVideoAd调用loadAdDat请求广告

复制
    //保证每次请求的广告对象为新的广告对象，避免重复使用广告同一个对象进行广告请求    
    BURewardedVideoModel *model = [[BURewardedVideoModel alloc] init];

    model.rewardName = @"金币";

    model.rewardAmount = 300;

    self.rewardedAd = [[BUNativeExpressRewardedVideoAd alloc] initWithSlotID:slotID rewardedVideoModel:model];

    // 不支持中途更改代理，中途更改代理会导致接收不到广告相关回调，如若存在中途更改代理场景，需自行处理相关逻辑，确保广告相关回调正常执行。
    self.rewardedAd.delegate = self;

    // optional
    self.rewardedAd.rewardPlayAgainInteractionDelegate = self.expressRewardedVideoAgainDelegateObj;

   [self.rewardedAd loadAdData];
展示广告

激励视频广告需要让用户主动选择去观看，给用户选择，不能强制用户观看激励视频广告。广告播放完成需要给用户发放相应的奖励。调用showAdFromRootViewController:方法展示激励视频广告，此处需要传入当前展示的页面。一定要设置rootViewController，即展示广告和跳转落地页需要的viewController

复制
//show的时候会进行WKWebview的渲染，建议一次最多展示三个广告，如果超过3个会很大概率导致WKWebview渲染失败。当然一般情况下激励视频一次只会show一个
- (void)showRewardVideoAd {
    if (self.rewardedAd) {
       [self.rewardedAd showAdFromRootViewController:self];
   }
}
展示时机

在收到nativeExpressRewardedVideoAdDidDownLoadVideo回调后再允许用户观看广告，可保证播放流畅和展示流畅，用户体验更好。

复制
- (void)nativeExpressRewardedVideoAdDidDownLoadVideo:(BUNativeExpressRewardedVideoAd *)rewardedVideoAd {
 //在此回调方法后进行广告的展示，可保证播放流畅和展示流畅，用户体验更好。
}
广告展示后重新加载广告

同一次请求的广告最多只能计一次展示，重复的展示会被系统过滤。因此建议开发者在用户观看完广告后在nativeExpressRewardedVideoAdDidClose回调里将原来的广告对象置为nil 保证广告对象为新的请求对象

复制
- (void)nativeExpressRewardedVideoAdDidClose:(BUNativeExpressRewardedVideoAd *)rewardedVideoAd {
//在此回调方法中可进行广告的置空操作
}
奖励发放机制

奖励发放机制分为客户端回调与服务端回调，具体可参考文档： 激励视频交互方式简介&奖励方法说明

接收广告加载结果

BUNativeExpressRewardedVideoAdDelegate回调说明

回调方法

	

注释




nativeExpressRewardedVideoAd:didFailWithError

	

返回的错误码(error)表示广告加载失败的原因，所有错误码详情请见链接。Link




nativeExpressRewardedVideoAdViewRenderFail:error

	

渲染失败，网络原因或者硬件原因导致渲染失败,可以更换手机或者网络环境测试。建议升级到穿山甲平台最新版本




nativeExpressRewardedVideoAdDidLoad

	

广告素材物料加载成功




nativeExpressRewardedVideoAdDidDownLoadVideo

	

视频下载完成

BUNativeExpressRewardedVideoAdDelegate回调说明

回调方法

	

注释




nativeExpressRewardedVideoAdDidLoad:

	

回调进入证明广告物料已成功加载




nativeExpressRewardedVideoAd: didFailWithError:

	

此回调方法中可定位具体的失败原因对应的错误码，打印error即可。所有错误码详情请见链接。Link




nativeExpressRewardedVideoAdCallback:withType:

	

此回调可知模版激励视频的广告类型




nativeExpressRewardedVideoAdDidDownLoadVideo

	

建议在此回调方法中进行广告的展示操作，可保证播放流畅和展示流畅，用户体验更好。




nativeExpressRewardedVideoAdViewRenderSuccess:

	

渲染成功回调。3100之后版本SDK，广告展示之后才会回调




nativeExpressRewardedVideoAdViewRenderFail:

	

渲染失败，网络原因或者硬件原因导致渲染失败,可以更换手机或者网络环境测试。建议升级到穿山甲平台最新版本




nativeExpressRewardedVideoAdWillVisible:

	

模版激励视频广告即将展示




nativeExpressRewardedVideoAdDidVisible:

	

模版激励视频广告已经展示




nativeExpressRewardedVideoAdWillClose:

	

模版激励视频广告即将关闭




nativeExpressRewardedVideoAdDidClose:

	

用户关闭广告时会触发此回调，注意：任何广告的关闭操作必须用户主动触发;




nativeExpressRewardedVideoAdDidClick:

	

点击回调方法




nativeExpressRewardedVideoAdDidClickSkip:

	

跳过回调方法




nativeExpressRewardedVideoAdDidPlayFinish:

	

视频正常播放完成时可触发此回调方法，当广告播放发生异常时，不会进入此回调;




nativeExpressRewardedVideoAdServerRewardDidSucceed:

	

异步请求的服务器验证成功回调，开发者需要在此回调中进行奖励发放。现在包括两个验证方法:1. C2C不需要服务器验证2。S2S需要服务器验证。nativeExpressRewardedVideoAdServerRewardDidFail:异步请求的服务器验证失败回调。可在此回调方法中打印error，定位具体失败的原因，或通过抓包定位具体原因，抓包地址：https://域名或者ip地址/api/ad/union/sdk/get_ads/ 提供返回的数据进行确认）到【留言反馈】-【技术类提问入口（技术类暂仅支持此入口的提问回复）】进行反馈，相关同学会为您处理




nativeExpressRewardedVideoAdDidCloseOtherController:interactionType:此回调在广告跳转到其他控制器时，该控制器被关闭时调用interactionType：

	

此参数可区分是打开的appstore/网页/视频广告详情页面

nativeExpressRewardedVideoAdServerRewardDidSucceed 回调说明

①不使用户服务端奖励验证的情况下nativeExpressRewardedVideoAdServerRewardDidSucceed回调只校验视频播放状态或者进度，视频播放90%或者因播放器异常导致出现播放失败，那么穿山甲都会回调nativeExpressRewardedVideoAdServerRewardDidSucceed，并且返回结果为true。

②使用服务端奖励验证的情况下nativeExpressRewardedVideoAdServerRewardDidSucceed回调返回的则是开发者服务端验证的结果。

综上： nativeExpressRewardedVideoAdServerRewardDidSucceed 回调只是视频播放状态的一个结果或者是开发者返回的结果，不代表此次播放是否计费等广告业务指标。

高级功能说明
进阶奖励功能
接入说明

①适用范围：此功能双端SDK均需在4600及以上。

②权限等级：功能为白名单制。对开发者appid或者指定的rit进行加白。

③视频时长限制：进阶奖励仅针对视频素材时长>35s生效。

接口说明
复制
// 区分此次奖励的类型，默认为BURewardType_Default

@property (nonatomic, assign) BURewardType rewardType;

typedef NS_ENUM(NSInteger, BURewardType) {    

BURewardType_Default            = 0,       // 基础奖励 (满足观看时长、互动抵时长、playable试完等)    

BURewardType_Interact           = 1,       // 进阶奖励-互动    

BURewardType_VideoComplete      = 2,       // 进阶奖励-超过30s的视频播放完成

};

number of rewards of propose, 0.0 ~ 1.0 (BURewardType_Default: 1). // 建议奖励数量，进阶奖励功能可选

@property (nonatomic, assign) float rewardPropose;//若rewardedVideoAd.rewardedVideoModel.rewardType不为BURewardType_Default时(暂支持BURewardType_Interact、BURewardType_VideoComplete)，即可发奖励，奖励数量可以参考rewardPropose给出的建议。
在激励广告中显示奖励内容
穿山甲激励广告增加「在广告中显示奖励内容」功能，根据开发者配置的奖励物品和数量，在激励视频广告播放过程中提示用户完成激励任务后将获取的奖励收益，以激发用户兴趣、观看更久更完整的视频广告，提升整体的CTR和收入。
使用说明

①版本要求：iOS/Android SDK版本>=5.2.0.4；

②权限等级：功能面向所有账户开放；

③若想使用此功能，必须在穿山甲平台-激励视频代码位创编中配置开启“是否在广告中显示奖励内容”开关。




④开启后，根据系统优选的结果，您设置的奖励物品名称、奖励数量将有可能在广告播放过程中展示。开发者可以通过两种方案配置具体显示的奖励内容。

在截图所示穿山甲平台的代码位创编页面中里输入奖励名称和数量。
在请求广告时传入奖励名称和数量，详见文档「接口改动」部分。
在实际使用时，会按照优先级 2 > 1 来展示，若奖励名称和数量任一为空，则认为此对值无效。
奖励数量为1~5个数字整数值，奖励内容为1～10个英文字符长度的字符串。

⑤具体的使用示例与温馨提示

奖励物品名称“金币”，奖励数量“500”，广告挽留弹窗提示用户继续看完视频可领取“500金币”；奖励物品名称“分钟免广告”，奖励数量“60”，挽留弹窗显示继续观看xxs可领取奖励“60分钟免广告”。
建议开发者配置/回传用户可真实获取的奖励内容，避免用户对奖励预期与获取产生出入。
推荐开发者通过接口回传的形式，以便更灵活、动态地下发奖励，在用户粒度个性化优化收益。




接口说明

在构建BUNativeExpressRewardedVideoAd时，使用BURewardedVideoModel，传入奖励名称与奖励数量。

复制
BURewardedVideoModel *model = [[BURewardedVideoModel alloc] init];
model.rewardName = @"金币";
model.rewardAmount = 300;
self.rewardedAd = [[BUNativeExpressRewardedVideoAd alloc] initWithSlotID:slotID rewardedVideoModel:model];
注意事项

①必须要设置rootViewController，用来处理广告跳转。SDK里所有的跳转均采用present的方式，请确保传入的rootViewController不能为空且没有present其他的控制器，否则会出现presentedViewController已经存在而导致present失败。

②如选择服务器回调方式，请确保userId为NSString类型且不为空，并保证回调URL 格式为

复制
{
    "isValid": true
}

③nativeExpressRewardedVideoAdViewRenderSuccess渲染成功回调在广告展示后

④为保证播放流畅和展示流畅，用户体验更好，在收到nativeExpressRewardedVideoAdDidDownLoadVideo回调后进行广告的展示

⑤extra透传参数应为json序列化后的字符串，确保不为空

⑥判断广告是否有效直接使用self.rewardedAd判断即可。目前isAdValid字段已废弃，请不要使用此字段判断广告是否有效

⑦个性化模板广告为了优化展示速度,会使用本地模板,请求时会拦截相关数据.如果接入方正在使用H5的页面发送请求,会造成请求body清空,其他逻辑不变.如果使用body传参请更换其他方式.例如:jsBridge方式。

⑧广告的关闭操作，必须用户主动触发，客户端不能进行代码层面的强制关闭操作

资源

详细接入可参照Demo中的BUDExpressRewardedVideoViewController类，广告加载请求部分可参照loadRewardVideoAdWithSlotID方法，广告展示部分可参照showRewardVideoAd方法










本篇目录
简介
配置说明
基础功能说明
创建广告位对象、请求广告
展示广告
展示时机
广告展示后重新加载广告
奖励发放机制
接收广告加载结果
高级功能说明
进阶奖励功能
接入说明
接口说明
在激励广告中显示奖励内容
使用说明
接口说明
注意事项
资源
评价此篇文档
有帮助
无帮助

---
Source: https://www.csjplatform.com/supportcenter/5381


# 模板渲染信息流广告

开发测试/iOS集成文档/模板渲染信息流广告
模板渲染信息流广告
最近更新 2025-10-11 16:16:46
简介

模板渲染信息流：场景应在应用的内容流中与应用内容穿插展示，广告内容与应用其他内容区分开，防止意外点击。

支持的广告尺寸：开发者在穿山甲平台上可以进行多模板、多尺寸的勾选。模板渲染信息流广告支持开发者调整、编辑

权限

模版渲染信息流广告： 默认提供

使用说明

模版信息流广告使用BUNativeExpressAdManager对象调用loadAd请求广告，使用BUNativeExpressAdView对象来进行广告的渲染render，通过设置BUNativeExpressAdViewDelegate代理，获取广告、展示、点击、关闭等回调。

创建广告位对象、请求广告

BUNativeExpressAdManager

请求广告时需要传入广告位对象，广告位对象创建时必须传入广告位ID

字段定义

	

是否必传

	

字段名称

	

字段类型

	

备注




BUAdSlot

	

是

	

/

	

/

	

基本信息配置，详见事例




adSize

	

是

	

广告尺寸

	

CGSize

	

广告尺寸，可将宽度设置为屏宽，自适应时，可将高度直接设置为0

请求广告前需要配置BUAdSlot广告的基本信息，使用BUNativeExpressAdManager创建对象，使用BUNativeExpressAdManager调用loadAd请求广告

复制
//保证每次请求的广告对象为新的广告对象，避免重复使用广告同一个对象进行广告请求
BUAdSlot *slot1 = [[BUAdSlot alloc] init];
slot1.ID = self.viewModel.slotID;
slot1.AdType = BUAdSlotAdTypeFeed;
BUSize *imgSize = [BUSize sizeBy:BUProposalSize_Feed228_150];
slot1.imgSize = imgSize;
slot1.position = BUAdSlotPositionFeed;
// self.nativeExpressAdManager可以重用
 if (!self.nativeExpressAdManager) {
    self.nativeExpressAdManager = [[BUNativeExpressAdManager alloc] initWithSlot:slot1 adSize:CGSizeMake(self.widthSlider.value, self.heightSlider.value)];
   }
self.nativeExpressAdManager.adSize = CGSizeMake(self.widthSlider.value, self.heightSlider.value);
self.nativeExpressAdManager.delegate = self;
NSInteger count = (NSInteger)self.adCountSlider.value;
[self.nativeExpressAdManager loadAdDataWithCount:count];

注意：

self.nativeExpressAdManager可以重用
模版信息流的尺寸adSize建议宽度设置为屏宽 高度设置为0或者设置平台勾选的高度 不要随意设置尺寸大小否则有可能出现拉取失败或者广告变形的情况
模版信息流广告可多条广告同时请求，最大请求数量为3
模版信息流广告默认设置imgSize为BUProposalSize_Feed228_150即可
展示广告

由于官方Demo使用tableView进行的广告的插入展示，直接更新数据源即可更新广告的相关插入展示，此处建议开发者在收到nativeExpressAdSuccessToLoad回调后再进行广告的渲染展示，可保证播放流畅和展示流畅，用户体验更好。因此如果不是使用此种方式进行的信息流广告的展示，切记不要忘记addSubview添加广告对象来进行广告的展示。

展示时机

在收到nativeExpressAdSuccessToLoad回调后再进行广告的渲染展示，刷新数据源

复制
- (void)nativeExpressAdSuccessToLoad:(BUNativeExpressAdManager *)nativeExpressAd views:(NSArray<__kindof BUNativeExpressAdView *> *)views {
   [self.expressAdViews removeAllObjects];//【重要】不能保存太多view，需要在合适的时机手动释放不用的，否则内存会过大
    if (views.count) {
       [self.expressAdViews addObjectsFromArray:views];
       [views enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
           BUNativeExpressAdView *expressView = (BUNativeExpressAdView *)obj;
           expressView.rootViewController = self;
           // important: 此处会进行WKWebview的渲染，建议一次最多预加载三个广告，如果超过3个会很大概率导致WKWebview渲染失败。
          [expressView render];
       }];
   }
   [self.tableView reloadData];
}

注意：为了避免内存过大触发系统优化机制，建议在合适的时机进行remove移除操作，目前SDK使用WKWebView实现，此建议不限于模版信息流广告，其他广告同样适用，保证不使用的广告，在合适的时机进行释放处理

接收广告加载结果

BUNativeExpressAdViewDelegate回调说明

回调方法

	

注释




nativeExpressAdFailToLoad

	

返回的错误码(error)表示广告加载失败的原因，所有错误码详情请见链接。Link




nativeExpressAdViewRenderFail

	

渲染失败，网络原因或者硬件原因导致渲染失败,可以更换手机或者网络环境测试。建议升级到穿山甲平台最新版本




nativeExpressAdSuccessToLoad

	

广告视图加载成功




nativeExpressAdViewRenderSuccess

	

渲染成功

通过设置BUNativeExpressAdViewDelegate代理，获取广告、展示、点击、关闭等回调。其他代理方法可参见Demo中BUDExpressFeedViewController类#pragma mark - BUNativeExpressAdViewDelegate部分




BUVideoAdViewDelegate回调说明

回调方法

	

注释




didLoadFailWithError:

	

播放失败时调用




playerReadyToPlay:

	

准备播放时调用




stateDidChanged:

	

回放状态改变时调用




rewardDidCountDown:

	

当奖励视频奖励开始倒计时调用，5.0.0.0及以上版本新增




playerDidPlayFinish:

	

播放结束时调用




videoAdViewDidClick:

	

点击时调用




videoAdViewFinishViewDidClick:

	

播放完成被点击时调用




videoAdViewDidCloseOtherController:

	

在app中打开appstore或打开网页或查看视频广告详情页面

激励信息流代理方法BUVideoAdViewDelegate新增奖励倒计时回调接口：

复制
/**
 This method is called when the countdown of reward video rewards starts
 @param countDown : countdown of reward video rewards
 */
- (void)videoAdView:(BUVideoAdView *)videoAdView
 rewardDidCountDown:(NSInteger)countDown;
dislike回调说明
复制
- (void)nativeExpressAdView:(BUNativeExpressAdView *)nativeExpressAdView dislikeWithReason:(NSArray<BUDislikeWords *> *)filterWords {
    //【重要】需要在点击叉以后 在这个回调中移除视图，否则，会出现用户点击叉无效的情况
   [self.expressAdViews removeObject:nativeExpressAdView];
    NSUInteger index = [self.expressAdViews indexOfObject:nativeExpressAdView];
    NSIndexPath *indexPath=[NSIndexPath indexPathForRow:index inSection:0];
   [self.tableView reloadRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationFade]; 
}

- (void)nativeExpressAdViewDidRemoved:(BUNativeExpressAdView *)nativeExpressAdView {
//【重要】若开发者收到此回调，代表穿山甲会主动关闭掉广告，广告移除后需要开发者对界面进行适配
  NSUInteger index = [self.expressAdViews indexOfObject:nativeExpressAdView];
  [self.expressAdViews removeObject:nativeExpressAdView];
  NSIndexPath *indexPath=[NSIndexPath indexPathForRow:index inSection:0];
  NSIndexPath *placeholderIndexPath = [NSIndexPath indexPathForRow:index + 1 inSection:0];
  [self.tableView deleteRowsAtIndexPaths:@[indexPath, placeholderIndexPath] withRowAnimation:UITableViewRowAnimationFade];
}

注意：

切记不要忘记设置BUNativeExpressAdView对象的rootViewController，保证当前传入的rootViewController不为空，并确保self.nativeExpressAdManager进行了重用判断，否则有可能出现偶现dislike回调方法不进的异常场景

BUMaterialMeta.h

广告推荐说明dislike功能下 “为什么看到此广告”模块，相关API在5.1.0.0及之后版本会完全下掉

复制
/// compliance statement of personalized advertising push
@property (nonatomic, strong) BUPersonalizationPrompts *personalPrompts DEPRECATED_MSG_ATTRIBUTE("The 'personalPrompts' api will not work and will be removed in the future.");

BUAdSlot.h

isOriginAd只有banner广告需要设置YES（5.1.0.0及后续版本）

复制
/// Banner ads are set to YES, other ad types are 0, the default is NO.
@property (nonatomic, assign) BOOL isOriginAd DEPRECATED_MSG_ATTRIBUTE("The 'isOriginAd's is only valid for Banner Ads, no longer required in version 5.1.0.0 and later.");

注意事项

①self.nativeExpressAdManager可以重用

②模版信息流的尺寸adSize建议宽度设置为屏宽 高度设置为0或者设置平台勾选的高度 不要随意设置尺寸大小否则有可能出现拉取失败或者广告变形的情况

③为了避免内存过大触发系统优化机制，建议在合适的时机进行remove移除操作

④不要忘记设置BUNativeExpressAdView对象的rootViewController，保证当前传入的rootViewController不为空

资源

详细接入可参照Demo中的BUDExpressFeedViewController类，广告加载请求部分可参照loadData方法，广告展示渲染部分可参照nativeExpressAdSuccessToLoad方法







本篇目录
简介
权限
使用说明
创建广告位对象、请求广告
展示广告
展示时机
接收广告加载结果
dislike回调说明
注意事项
资源
评价此篇文档
有帮助
无帮助

---
Source: https://www.csjplatform.com/supportcenter/5384


# 模版渲染Banner广告

开发测试/iOS集成文档/模版渲染Banner广告
模版渲染Banner广告
最近更新 2025-10-11 16:16:49
简介

在内容底部或顶部显示的小条形广告。不应将Banner广告放置于文本、图片和应用的其他可点击部分，避免误点。

支持的广告尺寸：支持600*300、600*400、600*500、600*260、600*90、600*150、640*100、690*388共8个尺寸，开发者按照展示场景进行勾选。创建好的尺寸不支持修改。

权限

模板Banner广告： 默认提供

轮播设置

①当是否轮播：是 选择轮播功能时，客户端不需要自行实现间隔一定时间重新请求的操作，轮播功能由SDK内部实现，客户端只要保证广告及时得到展示即可，另轮播间隔为30s～120s之间。

②当是否轮播：否 不选择轮播功能时，客户端可自行实现轮播效果，如间隔一定的时间重新请求广告，保证一定间隔时间内广告内容为新的内容，此时的广告请求间隔建议和平台实现的轮播功能间隔保持一致，请求时间间隔为30s～120s之间。

使用说明

模版banner使用BUNativeExpressBannerView对象调用loadAdData请求广告，使用addSubview添加广告对象来进行广告的展示，通过设置BUNativeExpressBannerViewDelegate代理，获取广告、展示、点击、关闭等回调。

创建广告位对象、请求广告

BUNativeExpressBannerView

请求广告时需要传入广告位对象，广告位对象创建时必须传入广告位ID

字段定义

	

是否必传

	

字段名称

	

字段类型

	

备注




slotID

	

是

	

代码位

	

NSString

	

代码位ID




adSize

	

是

	

广告尺寸

	

CGSize

	

广告尺寸，设置的尺寸务必和媒体平台配置保持相同比例

使用BUNativeExpressBannerView创建对象，使用BUNativeExpressBannerView调用loadAdData请求广告。

复制
//保证每次请求的广告对象为新的广告对象，避免重复使用广告同一个对象进行广告请求
UIWindow *window = nil;
    if ([[UIApplication sharedApplication].delegate respondsToSelector:@selector(window)]) {
        window = [[UIApplication sharedApplication].delegate window];
   }
    if (![window isKindOfClass:[UIView class]]) {
        window = [UIApplication sharedApplication].keyWindow;
   }
    if (!window) {
        window = [[UIApplication sharedApplication].windows objectAtIndex:0];
   }
    CGFloat bottom = 0.0;
    if (@available(iOS 11.0, *)) {
        bottom = window.safeAreaInsets.bottom;
   } else {
        // Fallback on earlier versions
   }
    
    NSValue *sizeValue = [self.sizeDcit objectForKey:slotID];
    CGSize size = [sizeValue CGSizeValue];
self.bannerView = [[BUNativeExpressBannerView alloc] initWithSlotID:slotID rootViewController:self adSize:CGSizeMake(screenWidth, bannerHeigh)];
self.bannerView.frame = CGRectMake(0, self.view.height-bannerHeigh, screenWidth, bannerHeigh);
self.bannerView.delegate = self;
[self.bannerView loadAdData];


字段定义

	

是否必传

	

字段名称

	

字段类型

	

备注




interval

	

是

	

轮播间隔

	

NSInteger

	

轮播间隔时间为30s～120s之间

备注：当媒体平台选择轮播功能时需要设置此参数，时间间隔为30s～120s之间。

复制
UIWindow *window = nil;
    if ([[UIApplication sharedApplication].delegate respondsToSelector:@selector(window)]) {
        window = [[UIApplication sharedApplication].delegate window];
   }
    if (![window isKindOfClass:[UIView class]]) {
        window = [UIApplication sharedApplication].keyWindow;
   }
    if (!window) {
        window = [[UIApplication sharedApplication].windows objectAtIndex:0];
   }
    CGFloat bottom = 0.0;
    if (@available(iOS 11.0, *)) {
        bottom = window.safeAreaInsets.bottom;
   } else {
        // Fallback on earlier versions
   }
    
    NSValue *sizeValue = [self.sizeDcit objectForKey:slotID];
    CGSize size = [sizeValue CGSizeValue];
// important: 升级的用户请注意，初始化方法去掉了imgSize参数
    self.bannerView = [[BUNativeExpressBannerView alloc] initWithSlotID:realSlotId rootViewController:self adSize:size interval:30];
    self.bannerView.frame = CGRectMake((self.view.width-size.width)/2.0, self.view.height-size.height-bottom, size.width, size.height);
  self.bannerView.delegate = self;
  [self.bannerView loadAdData];
展示广告

使用addSubview添加广告对象来进行广告的展示，此处建议开发者在收到nativeExpressBannerAdViewRenderSuccess回调后展示Banner广告，可保证播放流畅和展示流畅，用户体验更好。

复制
- (void)showBanner {
   [self.view addSubview:self.bannerView];
}
展示时机

在收到nativeExpressBannerAdViewRenderSuccess回调后再进行广告的展示，可保证播放流畅和展示流畅，用户体验更好。

复制
- (void)nativeExpressBannerAdViewRenderSuccess:(BUNativeExpressBannerView *)bannerAdView {
 //在此回调方法中进行广告的展示，可保证播放流畅和展示流畅，用户体验更好。
}
接收广告加载结果

回调方法

	

注释




nativeExpressBannerAdView:didLoadFailWithError

	

返回的错误码(error)表示广告加载失败的原因，所有错误码详情请见链接。Link




nativeExpressBannerAdViewRenderFail

	

渲染失败，网络原因或者硬件原因导致渲染失败,可以更换手机或者网络环境测试。建议升级到穿山甲平台最新版本




nativeExpressBannerAdViewDidLoad

	

广告加载成功回调




nativeExpressBannerAdViewRenderSuccess

	

渲染成功回调

BUNativeExpressBannerViewDelegate回调说明

回调方法

	

注释




nativeExpressBannerAdViewDidLoad:bannerAdView

	

加载成功回调




nativeExpressBannerAdView: didLoadFailWithError:

	

返回的错误码(error)表示广告加载失败的原因，所有错误码详情请见链接。Link




nativeExpressBannerAdViewRenderSuccess:

	

渲染成功回调




nativeExpressBannerAdViewRenderFail:error:

	

渲染失败，网络原因或者硬件原因导致渲染失败,可以更换手机或者网络环境测试。建议升级到穿山甲平台最新版本




nativeExpressBannerAdViewWillBecomVisible:

	

当显示新的广告时调用此方法




nativeExpressBannerAdViewDidClick:

	

点击回调




nativeExpressBannerAdView:dislikeWithReason:

	

dislike回调方法，需要在此回调方法中进行广告的移除操作，并将广告对象置为nil，如若不实现此回调方法，关闭按钮将不会生效




nativeExpressBannerAdViewDidCloseOtherController: interactionType:

	

此回调在广告跳转到其他控制器时，该控制器被关闭时调用interactionType：此参数可区分是打开的appstore/网页/详情页面等等

通过设置BUNativeExpressBannerViewDelegate代理，获取广告、展示、点击、关闭等回调。其他代理方法可参见Demo中BUDExpressBannerViewController类#pragma ---BUNativeExpressBannerViewDelegate部分

dislike回调说明
复制
- (void)nativeExpressAdView:(BUNativeExpressAdView *)nativeExpressAdView dislikeWithReason:(NSArray<BUDislikeWords *> *)filterWords {
    //【重要】需要在点击叉以后 在这个回调中移除视图，否则，会出现用户点击叉无效的情况
   [self.expressAdViews removeObject:nativeExpressAdView];
    NSUInteger index = [self.expressAdViews indexOfObject:nativeExpressAdView];
    NSIndexPath *indexPath=[NSIndexPath indexPathForRow:index inSection:0];
   [self.tableView reloadRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationFade]; 
}

- (void)nativeExpressBannerAdViewDidRemoved:(BUNativeExpressBannerView *)nativeExpressAdView {
//【重要】若开发者收到此回调，代表穿山甲会主动关闭掉广告，广告移除后需要开发者对界面进行适配
  [UIView animateWithDuration:0.25 animations:^{
   nativeExpressAdView.alpha = 0;
  } completion:^(BOOL finished) {
   [nativeExpressAdView removeFromSuperview];
    self.bannerView = nil;
  }];
 self.selectedView.promptStatus = BUDPromptStatusDefault;
}


BUMaterialMeta.h

广告推荐说明dislike功能下 “为什么看到此广告”模块，相关API5.1.0.0及后续版本会完全下掉

复制
/// compliance statement of personalized advertising push
@property (nonatomic, strong) BUPersonalizationPrompts *personalPrompts DEPRECATED_MSG_ATTRIBUTE("The 'personalPrompts' api will not work and will be removed in the future.");

BUAdSlot.h

isOriginAd 只有banner广告需要设置YES（5.1.0.0及以上版本）

复制
/// Banner ads are set to YES, other ad types are 0, the default is NO.
@property (nonatomic, assign) BOOL isOriginAd DEPRECATED_MSG_ATTRIBUTE("The 'isOriginAd's is only valid for Banner Ads, no longer required in version 5.1.0.0 and later.");
注意事项

①避免出现先展示广告后请求数据的情况，Banner广告的展示是通过使用addSubview添加广告对象来进行广告展示

②自行实现轮播功能时，建议和平台轮播间隔保持一致，请求间隔为30s～120s之间，避免广告请求过于频繁的情景，轮播间隔为30s～120s之间

③媒体平台选择轮播功能时，客户端进行广告的请求和展示操作即可，后续均由SDK内部实现

④目前banner广告已支持内容底部或顶部居中显示，开发者可自行调整左右边距。

资源

详细接入可参照Demo中的BUDExpressBannerViewController类，广告加载请求部分可参照loadBannerWithSlotID方法，广告展示部分可参照showBanner方法







本篇目录
简介
权限
轮播设置
使用说明
创建广告位对象、请求广告
展示广告
展示时机
接收广告加载结果
dislike回调说明
注意事项
资源
评价此篇文档
有帮助
无帮助

---
Source: https://www.csjplatform.com/supportcenter/5386


# 模版渲染全屏视频广告

开发测试/iOS集成文档/历史功能/模版渲染全屏视频广告
模版渲染全屏视频广告
最近更新 2025-07-25 16:02:40
简介

全屏视频是一种全屏播放的视频广告，用户可以进行0s跳过操作。全屏视频广告的展示场景为应用功能自然体验中断时，如游戏关卡之间。全屏视频广告不应突然出现，不应干扰用户使用应用的正常流程，例如应用加载，退出应用或者游戏过程中。

支持的广告尺寸： 全屏横屏(宽高比16:9)、全屏竖屏(宽高比9:16)

权限

模板渲染全屏视频： 已回收，不再提供创建（该部分面向历史有历史使用该广告类型的开发者）

使用说明

模版全屏视频使用BUNativeExpressFullscreenVideoAd对象调用loadAdData请求广告，使用BUNativeExpressFullscreenVideoAd对象调用showAdFromRootViewController:展示广告，通过设置BUNativeExpressFullscreenVideoAdDelegate代理，获取广告、展示、点击、关闭等回调。

创建广告位对象、请求广告

BUNativeExpressFullscreenVideoAd

请求广告时需要传入广告位对象，广告位对象创建时必须传入广告位id

字段定义

	

必传参数

	

字段名称

	

字段类型

	

备注




SlotID

	

是

	

广告位

	

NSString

	

代码位ID

使用BUNativeExpressFullscreenVideoAd创建对象，使用BUNativeExpressFullscreenVideoAd调用loadAdData请求广告

复制
self.fullscreenAd = [[BUNativeExpressFullscreenVideoAd alloc] initWithSlotID:slotID];
self.fullscreenAd.delegate = self;
[self.fullscreenAd loadAdData];
展示广告

全屏视频广告不应有诸如看视频给金币或奖励之类的激励展示规则，并允许用户在5秒后跳过该广告。开发者不应对用户何时可以跳过广告的设置进行更改，或添加任何其他跳过功能。

调用showAdFromRootViewController:方法展示激励视频广告，此处需要传入当前展示的页面。一定要设置rootViewController，即展示广告和跳转落地页需要的viewController，此处建议开发者在收到nativeExpressFullscreenVideoAdDidDownLoadVideo回调后再允许用户观看广告，可保证播放流畅和展示流畅，用户体验更好。

复制
//show的时候会进行WKWebview的渲染，建议一次最多展示三个广告，如果超过3个会很大概率导致WKWebview渲染失败。当然一般情况下全屏视频一次只会show一个
if (self.fullscreenAd) {
   [self.fullscreenAd showAdFromRootViewController:self];
}
展示时机

在收到nativeExpressFullscreenVideoAdDidDownLoadVideo回调后再允许用户观看广告，可保证播放流畅和展示流畅，用户体验更好。

复制
- (void)nativeExpressFullscreenVideoAdDidDownLoadVideo:(BUNativeExpressFullscreenVideoAd *)fullscreenVideoAd {
//此回调方法后进行广告的展示，可保证播放流畅和展示流畅，用户体验更好。
}
广告展示后重新加载广告

同一次请求的广告最多只能计一次展示，重复的展示会被系统过滤。 因此建议开发者在用户观看完广告后在nativeExpressFullscreenVideoAdDidClose回调里将原来的广告对象置为nil 保证广告对象为新的请求对象

复制
- (void)nativeExpressFullscreenVideoAdDidClose:(BUNativeExpressFullscreenVideoAd *)fullscreenVideoAd {
//在此回调方法中可进行广告的置空操作
}
接收广告加载结果

回调方法

	

注释




nativeExpressFullscreenVideoAd:didFailWithError

	

返回的错误码(error)表示广告加载失败的原因，所有错误码详情请见链接。Link




nativeExpressFullscreenVideoAdViewRenderFail:error

	

渲染失败，网络原因或者硬件原因导致渲染失败,可以更换手机或者网络环境测试。建议升级到穿山甲最新版本SDK




nativeExpressFullscreenVideoAdDidLoad

	

广告素材物料加载成功




nativeExpressFullscreenVideoAdDidDownLoadVideo

	

视频下载完成

BUNativeExpressFullscreenVideoAdDelegate回调说明

回调方法

	

注释




nativeExpressFullscreenVideoAdDidLoad:

	

此回调进入证明广告物料已成功加载




nativeExpressFullscreenVideoAd: didFailWithError:

	

此回调方法中可定位具体的失败原因对应的错误码，打印error即可。所有错误码详情请见链接。Link




nativeExpressFullscreenVideoAdViewRenderSuccess:

	

渲染成功回调方法




nativeExpressFullscreenVideoAdViewRenderFail:error:

	

进入了此回调证明触发了渲染失败条件，可更换手机或者网络环境测试。建议直接升级到穿山甲平台最新版本




nativeExpressFullscreenVideoAdDidDownLoadVideo:

	

在此回调方法中进行广告的展示，可保证播放流畅和展示流畅，用户体验更好。




nativeExpressFullscreenVideoAdWillVisible:

	

模版全屏广告即将展示回调




nativeExpressFullscreenVideoAdDidVisible:

	

模版全屏广告已经展示回调




nativeExpressFullscreenVideoAdDidClick:

	

点击回调




nativeExpressFullscreenVideoAdDidClickSkip:

	

点击5s跳过会触发此回调，如果需要在用户点击跳过时做相关的逻辑处理，可在此回调中进行相关逻辑处理




nativeExpressFullscreenVideoAdWillClose:

	

此回调方法可知用户进行了广告关闭操作，可在此回调方法中进行用户关闭广告时的相应的逻辑处理




nativeExpressFullscreenVideoAdDidClose:

	

点击关闭按钮会触发此回调




nativeExpressFullscreenVideoAdDidPlayFinish:

	

广告播放完成会触发此回调




nativeExpressFullscreenVideoAdCallback:withType:

	

此回调可知模版全屏视频的广告类型




nativeExpressFullscreenVideoAdDidCloseOtherController: interactionType:

	

此回调在广告跳转到其他控制器时，该控制器被关闭时调用interactionType：此参数可区分是打开的appstore/网页/视频广告详情页面

通过设置BUNativeExpressFullscreenVideoAdDelegate代理，获取广告、展示、点击、关闭等回调。其他代理方法可参见demo中BUDExpressFullScreenVideoViewController类#pragma mark - BUFullscreenVideoAdDelegate部分

注意事项

①必须要设置rootViewController，用来处理广告跳转。SDK里所有的跳转均采用present的方式，请确保传入的rootViewController不能为空且没有present其他的控制器，否则会出现presentedViewController已经存在而导致present失败。

②nativeExpressFullscreenVideoAdViewRenderSuccess渲染成功回调在广告展示后返回

③为保证播放流畅和展示流畅，用户体验更好，在收到nativeExpressFullscreenVideoAdDidDownLoadVideo回调后进行广告的展示

④判断广告是否有效直接使用self.fullscreenAd判断即可

⑤个性化模板广告为了优化展示速度,会使用本地模板,请求时会拦截相关数据.如果接入方正在使用H5的页面发送请求,会造成请求body清空,其他逻辑不变.如果使用body传参请更换其他方式.例如:jsBridge方式。

⑥每次请求数据时，都需要重新初始化一个新的BUNativeExpressFullscreenVideoAd对象。请勿重复使用本地缓存的全屏视频对象多次展示.

资源

详细接入可参照Demo中的BUDExpressFullScreenVideoViewController类，广告加载请求部分可参照loadFullscreenVideoAdWithSlotID方法，广告展示部分可参照showFullscreenVideoAd方法




本篇目录
简介
权限
使用说明
创建广告位对象、请求广告
展示广告
展示时机
广告展示后重新加载广告
接收广告加载结果
注意事项
资源
评价此篇文档
有帮助
无帮助

---
Source: https://www.csjplatform.com/supportcenter/5382


# 模版渲染新插屏广告

开发测试/iOS集成文档/模版渲染新插屏广告
模版渲染新插屏广告
最近更新 2025-10-11 16:16:43
简介

模版渲染新插屏广告开发者可以选择广告铺开大小：分为全屏和插屏，全屏和插屏场景下开发者都可以选择投放的广告类型，分别为图片+视频、仅视频、仅图片。

权限

模板渲染新插屏广告： 默认开放

SDK版本要求

双端3550及以上版本SDK

创建参数说明
使用说明

模版渲染新插屏使用BUNativeExpressFullscreenVideoAd对象调用loadAdData请求广告，使用BUNativeExpressFullscreenVideoAd对象调用showAdFromRootViewController:展示广告，通过设置BUNativeExpressFullscreenVideoAdDelegate代理，获取广告、展示、点击、关闭等回调。

创建广告位对象、请求广告

BUNativeExpressFullscreenVideoAd

请求广告时需要传入广告位对象，广告位对象创建时必须传入广告位id

字段定义

	

是否必传

	

字段名称

	

字段类型

	

备注




SlotID

	

是

	

广告位

	

NSString

	

代码位ID

使用BUNativeExpressFullscreenVideoAd创建对象，使用BUNativeExpressFullscreenVideoAd调用loadAdData请求广告

复制
//保证每次请求的广告对象为新的广告对象，避免重复使用广告同一个对象进行广告请求
self.fullscreenAd = [[BUNativeExpressFullscreenVideoAd alloc] initWithSlotID:slotID];
self.fullscreenAd.delegate = self;
[self.fullscreenAd loadAdData];
展示广告

调用showAdFromRootViewController:方法展示新模板插屏广告，此处需要传入当前展示的页面。一定要设置rootViewController，即展示广告和跳转落地页需要的viewController，此处建议开发者在收到nativeExpressFullscreenVideoAdDidDownLoadVideo回调后再去展示广告，可保证播放和展示效果。

复制
//show的时候会进行WKWebview的渲染，建议一次最多展示三个广告，如果超过3个会很大概率导致WKWebview渲染失败。当然一般情况下全屏视频一次只会show一个
if (self.fullscreenAd) {
   [self.fullscreenAd showAdFromRootViewController:self];
}
展示时机

在收到nativeExpressFullscreenVideoAdDidDownLoadVideo回调后再去展示广告，可保证播放和展示效果。

复制
- (void)nativeExpressFullscreenVideoAdDidDownLoadVideo:(BUNativeExpressFullscreenVideoAd *)fullscreenVideoAd {
//回调后再去展示广告，可保证播放和展示效果
}
广告展示后重新加载广告

同一次请求的广告最多只能计一次展示，重复的展示会被系统过滤。 因此建议开发者在用户观看完广告后在nativeExpressFullscreenVideoAdDidClose回调里将原来的广告对象置为nil 保证广告对象为新的请求对象

复制
- (void)nativeExpressFullscreenVideoAdDidClose:(BUNativeExpressFullscreenVideoAd *)fullscreenVideoAd {
//在此回调方法中可进行广告的置空操作
}
接收广告加载结果

回调方法

	

注释




nativeExpressFullscreenVideoAd:didFailWithError

	

返回的错误码(error)表示广告加载失败的原因，所有错误码详情请见链接。Link




nativeExpressFullscreenVideoAdViewRenderFail:error

	

渲染失败，网络原因或者硬件原因导致渲染失败,可以更换手机或者网络环境测试。建议升级到穿山甲最新版本SDK




nativeExpressFullscreenVideoAdDidLoad

	

广告物料加载成功




nativeExpressFullscreenVideoAdDidDownLoadVideo

	

视频/图片下载完成，建议开发者在此回调后进行广告的展示

BUNativeExpressFullscreenVideoAdDelegate回调说明

回调方法

	

注释




nativeExpressFullscreenVideoAdDidLoad:

	

此回调进入证明广告物料已成功加载




nativeExpressFullscreenVideoAd: didFailWithError:

	

此回调方法中可定位具体的失败原因对应的错误码，打印error即可。所有错误码详情请见链接。Link




nativeExpressFullscreenVideoAdViewRenderSuccess:

	

渲染成功回调方法




nativeExpressFullscreenVideoAdViewRenderFail:error:

	

进入了此回调证明触发了渲染失败条件，可更换手机或者网络环境测试。建议直接升级到穿山甲平台最新版本




nativeExpressFullscreenVideoAdDidDownLoadVideo:

	

在此回调方法中进行广告的展示，可保证播放流畅和展示流畅，用户体验更好。




nativeExpressFullscreenVideoAdWillVisible:

	

新模版插屏广告即将展示回调




nativeExpressFullscreenVideoAdDidVisible:

	

新模版插屏广告已经展示回调




nativeExpressFullscreenVideoAdDidClick:

	

点击回调




nativeExpressFullscreenVideoAdDidClickSkip:

	

点击跳过会触发此回调，如果需要在用户点击跳过时做相关的逻辑处理，可在此回调中进行处理




nativeExpressFullscreenVideoAdWillClose:

	

此回调方法可知用户进行了广告关闭操作，可在此回调方法中进行用户关闭广告时的相应的逻辑处理




nativeExpressFullscreenVideoAdDidClose:

	

点击关闭按钮会触发此回调




nativeExpressFullscreenVideoAdDidPlayFinish:

	

广告播放完成会触发此回调




nativeExpressFullscreenVideoAdCallback:withType:

	

此回调可知新模版插屏的广告类型




nativeExpressFullscreenVideoAdDidCloseOtherController: interactionType:

	

此回调在广告跳转到其他控制器时，该控制器被关闭时调用interactionType：此参数可区分是打开的appstore/网页/视频广告详情页面

通过设置BUNativeExpressFullscreenVideoAdDelegate代理，获取广告、展示、点击、关闭等回调。其他代理方法可参见demo中BUDExpressFullScreenVideoViewController类#pragma mark - BUFullscreenVideoAdDelegate部分

注意事项

①必须要设置rootViewController，用来处理广告跳转。SDK里所有的跳转均采用present的方式，请确保传入的rootViewController不能为空且没有present其他的控制器，否则会出现presentedViewController已经存在而导致present失败。

②nativeExpressFullscreenVideoAdViewRenderSuccess渲染成功回调在广告展示后返回

③为保证播放流畅和展示流畅，用户体验更好，在收到nativeExpressFullscreenVideoAdDidDownLoadVideo回调后进行广告的展示

④判断广告是否有效直接使用self.fullscreenAd判断即可

⑤个性化模板广告为了优化展示速度,会使用本地模板,请求时会拦截相关数据.如果接入方正在使用H5的页面发送请求,会造成请求body清空,其他逻辑不变.如果使用body传参请更换其他方式.例如:jsBridge方式。

⑥每次请求数据时，都需要重新初始化一个新的BUNativeExpressFullscreenVideoAd对象。请勿重复使用本地缓存的全屏视频对象多次展示.

资源

详细接入可参照Demo中的BUDExpressFullScreenVideoViewController类，广告加载请求部分可参照loadFullscreenVideoAdWithSlotID方法，广告展示部分可参照showFullscreenVideoAd方法







本篇目录
简介
权限
SDK版本要求
创建参数说明
使用说明
创建广告位对象、请求广告
展示广告
展示时机
广告展示后重新加载广告
接收广告加载结果
注意事项
资源
评价此篇文档
有帮助
无帮助

---
Source: https://www.csjplatform.com/supportcenter/5383


# 隐私数据获取说明

开发测试/iOS集成文档/隐私数据获取说明
隐私数据获取说明
最近更新 2023-02-17 10:53:09

以下用户数据由穿山甲 iOS SDK收集，开发者在回答app Store Connect上的应用隐私问题时可以参考此图表。

访问https://developer.apple.com/app-store/app-privacy-details/了解更多信息。

1. 收集的数据类型

数据类型

	

穿山甲收集情况

	

备注




联系信息 Contact Info 姓名 Name 电子邮件地址 Email Address 电话号码 Phone Number 实际地址 Physical Address 其他用户联系信息 Other User Contact Info

	

不收集

	







健康与健身 Health and Fitness 健康 Health 健身 Fitness

	

不收集

	







财务信息 Financial Info 付款信息 Payment Info 信用信息 Credit Info 其他财务信息 Other Financial Info

	

不收集

	







位置 Location 精确位置




Precise Location 粗略位置 Coarse Location

	

精确位置-可选收集

粗略位置-不收集

	

穿山甲会获取地理位置信息用于广告投放与反作弊。应用被用户授予地理位置权限时，穿山甲会获取地理位置信息，用于广告定向与反作弊；当应用不获取地理位置权限时，穿山甲不会主动获取地理位置权限及地理位置信息。




敏感信息 Sensitive Info

	

不收集

	







联系人 Contacts

	

不收集

	







用户内容 User Content 电子邮件或短信内容 Emails or Text Messages 照片或视频 Photos or Videos 音频数据 Audio Data 游戏内容 Gameplay Content 客户支持 Customer Support 其他用户内容 Other User Content

	

不收集

	







浏览历史记录 Browsing History

	

不收集

	







搜索历史记录 Search History

	

不收集

	







标识符 Identifiers 用户ID User ID 设备ID Device ID

	

可选收集

	

用户ID仅用于功能-奖励视频服务器端验证回调。开发者在广告请求期间向穿山甲发送他们的用户名（基本上是一系列数字，穿山甲不知道它与该应用内的内部用户id或内部用户名的关系。穿山甲只接收这些数据，不检查、不验证、不存储、不使用）。当用户完全观看奖励视频时，穿山甲将把这个用户id发送回开发者的服务器。如果开发者不使用此功能，则开发者不需要传递此类数据。设备ID 当应用被用户授予广告追踪权限时，穿山甲将获取idfa用于广告归因与追踪。




购买项目 Purchases 购物历史 Purchase History

	

不收集

	







使用数据 Usage Data 产品交互 Product Interaction




广告数据 Advertising Data




其他使用数据 Other Usage Data

	

Usage Data-不收集

广告数据-收集




其他使用数据- 不收集

	

穿山甲广告将统计下列广告数据，以用于广告主统计投放结果。展示 、点击 、转化




诊断 Diagnostics 崩溃数据




Crash Data 性能数据




Performance Data 其他诊断数据 Other Diagnostic Data

	

崩溃数据-收集




性能数据-收集




其他诊断数据-不收集

	

崩溃数据 穿山甲将收集穿山甲SDK带来的崩溃信息，以此来优化代码缺陷。 性能数据 穿山甲将收集SDK运行过程中性能数据，以优化穿山甲SDK的性能。




其他数据 Other Data 其他数据类型 Other Data Types

	

收集

	

技术上我们还会收集一些设备信息（例如，设备型号、操作系统及版本、时区、网络类型等）

2. 数据使用目的

苹果明示了一系列的数据使用目的，需要开发者根据App中数据的使用情况进行披露。如下表所示，穿山甲依据自身功能，列举了数据的使用目的，以供参考。

基于媒体与穿山甲的合作目的，穿山甲收集数据的主要目的是用于“第三方广告”、“APP功能”（如流量变现的功能）；其他使用目的基于媒体自己的使用情况而定。

使用目的

	

基于接入穿山甲的功能，选择数据的使用目的




第三方广告Third-Party Advertising

	

是




开发者广告或营销内容Developer’s Advertising or Marketing

	

如您的App使用数据用于显示应用中的第一方广告（包括与第三方共享以显示广告）或其他营销目的是，请选择 是




分析Analytics

	

广告数据会用于分析，请选择 是




产品个性化 Product Personalization

	

如您的App将数据用于任何产品个性化功能，请选择 是




App 功能App Functionality

	

是




其他用途 Other Purposes

	

如您的App出于其他目的使用数据，请选择 是

3. 与用户身份的关联性

通过穿山甲收集的数据类型通常连接到诸如设备ID（如果可用）之类的标识符，或者与其他标识符（如配置时由客户发布的ID）相连。因此，在询问数据是否链接到身份时，应用程序开发人员应选择 是。

4. 关于用户追踪

根据苹果对追踪的定义，当应用被用户授予广告追踪权限时，穿山甲将获取IDFA用于广告归因与追踪。







本篇目录
1. 收集的数据类型
2. 数据使用目的
3. 与用户身份的关联性
4. 关于用户追踪
评价此篇文档
有帮助
无帮助

---
Source: https://www.csjplatform.com/supportcenter/5374


# 支持微信小程序/小游戏跳转接入方案

开发测试/iOS集成文档/支持微信小程序/小游戏跳转接入方案
支持微信小程序/小游戏跳转接入方案
最近更新 2025-04-18 18:40:12
简介

微信小程序/小游戏广告，是指用户点击广告后，将跳转至微信打开微信小程序/小游戏，在微信内部发生后续的行为转化。

整体接入流程

1.进入微信开放平台创建移动应用并获取到相应的微信AppID；

2.在移动端嵌入最新版OpenSDK（仅嵌入即可，无需额外开发工作），并确认版本为 1.8.6 以上；

3.在穿山甲开发者平台，将微信开放平台填写的AppID与当前应用进行关联；注意：iOS与Android应用的不同之处在于需要额外填写universial_link；

4.嵌入更新穿山甲SDK到4800及以上版本。

详细步骤

1.在【微信开放平台】创建移动应用；

2.应用创建完成后，在【微信开放平台】获取相应的AppID；

3.登录【穿山甲媒体平台】，点击【广告变现】模块下的【流量】选择【应用】模块，找到需要关联App ID的应用，点击“去接入微信小游戏预算；

4.点击【编辑】进入到编辑应用信息页面，操作微信AppID的关联及Universial_link的填写，详情可参照如下图示：

5.在开发者APP内嵌入穿山甲SDK版本4800及以上，并集成1.8.6 以上OpenSDK版本；

6.开发者仅需绑定微信AppID及universial_link，无需额外开发工作，后续逻辑由穿山甲SDK完成。




本篇目录
简介
整体接入流程
详细步骤
评价此篇文档
有帮助
无帮助

---
Source: https://www.csjplatform.com/supportcenter/26768


---
Source: output/Pangle-iOS-SDK_data/pages/自渲染基础模块配置说明.json

# Pangle iOS SDK: Core Components for Self-Rendering Ads

Reviewed the core configuration documentation for the Pangle iOS SDK's self-rendering module. The implementation workflow starts with configuring a `BUAdSlot` object to define the ad placement. This slot is then used with the `BUNativeAd` class to load ad data. Upon a successful load, the ad's metadata and creative assets are available in a `BUMaterialMeta` object. Developers can then render this data into a custom UI. For the SDK to track clicks and handle interactions (like opening a URL or the App Store), the custom view and its clickable subviews must be registered. The SDK also provides helper classes like `BUNativeAdRelatedView` to simplify adding common ad elements and `BUVideoAdView` for managing video playback.

**Key Facts:**
- The `BUAdSlot` class is used to configure ad placement details like ID, ad type, and image sizes, and is a required parameter for any ad request.
- `BUNativeAd` is the primary class for requesting a single native ad and managing its lifecycle and interactions.
- All creative and metadata for an ad are encapsulated in the `BUMaterialMeta` object, which is accessed after a successful ad load.
- It is mandatory to register the ad's container view and all its clickable elements with the `BUNativeAd` instance using `registerContainer:withClickableViews:` to enable SDK-handled clicks.
- `BUNativeAdRelatedView` is a helper class for easily adding standard UI components like the ad logo, dislike button, or video view.
- The `BUVideoAdView` class provides specific controls (play, pause) and delegate callbacks for managing video ad playback.
- The `BUDislike` class allows for a custom implementation of the "dislike ad" feature, but requires that the user's reason is reported back to the SDK.

---
Source: output/Pangle-iOS-SDK_data/pages/自渲染Draw广告.json

# Pangle iOS SDK: Self-Rendering Draw Ads

The documentation for "Self-rendering Draw Ads" in the Pangle iOS SDK was reviewed. This investigation revealed that this ad type is a legacy feature, no longer available for new integrations, and is maintained for developers with historical usage. The implementation involves using the `BUNativeAdsManager` to request ads, with a `BUAdSlot` object configured with `AdType` set to `BUAdSlotAdTypeDrawVideo`. The `BUNativeAdRelatedView`'s `videoAdView` offers specific customizations for Draw ads, such as setting a custom pause button icon and enabling or disabling the click-to-pause functionality. The `BUDDrawVideoViewController` class within the demo application is the recommended resource for a detailed implementation example.

**Key Facts:**
- Self-rendering Draw video ads are a retired feature, no longer available for new creation, and documentation is for historical users.
- The BUNativeAdsManager class is used to load ad data for Draw ads.
- The BUAdSlot object must be configured with the AdType property set to BUAdSlotAdTypeDrawVideo.
- The demo class `BUDDrawVideoViewController` serves as a reference implementation for this ad type.
- Customization options include setting the pause icon and size via `playerPlayIncon:playInconSize:`.
- The `drawVideoClickEnable` property on `videoAdView` controls whether clicking the video pauses it.

---
Source: output/Pangle-iOS-SDK_data/pages/自渲染激励视频广告.json

# Pangle iOS SDK: SDK-Rendered Rewarded Video Ads

Reviewed the documentation for what is titled "Self-Rendering Rewarded Video Ads," but is in fact an SDK-rendered format. This ad type presents a full-screen video that users can watch to earn a reward. The implementation relies on the `BURewardedVideoAd` class. The correct flow is to instantiate this class, load the ad data, and critically, wait for the `rewardedVideoAdVideoDidLoad:` callback before presenting the ad to the user. This ensures the video is fully buffered and plays smoothly. The system supports both client-side and server-to-server reward verification. It's a strict requirement to create a new ad object for every new ad request to prevent issues with stale ads. The SDK also offers an advanced rewards system for more complex reward logic.

**Key Facts:**
- Rewarded video is a full-screen ad format, available in both horizontal and vertical orientations.
- Unlike other self-rendering types, this ad is implemented using the `BURewardedVideoAd` class, not `BUNativeAd`.
- A new `BURewardedVideoAd` object must be created for each ad request; reusing old instances is not permitted.
- For the best user experience, the ad should only be shown after the `rewardedVideoAdVideoDidLoad:` delegate method is called, confirming the video is fully downloaded.
- Reward validation can be handled either on the client-side or through a server-to-server callback for which a `userId` is required.
- The SDK supports an "advanced rewards" feature, allowing for different reward amounts based on interaction or full video completion.
- The `BUDRewardedVideoAdViewController` class in the demo project serves as the reference implementation.

---
Source: output/Pangle-iOS-SDK_data/pages/自渲染插屏广告.json

# Pangle iOS SDK: Self-Rendering Interstitial Ads

The documentation for self-rendering interstitial ads in the Pangle iOS SDK was analyzed. This ad format is a legacy feature that has been retired and is no longer available for new creation. Implementation for historical users involves creating a `BUNativeAd` instance, which takes a `BUAdSlot` object for configuration. This slot object must specify the ad type as `BUAdSlotAdTypeInterstitial`. The `BUNativeAdDelegate` is crucial for handling the ad lifecycle, including successful data loading, failures, and user interaction events like clicks. Once ad data is received in the `nativeAdDidLoad:` callback, it's necessary to populate the custom UI and then call `registerContainer:withClickableViews:` to enable ad interactivity. The demo class `BUDNativeInterstitialViewController` is cited as the primary example for integration.

**Key Facts:**
- Self-rendering interstitial ads have been retired and are no longer available for new integrations; the documentation serves historical users.
- Ad requests are made using a `BUNativeAd` object, which is initialized with a `BUAdSlot` configuration.
- The `BUAdSlot` must be configured with `AdType` set to `BUAdSlotAdTypeInterstitial`.
- The `BUNativeAdDelegate` protocol is used to receive callbacks for ad load success (`nativeAdDidLoad:`), failure (`nativeAd:didFailWithError:`), and user interactions.
- After ad data is loaded, `registerContainer:withClickableViews:` must be called to bind the ad's interactive elements.
- The `BUDNativeInterstitialViewController` class in the demo project provides a reference implementation.
