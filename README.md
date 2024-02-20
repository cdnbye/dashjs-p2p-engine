**English | [简体中文](Readme_zh.md)**

<h1 align="center"><a href="" target="_blank" rel="noopener noreferrer"><img width="250" src="https://www.swarmcloud.net/img/logo.png" alt="cdnbye logo"></a></h1>
<h4 align="center">Let your viewers become your unlimitedly scalable CDN.</h4>
<p align="center">
  <a href="https://www.npmjs.com/package/cdnbye-dash"><img src="https://img.shields.io/npm/v/cdnbye-dash.svg?style=flat" alt="npm"></a>
  <a href="https://www.jsdelivr.com/package/npm/cdnbye-dash"><img src="https://data.jsdelivr.com/v1/package/npm/cdnbye-dash/badge" alt="jsdelivr"></a>
</p>


CDNBye dashjs-p2p-engine implements [WebRTC](https://en.wikipedia.org/wiki/WebRTC) datachannel to scale live/vod video streaming by peer-to-peer network using bittorrent-like protocol. The forming peer network can be layed over other CDNs or on top of the origin server. Powered by [dash.js](https://github.com/Dash-Industry-Forum/dash.js), it can play MPEG-dash on PC and mobile.

## Features
- WebRTC data channels for lightweight peer-to-peer communication with no plugins
- Support live and VOD streams over MPEG-dash protocol
- Very easy to integrate with an existing dash.js project
- Seamlessly fallback to normal server usage if a browser doesn't support WebRTC
- Compatible with all CDNs, agnostic to DRM and video codecs. No service side changes required.
- Efficient scheduling policies to enhance the performance of P2P streaming
- Use IP database to group up peers by ISP and regions

## Getting Started
Put the [quick-start.html](demo/quick-start.html) in your web page, run it. Wait for a few seconds，then open the same page from another browser. Now you have a direct P2P connection between two browsers without plugin!
The first web peer will serve as a seed, if no one else in the same channel.

## Browser Support
WebRTC has already been incorporated into the HTML5 standard and it is broadly deployed in modern browsers. The compatibility of CDNBye depends on the browser support of WebRTC and dash.js. Please note that iOS Safari "Mobile" does not support the MediaSource API.

Compatibility|Chrome | Firefox | macOS Safari| Android Wechat/QQ | Opera | Edge | IE | iOS Safari |
:-: | :-: | :-: | :-: | :-: | :-: | :-:| :-:| :-:
WebRTC Datachannel | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
dash.js | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
CDNBye | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |

## Include
Include the pre-built script of latest version:
```html
<script src="https://cdn.jsdelivr.net/npm/cdnbye-dash@latest"></script>
```

## API and Configuration
See [API.md](https://docs.swarmcloud.net/dashjs/API)

## Console
Register your domain in `https://oms.cdnbye.com`, where you can view p2p-related information.

## Related Projects
- [shaka-p2p-engine](https://github.com/cdnbye/shaka-p2p-engine) - P2P engine for Shaka Player.
- [hlsjs-p2p-engine](https://github.com/cdnbye/hlsjs-p2p-engine) - Web Video Delivery Technology with No Plugins for hls.js.
- [mp4-p2p-engine](https://github.com/cdnbye/mp4-p2p-engine) - Web Video Delivery Technology with No Plugins for MP4.

## FAQ
We have collected some [frequently asked questions](https://docs.swarmcloud.net/faq). Before reporting an issue, please search if the FAQ has the answer to your problem.

## Contact Us
Email: service@cdnbye.com
<br>
Telegram: @cdnbye
<br>
Skype: live:86755838








