
---
title: Record a Call
description: 
platform: Linux Java
updatedAt: Mon Sep 09 2019 08:10:06 GMT+0800 (CST)
---
# Record a Call
This page demonstrates how to record a call by using the command line. You can also record calls by calling the API methods. For the detailed API reference, see [Recording API](https://docs.agora.io/en/Recording/API%20Reference/recording_java/index.html). The command line and API methods implement the same functions. 

Ensure you integrate the recording SDK before proceeding, see [Integrate the SDK](../../en/Recording/recording_integrate_java.md).

> When the recording SDK joins the channel, it is equivalent to a dumb client. So the On-premise Recording SDK needs to join the same channel and use the same App ID and channel profile as the Agora Native/Web SDK.

## Start recording

Open a command-line tool and run the following command under the **/samples/java/bin** directory. 

```bash
java RecordingSample --appId <your App ID> --channel <channel name> --uid 0 --channelProfile <0 Communication, 1 Live broadcast> --appliteDir Agora_Recording_SDK_for_Linux_FULL/bin
```

- `appId`, `channel,` and `channelProfile` must be the same as the Agora Native/Web SDK.
- `appliteDir` must be the path to the `AgoraCoreService` file.

## Set recording options

Apart from the parameters in the above example, the demo provides other parameters and options. Run `java RecordingSample`  under the **/samples/java/bin** directory to view all the parameters and options.

> The `appId`, `uid`, `channel`, and `appliteDir` parameters are mandatory to start a recording. Other parameters can be set as needed.

### Mandatory parameters

- `appId`: The App ID must be the same as that of the Agora RTC SDK. For details, see [Getting an App ID](../../en/Recording/token.md).
- `uid`: User ID. A 32-bit unsigned integer ranging from 1 to (2<sup>32</sup>-1) that is unique in a channel. If the UID is set as 0, the SDK assigns a UID. 
- `channel`: Name of the channel to be recorded.
- `appliteDir`: The directory of `AgoraCoreServices`. The default path for `AgoraCoreServices` in the SDK package is: **Agora_Recording_SDK_for_Linux_FULL/bin/**.

The following parameters are not mandatory, but they depend on the settings of the recording channel.

- `channelKey`: The [dynamic key](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-namekeyadynamic-key) used in the channel to be recorded. Ensure that you set this parameter if the recording channel uses a token.
- `channelProfile`: Sets the channel profile. **The On-premise Recording SDK must use the same channel profile as the Agora RTC SDK. Otherwise, issues may occur.**
  - `0`: (Default) The communication profile.
  - `1`: The live broadcast profile. 
- `decryptionMode`: When the whole channel is encrypted, the On-premise Recording SDK uses `decryptionMode` to enable the built-in decryption function. The decryption mode must be the same as the encryption mode set by the Native/Web SDK. The following decryption methods are supported:
  - `aes-128-xts`: AES-128, XTS mode
  - `aes-128-ecb`: AES-128, ECB mode
  - `aes-256-xts`: AES-256, XTS mode
- `secret`: The decryption password when decryption mode is enabled.

### Range of ports

We recommend that you specify the range of ports for the recording processes. Configure a large range for all recording processes (Agora recommends 40000 to 41000 or larger). If so, the On-premise Recording SDK assigns ports to each recording process within the specified range and avoids port conflicts automatically. 

> If the `lowUdpPort` and `highUdpPort` parameters are not specified, the ports used by the recording processes are at random, which may cause port conflicts.

- `lowUdpPort`: Sets the lowest UDP port. Ensure that the value of highUdpPort - lowUdpPort is greater than 6. The default value is 0.
- `highUdpPort`: Sets the highest UDP port. Ensure that the value of highUdpPort - lowUdpPort is greater than 6. The default value is 0.

### Recording session control

- `triggerMode`: Sets whether to start recording automatically or manually.

  - `0`: Automatically. The recording SDK automatically starts recording when joining a channel and stops recording when leaving a channel.
  - `1`: Manually. Start and stop recording by commands. See [FAQ](https://docs.agora.io/en/faq/cmd_control_record) for details.

- `idle`: The Agora On-premise Recording SDK automatically stops recording when the recording channel is idle after a time period set by this parameter. The value must be greater than three seconds. The default value is 300 seconds. 

  The idle time is also charged as usage. A channel is idle in the following situations:

  - In a communication channel, no user of the Native SDK is in the channel, and no user of the Web SDK publishes a stream.
  - In a live-broadcast channel, no host is in the channel.

### Selective recording

- `isAudioOnly`: Sets whether or not to record audio only.
  - `0`: (Default) Enables both audio and video recording.
  - `1`: Enables audio recording and disables video recording.
- `isVideoOnly`: Sets whether or not to record video only.
  - `0`: (Default) Enables both audio and video recording.
  - 1: Enables video recording and disables audio recording.
- `streamType`: Sets the type of stream to be recorded. Takes effect only when the recording channel enables the dual-stream mode.
  - `0`: (Default) Record the high-video stream.
  - `1`: Record the low-video stream.

### Recording format

- `getAudioFrame`: Sets the recording audio format. Do not set `isMixingEnabled` as 1 when this parameter is set as 1, 2, or 3.
  - `0`: Default audio format.
  - `1`: Audio frame in AAC format.
  - `2`: Audio frame in PCM format.
  - `3`: Audio-mixing frame in PCM format.
- `getVideoFrame`: Sets the recording video format. Do not set `isMixingEnabled` as 1 when this parameter is set as 1, 2, 3, or 4.
  - `0`: Default video format.
  - `1`: Video frame in H.264 format.
  - `2`: Video frame in YUV format.
  - `3`: Video frame in JPEG format.
  - `4`: Screenshots in JPEG format.
  - `5`: Screenshots in JPEG format + MP4 video files.
    - Individual Mode (`isMixingEnabled` is 0): One MP4 video file and multiple JPEG screenshots for each user.
    - Composite Mode (`isMixingEnabled` is 1): One MP4 video file for all the users and multiple JPEG screenshots for each user.
- `captureInterval`: Sets the interval of the screen capture. The interval must be longer than one second and the default value is five seconds. Takes effect only when `getVideoFrame` is set as 3, 4, or 5.

### Composite recording mode

- `isMixingEnabled`: Sets whether or not to enable composite recording mode.

  - `0`: (Default) Enables individual recording mode, which means recording one audio or video file for each user. The sample rate of the recording file is 48 KHz, and the bitrate and audio channel number of the recording file are the same as those of the original audio stream. The video profile is the same as that of the original video.
  - `1`: Enables composite recording mode, which means the audio of all the users is mixed in an audio file and the video of all the users is mixed in a video file. You can set the audio profile of the recording file by the `audioProfile` parameter and set the video profile by the `mixResolution` parameter.

- `mixedVideoAudio`：When `isMixingEnabled` is 1, you can set this parameter to mix the audio and video in an MP4 file in real time. 

  - `0`: (Default) Mixes the audio and video respectively.
  - `1`: Mixes the audio and video in real time into an MP4 file. Supports limited players.
  - `2`: Mixes the audio and video in real time into an MP4 file. Supports more players.

  See [FAQ](https://docs.agora.io/cn/faq/recording_player) for the supported players.

- `layoutMode`：Sets the video mixing layout. See [Set Video Layout](../../en/Recording/recording_layout_guide.md) for details.

  - `0`: (Default) Floating layout. The first user in the channel occupies the full canvas. The other users occupy the small regions on top of the canvas, starting from the bottom left corner. The small regions are arranged in the order of the users joining the channel. This layout supports one full-size region and up to four rows of small regions on top with four regions per row, comprising 17 users.
  - `1`: Best Fit Layout. This is a grid layout. The number of columns and rows and the grid size vary depending on the number of users in the channel. This layout supports up to 17 users.
  - `2`: Vertical Layout. One large region is displayed on the left edge of the canvas, and several smaller regions are displayed along the right edge of the canvas. The space on the right supports up to 2 columns of small regions with 8 regions per column. This layout supports up to 17 users.

- `maxResolutionUid`: When `layoutMode` is set as `2` (vertical layout), you can specify the UID of the large video window by this parameter.

- `audioProfile`: Sets the audio profile (sample rate, bitrate, encode mode, and the number of channels) of the recording file. Takes effect only when `isMixingEnabled` is 1.

  - `0`: (Default) Sample rate of 48 KHz, communication encoding, mono, and a bitrate of up to 48 Kbps.
  - `1`: Sample rate of 48 KHz, music encoding, mono, and a bitrate of up to 128 Kbps.
  - `2`: Sample rate of 48 KHz, music encoding, stereo, and a bitrate of up to 192 Kbps.

- `mixResolution`: When `isMixingEnabled` is set as 1, you can use this parameter to set the video profile, including the width, height, frame rate, and bitrate. The default setting is 360 x 640, 15 fps, and 500 Kbps. We recommend you set the video profile according to the [Video Profile Table](https://docs.agora.io/en/faq/recording_video_profile).

- `defaultVideoBg`: Sets the directory of the default background image of the canvas in composite recording mode. Only supports local images in JPEG format. If `defaultVideoBg` is not set, the background image is black by default.

- `defaultUserBg`: Sets the directory of the default background image of the users in composite recording mode. The background image is displayed when a user is online and does not send any video stream. Only supports local images in JPEG format. If `defaultVideoBg` is not set, the background image is black by default.

  > This setting does not work for users of the Agora Web SDK.

### Path of the recorded files

Use either of the following parameters to set the path of the recorded files.

- `recordFileRootDir`：Sets the path of the recorded files. After setting `recordFileRootDir`, the subdirectory will be automatically generated according to the date of the recording.
- `cfgFilePath`: Sets the path of the configuration file. In the configuration file, you can set the absolute directory of the recorded files, but the subdirectory will not be automatically generated. The content in the configuration file must be in JSON format, such as `{"Recording_Dir" : "<recording path>"}`. Do not modify `"Recording_Dir"`.

### Other settings

- `proxyServer`: Sets the proxy server, which allows recording with the Intranet server. For example `127.0.0.1:1080`.
- `audioIndicationInterval`: Sets whether or not to detect the users who speak. Disabled by default.
  - `0`: (Default) Disables the function of detecting the users who speak.
  - \> 0: The interval (ms) of detecting the users who speak. Agora recommends setting the interval to be longer than 200 ms. When the SDK detects the users who speak, the SDK returns the UID of the user who speaks loudest and the UIDs of all the users who speak and their voice volumes.
- `logLevel`: Sets the log level. Only log levels preceding the selected level are generated. The default value of the log level is 5.
  - `1`: The log level is Fatal.
  - `2`: The log level is Error.
  - `3`: The log level is Warn.
  - `4`: The log level is Notice.
  - `5`: The log level is Info.
  - `6`: The log level is Debug.

## Next Steps

After the recording is complete, use the transcoding script to merge the recorded files. See [Using the Transcoding Script](../../en/Recording/recording_voice_video.md).

If any error or warning occurs during the recording, see [Error codes](https://docs.agora.io/en/Recording/API%20Reference/recording_java/enumio_1_1agora_1_1recording_1_1common_1_1_common_1_1_e_r_r_o_r___c_o_d_e___t_y_p_e.html) and [Warning codes](https://docs.agora.io/en/Recording/API%20Reference/recording_java/enumio_1_1agora_1_1recording_1_1common_1_1_common_1_1_w_a_r_n___c_o_d_e___t_y_p_e.html).

