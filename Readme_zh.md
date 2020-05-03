**[English](README.md) | 简体中文**

<h1 align="center"><a href="" target="_blank" rel="noopener noreferrer"><img width="250" src="https://cdnbye.oss-cn-beijing.aliyuncs.com/pic/cdnbye.png" alt="cdnbye logo"></a></h1>
<h4 align="center">Save Your Bandwidth using WebRTC.</h4>
<h4 align="center">视频网站省流量&加速神器.</h4>
<p align="center">
    <a href="https://www.npmjs.com/package/cdnbye-dash"><img src="https://img.shields.io/npm/v/cdnbye-dash.svg?style=flat" alt="npm"></a>
    <a href="https://www.jsdelivr.com/package/npm/cdnbye-dash"><img src="https://data.jsdelivr.com/v1/package/npm/cdnbye-dash/badge" alt="jsdelivr"></a>
    <a href="https://github.com/cdnbye/dashjs-p2p-engine/tree/master/dist"><img src="https://badge-size.herokuapp.com/cdnbye/dashjs-p2p-engine/master/dist/dashjs-p2p-engine.min.js?compression=gzip&style=flat-square" alt="size"></a>
</p>

P2P技术使观看相同内容的用户之间可以相互分享数据，不仅能效降低视频/直播网站的带宽成本，还可以提升用户的播放体验，降低卡顿、二次缓存的发生率。
另外，随着H5的普及，flash逐渐被淘汰已成为不可逆转的趋势。而在H5采用的视频传输格式中，dash由于兼容ios和android、可以穿过任何允许HTTP数据通过的防火墙、容易使用内容分发网络来传输媒体流和码率自适应等众多优势而在业界得到广泛使用。通过使用[dash.js](https://github.com/Dash-Industry-Forum/dash.js)这个第三方库，几乎所有现代浏览器都可以播放dash视频。dash天生分片传输的优势，使其可以采用p2p的方式进行传输，从而减小服务器的负担。在web端，无插件化实现p2p传输能力的最好选择就是[WebRTC](https://zh.wikipedia.org/wiki/WebRTC)技术，与dash.js类似，WebRTC也支持几乎所有现代浏览器。本项目是一个dash.js的插件，通过WebRTC datachannel技术，在不影响用户体验的前提下，最大化p2p率，是面向未来的Web P2P技术。
<br>想知道P2P效果如何吗，登录[后台管理系统](https://oms.cdnbye.com)并绑定域名就可以查看啦！

该插件的优势如下：
- 浏览器原生支持，不需要安装任何插件，采用仿BT算法，在线人数越多效果越好
- 支持基于MPEG-dash流媒体协议的直播和点播场景
- 不改动dash.js源码，并且可以与其无缝衔接，几行代码集成，便于在现有项目中快速集成
- 浏览器不支持WebRTC时无缝切换到HTTP下载模式
- 高可配置化，用户可以根据特定的使用环境调整各个参数
- 支持video.js、Clappr、Flowplayer、DPlayer等第三方播放器
- 通过有效的调度策略来保证用户的播放体验以及p2p率
- Tracker服务器根据访问IP的ISP、地域等进行智能调度
- API已经固化，新版本完全兼容旧版本代码

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

## 使用方法
```html
<!doctype html>
<html>
    <head>
        <title>Dash with P2P</title>
        <style>
            video {
                width: 640px;
                height: 360px;
            }
        </style>
    </head>
    <body>
        <div>
            <video id="videoPlayer" controls></video>
        </div>
        <script src="yourPathToDash/dash.all.min.js"></script>
        <script src="https://cdn.jsdelivr.net/npm/cdnbye-dash@latest"></script>
        <script>
            (function(){
                var url = "https://dash.akamaized.net/envivio/EnvivioDash3/manifest.mpd";
                var player = dashjs.MediaPlayer().create();
                new P2PEngine(player);
                player.initialize(document.querySelector("#videoPlayer"), url, true);
            })();
        </script>
    </body>
</html>
```


## API文档
### 实例化与参数配置
#### `var engine = new P2PEngine(player, {p2pConfig: [opts]});`  
创建一个新的`P2PEngine`实例，其中 player 是`dashjs`的MediaPlayer实例。

如果指定了`opts`，那么对应的默认值将会被覆盖。

| 字段 | 类型 | 默认值 | 描述 |
| :-: | :-: | :-: | :-: |
| `logLevel` | string or boolean | 'none' | log的等级，分为warn、error、none，设为true等于warn，设为false等于none。
| `live` | boolean | false | 直播或者点播模式，建议在点播模式下设为false，p2p插件会预缓存buffer以避免卡顿。
| `wsSignalerAddr` | string | 'wss://signal.cdnbye.com' | 信令服务器地址。
| `announce` | string | 'https://tracker.cdnbye.com/v1' | tracker服务器地址。
| `wsMaxRetries` | number | 15 |websocket连接重试次数。
| `wsReconnectInterval` | number | 30 | websocket重连时间间隔。
| `memoryCacheLimit` | Object | {"pc": 1024 * 1024 * 512, "mobile": 1024 * 1024 * 256} | p2p缓存的最大数据量，分为PC和mobile。
| `p2pEnabled` | boolean | true | 是否开启P2P。
| `dcDownloadTimeout` | number | 25 | p2p下载的最大超时时间。
| `webRTCConfig` | Object | {} | 用于配置stun和datachannel的[字典](https://github.com/feross/simple-peer)。
| `useHttpRange` | boolean | false | 在可能的情况下使用Http Range请求来补足p2p下载超时的剩余部分数据（直播模式下默认是true）。
| `getStats` | function | - | 获取p2p统计信息，包括totalP2PDownloaded、totalP2PUploaded和totalHTTPDownloaded。
| `getPeerId` | function | - | 获取本节点的Id，当从服务端获取到peerId时回调该事件。
| `getPeersInfo` | function | - | 获取成功连接的节点的信息，当与新的节点成功建立p2p连接时回调该事件。
| `channelId` | function | - | 标识channel的字段，同一个channel的用户可以共享数据。（参考高级用法）
| `validateSegment` | function | - | 用于校验从其它节点下载的ts文件的合法性。

### P2PEngine API

#### `P2PEngine.version` (static method)
获取`P2PEngine`的版本号。

#### `P2PEngine.isSupported()` (static method)
判断当前浏览器是否支持WebRTC datachannel。

#### `engine.enableP2P()`
在p2p暂停或未启动情况下启动p2p。

#### `engine.disableP2P()`
停止p2p并释放内存。

#### `engine.destroy()`
停止p2p、销毁engine并释放内存。

### P2PEngine事件

#### `engine.on('peerId', function (peerId) {})`
当从服务端获取到peerId时回调该事件。

#### `engine.on('peers', function (peers) {})`
当与新的节点成功建立p2p连接时回调该事件。

####`engine.on('stats', function (stats) {})`
该回调函数可以获取p2p信息，包括：</br>
stats.totalHTTPDownloaded: 从HTTP(CDN)下载的数据量（单位KB）</br>
stats.totalP2PDownloaded: 从P2P下载的数据量（单位KB）</br>
stats.totalP2PUploaded: P2P上传的数据量（单位KB）

### 高级用法
#### 通过向p2pConfig传入getStats获取p2p下载信息
```javascript
p2pConfig: {
    getStats: function (totalP2PDownloaded, totalP2PUploaded, totalHTTPDownloaded) {
        // do something
    }
}
```

#### 通过向p2pConfig传入getPeerId获取本节点的Id
```javascript
p2pConfig: {
    getPeerId: function (peerId) {
        // do something
    }
}
```

#### 通过向p2pConfig传入getPeersInfo获取成功连接的节点的信息
```javascript
p2pConfig: {
    getPeersInfo: function (peers) {
        // do something
    }
}
```

#### 解决动态 MPD 路径问题
某些流媒体提供商的 MPD 是动态生成的，不同节点的mpd地址不一样，例如example.com/clientId1/file.mpd和example.com/clientId2/file.mpd,
而本插件默认使用mpd作为channelId。这时候就要构造一个共同的chanelId，使实际观看同一直播/视频的节点处在相同频道中。`强烈建议在chanelId中加入唯一标识符，防止与其他频道产生冲突。`
```javascript
p2pConfig: {
    channelId: function (mpdUrl) {
        const formatedUrl = 'YOUR_UNIQUE_ID' + format(mpdUrl);   // 忽略差异部分，构造一个一致的channelId
        return formatedUrl;
    }
}
```

#### 配置STUN服务器
```javascript
p2pConfig: {
    webRTCConfig: { 
        config: {         // custom webrtc configuration (used by RTCPeerConnection constructor)
            iceServers: [
                { urls: 'stun:stun.l.google.com:19302' }, 
                { urls: 'stun:global.stun.twilio.com:3478?transport=udp' }
            ] 
        }
    }
}
```

#### 允许Http Range请求
当对等端上行带宽不够时，可能导致p2p传输超时而转向http下载，原本p2p下载的数据无法复用。Http Range请求用于补足p2p下载超时的剩余部分数据，要开启Http Range，首先需要源服务器支持，然后增加以下配置：
```javascript
p2pConfig: {
    useHttpRange: true,
}
```

## 后台管理系统
在接入P2P插件后，访问`https://oms.cdnbye.com`，注册并绑定域名，即可查看该域名的P2P流量、在线人数、用户地理分布等信息。

## 客户案例
[<img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1531253035445&di=7af6cc9ad4abe3d06ba376af22d85131&imgtype=0&src=http%3A%2F%2Fimg.kuai8.com%2Fattaches%2Fintro%2F1213%2F201612131436417407.png" width="120">](https://egame.qq.com/?hls=1&p2p=1&_debug=1)

## 相关项目
- [hlsjs-p2p-engine](https://gitee.com/cdnbye/hlsjs-p2p-engine) - HLS协议的Web端P2P流媒体方案。

## FAQ
我们收集了一些[常见问题](https://www.cdnbye.com/views/FAQ.html)。在报告issue之前请先查看一下。

## 联系我们
邮箱：service@cdnbye.com

