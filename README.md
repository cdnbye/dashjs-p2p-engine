**English | [简体中文](Readme_zh.md)**

<h1 align="center"><a href="" target="_blank" rel="noopener noreferrer"><img width="250" src="https://cdnbye.oss-cn-beijing.aliyuncs.com/pic/cdnbye.png" alt="cdnbye logo"></a></h1>
<h4 align="center">Save Your Bandwidth using WebRTC.</h4>
<p align="center">
  <a href="https://www.npmjs.com/package/cdnbye-dash"><img src="https://img.shields.io/npm/v/cdnbye-dash.svg?style=flat" alt="npm"></a>
  <a href="https://www.jsdelivr.com/package/npm/cdnbye-dash"><img src="https://data.jsdelivr.com/v1/package/npm/cdnbye-dash/badge" alt="jsdelivr"></a>
  <a href="https://github.com/cdnbye/dashjs-p2p-engine/tree/master/dist"><img src="https://badge-size.herokuapp.com/cdnbye/dashjs-p2p-engine/master/dist/dashjs-p2p-engine.min.js?compression=gzip&style=flat-square" alt="size"></a>
</p>


CDNBye dashjs-p2p-engine implements [WebRTC](https://en.wikipedia.org/wiki/WebRTC) datachannel to scale live/vod video streaming by peer-to-peer network using bittorrent-like protocol. The forming peer network can be layed over other CDNs or on top of the origin server. Powered by [dash.js](https://github.com/Dash-Industry-Forum/dash.js), it can play MPEG-dash on any platform with many popular HTML5 players such as video.js, JWPlayer and Flowplayer. BTW, you can view how much traffic has been saved [here](https://oms.cdnbye.com)!
## Features
- WebRTC data channels for lightweight peer-to-peer communication with no plugins
- Support live and VOD streams over MPEG-dash protocol
- Very easy to integrate with an existing dash.js project
- Seamlessly fallback to normal server usage if a browser doesn't support WebRTC
- Highly configurable for users
- Support most popular HTML5 players such as video.js、Clappr、Flowplayer
- Efficient scheduling policies to enhance the performance of P2P streaming
- Use IP database to group up peers by ISP and regions
- API frozen, new releases will not break your code

## Getting Started
Put the [quick-start.html](demo/quick-start.html) in your web page, run it. Wait for a few seconds，then open the same page from another browser. Now you have a direct P2P connection between two browsers without plugin!
The first web peer will serve as a seed, if no one else in the same channel.

## Browser Support
WebRTC has already been incorporated into the HTML5 standard and it is broadly deployed in modern browsers. The compatibility of CDNBye depends on the browser support of WebRTC and dash.js. Please note that iOS Safari "Mobile" does not support the MediaSource API.

 Compatibility|Chrome | Firefox | macOS Safari| Android Wechat/QQ | Opera | Edge | IE | iOS Safari | 
:-: | :-: | :-: | :-: | :-: | :-: | :-:| :-:| :-:
WebRTC Datachannel | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
dash.js | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ❌ |
CDNBye | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ❌ | ❌ |

## Include
Include the pre-built script of latest version: 
```html
<script src="https://cdn.jsdelivr.net/npm/cdnbye-dash@latest"></script>
```

## Usage
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


## API and Configuration
## Create instance

### `var engine = new P2PEngine(player, {p2pConfig: [opts]});` 
Create a new `P2PEngine` instance, `player` is an instance of `dashjs#MediaPlayer`.

If `opts` is specified, then the default options (shown below) will be overridden.

| Field | Type | Default | Description |
| :-: | :-: | :-: | :-: |
| `logLevel` | string or boolean | 'none' | Print log level(warn, error, none，false=none, true=warn).
| `live` | boolean | false | tell engine whether in live or VOD mode, set to false will pre-buffer for smooth playing.
| `wsSignalerAddr` | string | 'wss://signal.cdnbye.com' | The address of signal server.
| `announce` | string | 'https://tracker.cdnbye.com/v1' | The address of tracker server.
| `wsMaxRetries` | number | 15 | The maximum number of reconnection attempts that will be made by websocket before giving up.
| `wsReconnectInterval` | number | 30 | The number of seconds to delay before attempting to reconnect by websocket.
| `memoryCacheLimit` | Object | {"pc": 1024 * 1024 * 512, "mobile": 1024 * 1024 * 256} | The max size of binary data that can be stored in the cache.
| `p2pEnabled` | boolean | true | Enable or disable p2p engine.
| `dcDownloadTimeout` | number | 25 | Max download timeout for WebRTC datachannel.
| `webRTCConfig` | Object | {} | A [Configuration dictionary](https://github.com/feross/simple-peer) providing options to configure WebRTC connections.
| `useHttpRange` | boolean | false | Use HTTP ranges requests where it is possible. Allows to continue (and not start over) aborted P2P downloads over HTTP(True in live mode by default).
| `getStats` | function | - | Get the downloading statistics, including totalP2PDownloaded, totalP2PUploaded and totalHTTPDownloaded.
| `getPeerId` | function | - | Emitted when the peer Id of this client is obtained from server.
| `getPeersInfo` | function | - | Emitted when successfully connected with new peer.
| `channelId` | function | - | Pass a function to generate channel Id.(See advanced usage)
| `validateSegment` | function | - | Pass a function to check segment validity downloaded from peers.

### P2PEngine API

#### `P2PEngine.version` (static method)
Get the version of `P2PEngine`.

#### `P2PEngine.isSupported()` (static method)
Returns true if WebRTC data channel is supported by the browser.

#### `engine.enableP2P()`
Resume P2P if it has been stopped.

#### `engine.disableP2P()`
Disable engine to stop p2p and free used resources.

#### `engine.destroy()`
Stop p2p and free used resources.

### P2PEngine Events

#### `engine.on('peerId', function (peerId) {})`
Emitted when the peer Id of this client is obtained from server.

#### `engine.on('peers', function (peers) {})`
Emitted when successfully connected with new peer.

#### `engine.on('stats', function (stats) {})`
Emitted when data is downloaded/uploaded.</br>
stats.totalHTTPDownloaded: total data downloaded by HTTP(KB).</br>
stats.totalP2PDownloaded: total data downloaded by P2P(KB).</br>
stats.totalP2PUploaded: total data uploaded by P2P(KB).

### Advanced Usage
#### Another way to get the downloading statistics
```javascript
p2pConfig: {
    getStats: function (totalP2PDownloaded, totalP2PUploaded, totalHTTPDownloaded) {
        // do something
    }
}
```

#### Another way to get peer Id
```javascript
p2pConfig: {
    getPeerId: function (peerId) {
        // do something
    }
}
```

#### Another way to get peers information
```javascript
p2pConfig: {
    getPeersInfo: function (peers) {
        // do something
    }
}
```

#### Dynamic MPD path issue
Some MPD urls play the same live/vod but have different paths on them. For example, 
example.com/clientId1/file.mpd and example.com/clientId2/file.mpd. In this case, you can format a common channelId for them. `It is strongly recommended to add a unique identifier to the channelid to prevent conflicts with other channels.`
```javascript
p2pConfig: {
    channelId: function (mpdUrl) {
        const formatedUrl = 'YOUR_UNIQUE_ID' + format(mpdUrl);   // format a channelId by removing the different part
        return formatedUrl;
    }
}
```

#### Config STUN Servers
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

#### Allow Http Range Request
If http range request is activated, we are able to get chunks of data from peer and then complete the segments by getting other chunks from the CDN, thus, reducing your CDN bandwidth. Besides, the code below is needed：
```javascript
p2pConfig: {
    useHttpRange: true,
}
```

## Console
Bind your domain in `https://oms.cdnbye.com`, where you can view p2p-related information.

## They are using CDNBye
[<img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1531253035445&di=7af6cc9ad4abe3d06ba376af22d85131&imgtype=0&src=http%3A%2F%2Fimg.kuai8.com%2Fattaches%2Fintro%2F1213%2F201612131436417407.png" width="120">](https://egame.qq.com/?hls=1&p2p=1&_debug=1)

## Related Projects
- [hlsjs-p2p-engine](https://github.com/cdnbye/hlsjs-p2p-engine) - Web Video Delivery Technology with No Plugins for hls.js.

## FAQ
We have collected some [frequently asked questions](https://p2p.cdnbye.com/en/views/FAQ.html). Before reporting an issue, please search if the FAQ has the answer to your problem.

## Contact Us
Email: service@cdnbye.com







