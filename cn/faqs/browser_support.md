
---
title: Agora Web SDK 支持哪些浏览器
description: 
platform: Web
updatedAt: Wed Sep 11 2019 11:03:54 GMT+0800 (CST)
---
# Agora Web SDK 支持哪些浏览器
Agora Web SDK 支持所有主流浏览器，支持的浏览器及平台如下。

<table>
  <tr>
    <th>平台</th>
    <th>Chrome 58+</th>
    <th>Firefox 56+</th>
    <th>Safari 11+</th>
    <th>Opera 45+</th>
    <th>QQ 浏览器最新版</th>
    <th>360 安全浏览器</th>
    <th>微信浏览器</th>
  </tr>
  <tr>
    <td>Android 4.1+</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
		<td><b>N/A</b></td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>iOS 11+</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>macOS 10+</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>Windows 7+</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
		<td><b>N/A</b></td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
  </tr>
</table>

> - Agora Web SDK 2.5 及以上版本还支持 Windows XP 平台的 Chrome 49 版本。
> - Agora Web SDK 2.7 及以上版本还支持 Windows 10 平台的 Edge 浏览器，详见 [Edge 浏览器支持](https://docs.agora.io/cn/faq/browser_support#a-nameedgeaedge)。
> - 以下场景中请务必将 Web SDK 升级至 2.6 或更高版本:
>   - iOS 12.1.4 及以上版本使用 Safari 浏览器
>   - macOS 上使用 Safari 12.1 及以上版本

由于浏览器的差异，在不同浏览器及平台上支持的功能可能不同。下面列出浏览器和平台的已知限制。

## Chrome

Agora Web SDK 是基于 WebRTC 实现的采集和编解码，而 Chrome 又是第一批支持 WebRTC 的先行者，所以在 Chrome 上的限制最少，已知限制：

- Chrome 版本要求 58 及以上。
- 部分 API 需要 Chrome 更高版本支持，具体见各 API 内的描述。

## Safari

- Safari 12.1 及之前版本仅支持 H.264 编解码格式。
- Safari 只支持视频帧率设为 30 fps。
- 设备权限
  - Safari 无法获取输出设备信息，因此不支持 `getPlayoutDevices` 和 `setAudioOutput` 这两个方法。
  - 如果 Safari 浏览器没有打开自动播放，直接播放音视频流会听不到声音，必须在播放前调用 `navigator.mediaDevices.getUserMedia` 方法获取设备权限。
- Safari 不支持 `addTrack` 和 `removeTrack`。
- iOS 端 Safari 不支持 `setAudioLevel` 方法。

## Firefox 

- 如果 Web 端使用 Firefox 浏览器，Native 端使用 iOS 设备，Firefox 看到的视频方向会发生旋转。
- 在部分设备上 Firefox 设置视频编码配置不生效，目前已知有此问题的设备如下：
  - MacBook Pro (13-inch, 2016, Two Thunderbolt 3 ports)
  - Windows 10 (MI)

## <a name="edge"></a>Edge

Agora Web SDK 2.7 及以上版本支持 Edge 浏览器。受浏览器自身限制，仅支持以下功能：

- 与 Agora Native/Web SDK 音视频互通
- 调用 `getStats` 方法获取音视频流的连接数据（受浏览器更新的影响，可能存在部分字段缺失的情况）
- 调用 `getAudioLevel` 方法获取当前音量
- 调用 `muteAudio`/`unmuteAudio` 方法禁用/启用音频轨道
- 调用 `muteVideo`/`unmuteVideo` 方法禁用/启用视频轨道
- 调用 `setVideoProfile` 方法设置视频属性
