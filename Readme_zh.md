**[English](README.md) | 简体中文**

<h1 align="center"><a href="" target="_blank" rel="noopener noreferrer"><img width="250" src="https://www.cdnbye.com/logo.png" alt="cdnbye logo"></a></h1>
<h4 align="center">Save Your Bandwidth using WebRTC.</h4>
<h4 align="center">视频网站省流量&加速神器.</h4>
<p align="center">
    <a href="https://www.npmjs.com/package/cdnbye-dash"><img src="https://img.shields.io/npm/v/cdnbye-dash.svg?style=flat" alt="npm"></a>
    <a href="https://www.jsdelivr.com/package/npm/cdnbye-dash"><img src="https://data.jsdelivr.com/v1/package/npm/cdnbye-dash/badge" alt="jsdelivr"></a>
</p>

P2P技术使观看相同内容的用户之间可以相互分享数据，不仅能效降低视频/直播网站的带宽成本，还可以提升用户的播放体验，降低卡顿、二次缓存的发生率。
另外，随着H5的普及，flash逐渐被淘汰已成为不可逆转的趋势。而在H5采用的视频传输格式中，dash由于兼容ios和android、可以穿过任何允许HTTP数据通过的防火墙、容易使用内容分发网络来传输媒体流和码率自适应等众多优势而在业界得到广泛使用。通过使用[dash.js](https://github.com/Dash-Industry-Forum/dash.js)这个第三方库，几乎所有现代浏览器都可以播放dash视频。dash天生分片传输的优势，使其可以采用p2p的方式进行传输，从而减小服务器的负担。在web端，无插件化实现p2p传输能力的最好选择就是[WebRTC](https://zh.wikipedia.org/wiki/WebRTC)技术，与dash.js类似，WebRTC也支持几乎所有现代浏览器。本项目是一个dash.js的插件，通过WebRTC datachannel技术，在不影响用户体验的前提下，最大化p2p率，是面向未来的Web P2P技术。

该插件的优势如下：
- 浏览器原生支持，不需要安装任何插件，采用仿BT算法，在线人数越多效果越好
- 支持基于MPEG-dash流媒体协议的直播和点播场景
- 不改动dash.js源码，并且可以与其无缝衔接，几行代码集成，便于在现有项目中快速集成
- 浏览器不支持WebRTC时无缝切换到HTTP下载模式
- 高可配置化，用户可以根据特定的使用环境调整各个参数
- 通过有效的调度策略来保证用户的播放体验以及p2p率
- Tracker服务器根据访问IP的ISP、地域等进行智能调度

## 快速入门
将[quick-start.html](demo/quick-start.html)拷贝到您的网页中并运行。再打开另一个相同的网页。见证奇迹的时候到了！您已在两个网页之间建立了一个P2P连接，在不安装任何插件的情况下。如果在这个频道中（一个mpd标识了一个频道）没有其它参与者，那么您打开的第一个网页将作为种子为第二个网页提供数据。

## 浏览器支持情况
由于WebRTC已成为HTML5标准，目前大部分主流浏览器都已经支持。CDNBye的浏览器兼容性取决于WebRTC和dash.js。需要注意的是iOS版Safari由于不支持MediaSource API，因此也不支持dash.js。

 兼容性|Chrome | Firefox | macOS Safari| 安卓微信/QQ | Opera | Edge | IE | iOS Safari | 
:-: | :-: | :-: | :-: | :-: | :-: | :-:| :-:| :-:
WebRTC Datachannel | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
dash.js | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ❌ |
CDNBye | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ❌ | ❌ | 

## 集成
通过script标签引入最新版本：
```html
<script src="https://cdn.jsdelivr.net/npm/cdnbye-dash@latest"></script>
```

## API文档
参见 [API.md](https://www.cdnbye.com/cn/dashjs/API.html)

## 后台管理系统
在接入P2P插件后，访问`https://www.cdnbye.com/oms`，注册并绑定域名，即可查看该域名的P2P流量、在线人数、用户地理分布等信息。

## 相关项目
- [shaka-p2p-engine](https://github.com/cdnbye/shaka-p2p-engine) - 同时支持HLS和Mpeg-Dash格式。
- [hlsjs-p2p-engine](https://gitee.com/cdnbye/hlsjs-p2p-engine) - HLS协议的Web端P2P流媒体方案。
- [mp4-p2p-engine](https://github.com/cdnbye/mp4-p2p-engine) - 支持MP4的Web端P2P流媒体方案。

## FAQ
我们收集了一些[常见问题](https://www.cdnbye.com/faq.html)。在报告issue之前请先查看一下。

## 联系我们
邮箱：service@cdnbye.com

