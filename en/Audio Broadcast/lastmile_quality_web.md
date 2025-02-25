
---
title: Last-mile Tests
description: 
platform: Web
updatedAt: Thu Sep 26 2019 05:59:32 GMT+0800 (CST)
---
# Last-mile Tests
## Introduction

To check if the network condition is good enough to support the audio bitrate or target bitrate of the chosen video profile, you can conduct a last-mile network quality test before actually joining a channel.

This function particularly applies to scenarios which have high requirements on the network quality.

## Implementation 

Before conducting the last-mile test, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Audio%20Broadcast/start_call_web.md) or [Start a Live Broadcast](../../en/Audio%20Broadcast/start_live_web.md).

Before joining the channel, create two Client objects and two streams to join a test channel, where one stream tests the uplink network conditions, and the other tests the downlink.

- After publishing the first stream with the `Stream.publish` method, call the `getStats` method to get `LocalStreamStats`, which reports the uplink network statistics.
- After subscribing to the second stream with the `Stream.subscribe` method, call the `getStats` method to get `RemoteStreamStats`, which reports the downlink network statistics.


### API call sequence

Refer to the following diagram to implement the last-mile test in your project.

![](https://web-cdn.agora.io/docs-files/1569477125325)

### Sample code

Refer to the following code to implement the last-mile test in your project.

```javascript
localStream.getStats((stats) => {
    console.log(`Local Stream accessDelay: ${stats.accessDelay}`);
    console.log(`Local Stream audioSendBytes: ${stats.audioSendBytes}`);
    console.log(`Local Stream audioSendPackets: ${stats.audioSendPackets}`);
    console.log(`Local Stream audioSendPacketsLost: ${stats.audioSendPacketsLost}`);
    console.log(`Local Stream videoSendBytes: ${stats.videoSendBytes}`);
    console.log(`Local Stream videoSendFrameRate: ${stats.videoSendFrameRate}`);
    console.log(`Local Stream videoSendPackets: ${stats.videoSendPackets}`);
    console.log(`Local Stream videoSendPacketsLost: ${stats.videoSendPacketsLost}`);
    console.log(`Local Stream videoSendResolutionHeight: ${stats.videoSendResolutionHeight}`);
    console.log(`Local Stream videoSendResolutionWidth: ${stats.videoSendResolutionWidth}`);
});

remoteStream.getStats((stats) => {
    console.log(`Remote Stream accessDelay: ${stats.accessDelay}`);
    console.log(`Remote Stream audioReceiveBytes: ${stats.audioReceiveBytes}`);
    console.log(`Remote Stream audioReceiveDelay: ${stats.audioReceiveDelay}`);
    console.log(`Remote Stream audioReceivePackets: ${stats.audioReceivePackets}`);
    console.log(`Remote Stream audioReceivePacketsLost: ${stats.audioReceivePacketsLost}`);
    console.log(`Remote Stream endToEndDelay: ${stats.endToEndDelay}`);
    console.log(`Remote Stream videoReceiveBytes: ${stats.videoReceiveBytes}`);
    console.log(`Remote Stream videoReceiveDecodeFrameRate: ${stats.videoReceiveDecodeFrameRate}`);
    console.log(`Remote Stream videoReceiveDelay: ${stats.videoReceiveDelay}`);
    console.log(`Remote Stream videoReceiveFrameRate: ${stats.videoReceiveFrameRate}`);
    console.log(`Remote Stream videoReceivePackets: ${stats.videoReceivePackets}`);
    console.log(`Remote Stream videoReceivePacketsLost: ${stats.videoReceivePacketsLost}`);
    console.log(`Remote Stream videoReceiveResolutionHeight: ${stats.videoReceiveResolutionHeight}`);
    console.log(`Remote Stream videoReceiveResolutionWidth: ${stats.videoReceiveResolutionWidth}`);
});
```

We also provide an open-source [Agora-WebRTC-Troubleshooting](https://github.com/AgoraIO/Tools/tree/master/TroubleShooting/Agora-WebRTC-Troubleshooting) demo project on Github. You can [try the demo online](https://webdemo.agora.io/agora_webrtc_troubleshooting/), or view the source code in the [App.Vue](https://github.com/AgoraIO/Tools/blob/master/TroubleShooting/Agora-WebRTC-Troubleshooting/src/App.vue) file.

### API Reference

- [`getStats`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#getstats)
- [`LocalStreamStats`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.localstreamstats.html)
- [`RemoteStreamStats`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.remotestreamstats.html)

## Considerations

- One Client object can publish one stream only. You need to create two Client objects to publish two streams.
- It may take some time to get some of the statistics.
- Due to browser limitations, some arguments in `LocalStreamStats` and `RemoteStreamStats` may be missing.
