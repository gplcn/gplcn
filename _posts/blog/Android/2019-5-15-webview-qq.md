---

layout: post
title: QQ Hybrid 架构优化演进经验
categories: Android
description: WebView 优化
keywords: WebView, Android，Hybrid

---
# QQ Hybrid 架构优化演进经验

## 传统的静态页面加载过程
用户从click开始，到launch WebView，WebView去加载CDN上的HTML文件，页面loading起来后才会去获取JSON，为了加速这个过程可能会用到localStroage做缓存；

* webview创建网络空等待
	* launch WebView的时候网络处于空等状态，这会浪费时间，如果WebView生存在另一个进程内部， launch一次WebView除了进程loading还有浏览器内核的加载；
*  HTML不包含展示内容
	* 发布在CDN上的静态页面内部不包含item数据，所以用户第一眼看到从CDN下载的页面，里面的banner区域和item区域处于一片空白，这对用户体验也是很大的伤害  
* 动态构造DOM时间开销
	* 页面loading起来要refresh当前的DOM，即拉取JSON之后拼接DOM结构再refresh，我们发现在一些QQ用户所使用的低端Android机器里，这个执行也会非常消耗时间。

### 静态直出+离线预推的模式。

首先我们把WebView的加载和网络请求做了并行，所有的网络请求并不是从WebView内核发起request，而是loading WebView的过程中，我们通过native的渠道建立自己的HTTP链接，然后从CDN和我们称作offlineServer的地方获取页面，这个offlineServer也就是大家听说过的***离线包缓存策略***。

我们在native会有offlineCache，发起HTTP请求的时候首先检查offlineCache里有没有当前HTML缓存，这个缓存和WebView的缓存是隔离的，不会受到WebView的缓存策略影响，完全由我们自控。

如果offlineCache没有缓存才会去offlineServer去同步文件，同时也会去从CDN去下载更新。我们在CDN上存储的HTML已经把banner和item等所有的数据打在静态页面里，这个时候WebView只要拿到HTML就不需要再做refresh和执行任何JS，整个页面可以直接展示出来，用户也可以进行交互

#### 统一数据
对静态直出这种模式做了小型的***自动构建***系统，产品经理在管理端配置数据要同步dataServer时，我们会立刻启动我们内部称为vnues的构建系统。

这套系统是基于Node.js搭建的，会把开发所编写的代码文件和UI素材图片等等数据实时生成最新版本的HTML，然后发布到CDN以及同步到offlineServer上，这可以解决CDN的文件与最新数据不一致的问题。

* offlineServer内部分为流控和offline计算两部分。当一个页面的所有资源需要进行离线包计算打包的时候，offline计算这部分除了把所有的资源打包，内部也会存储之前所有的历史版本，同时根据历史版本和最新版本生成所有的diff，即每个***离线包的差样***部分。

用户登录后，每次都会询问offline流控server看有没有最新的包可以下载，如果当前流控server统计的带宽在可接受的成本（目前暂定为10GB到20GB的空间），当CDN的带宽撑得住的时候就会把最新的diff下发给客户端，这样就做到离线包一有更新时客户端能以最小的流量代价得到刷新

### 动态直出 动态缓存 减少数据传输

不让WebView直接访问我们的Node.js服务器，我们在这中间加上之前提到地类似offlineCache的中间层sonicBridge
我们***改变了Node.js组HTML的协议***，当sonicBridge在第二次请求数据的时候，Node.js服务器并不会返回整个HTML给sonicBridge，而是返回给我们称为data数据的部分。

拿到data数据之后，我们和H5页面做了约定，由native侧调用页面的固定刷新函数，并传递数据给页面。页面会去局部刷新自己的DOM节点，这样即使页面需要刷新也不会reload整个页面。

核心

* HTML分模版和数据进行缓存


## 优化传输数据办法
我们发现用户的不同机型会存在流量浪费的情况。我们的UI设计通常都是针对iPhone6的屏幕尺寸做的，默认是750px的图片素材。小屏幕的手机，比如640px和480px，同样是下载750px的图片，然后在渲染的时候进行缩小，这样实际浪费了非常大的带宽，所以我们思考CDN是否能根据用户手机屏幕尺寸来下发不同格式的图片。

* WebView会自动带上终端的屏幕尺寸以及支持哪些图片格式给CDN节点，CDN节点再从源站获取最新的图片，源站这个时候有可能已经离线或实时生成好对应的图片了。

## 问题
中国不同地区运营商之间，会做类似CDN Cache的缓存服务。当Android用户第一次请求sharpP图片的时候，运营商的server从我们的CDN拿到了sharpP格式链接。当缓存生效期间内，同一个地区其他iOS用户上来请求时，运营商发现URL一样，直接就把sharpP格式的图片返回给iOS用户。

***在CDN分发内容的时候，通过Vary字段指定缓存的时候要去参考Accept和User-Agent里的字段，我们把这个Vary加上之后问题基本解决了***


## 经验
做运营监控工具，有运营监控系统，能放心大胆地修改页面和发布新功能，同时保证稳定和可靠。

许多调试能力已经提前部署在所有手机QQ终端。我们可以通过远程命令去检查用户的DNS解析情况，命中了哪台server，用户是否受到运营商劫持等等（***写脚本实现***）。

[原文](https://mp.weixin.qq.com/s/evzDnTsHrAr2b9jcevwBzA?)