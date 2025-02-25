
---
title: MediaPlayer Kit
description: 
platform: Windows
updatedAt: Thu Sep 05 2019 06:53:12 GMT+0800 (CST)
---
# MediaPlayer Kit
## Description

During a video call, you can publish a separate video stream to the remote user through Agora’s SD-RTN™ and adjust the playback of the separate video stream in real time by using MediaPlayer Kit.

<a name="format"></a>
### Supported formats

- Local video: AVI, MP4, MKV, and FLV file formats.
- Online video: RTMP and RTSP streams.

<div class="alert note"> Only supports the single/dual video with a sampling rate of 32 kHz, 44100 Hz or 48 kHz.</div>

### Usage

Using MediaPlayer Kit, you can start/pause the playback video, adjust the playback progress, adjust the playback volume, and publish the playback video to the remote user:

- If you want to publish the playback video to the remote user, you may need to use a dual process. In a process, you can capture and publish the call video; while in another process, you can publish the playback video stream. If you join the channel with only a process, the remote user can only see the video window corresponding to that process.

- If you do not need to publish the playback video to the remote user, you can use MediaPlayer Kit as your local media player. We will not discuss this scenario here.

## Quickly experience MediaPlayer Kit

### Prerequisites

- Operating System: Windows 7 or later.
- Development Tools: Microsoft Visual Studio 2015 or later.

### Use MediaPlayerKit

1. Download and unzip the [MediaPlayerKitQuickstart](https://github.com/AgoraIO/Advanced-Video/tree/master/MediaPlayer/Mediaplayer-Windows) folder.
2. Open the `MediaPlayerKitQuickstart.sln` file with Microsoft Visual Studio.
3. Click **Build** > **Build Solution**.
4. After a successful compilation, click **Debug** > **Start Debugging**.
5. After a successful debugging, you can see a window that plays the video.

To specify the video that you want to play with MediaPlayer Kit, modify the video path in the following code in the project `.cpp`file.
``` C++
//The video path
Std::string path = "F://1080.mp4";
   Char *videopath = (char *)path.data();
   mediaPlayerKit->load(videopath, true);
```


## Implement MediaPlayer Kit

### Prerequisites

- Operating System: Windows 7 or later.
- Development Tools: Microsoft Visual Studio 2015 or later.

### Integrate MediaPlayer Kit

**1. Preparation**

- Download and unzip Agora Native SDK. See the **Video SDK** in [SDK Downloads](https://docs.agora.io/en/Agora%20Platform/downloads).
- Make sure you have completed the integration of the Agora Native SDK, as described in [Integrated Client](../../en/Audio%20Broadcast/windows_video.md).
- Download and unzip the MediaPlayerKit folder.

**2. Create a project**

<div class="alert warning"> <li>In this step, you should continue to integrate MediaPlayerKit projects based on existing project that has integrated the Agora Native SDK, rather than creating a new project from scratch. <li>This step shows you how to create a new project, you can skip it if you don't need this reference.</div>

- Create a new **Windows Desktop Application** based on **Visual C++**, and click **OK**.
- Click **Build** > **Build Solution** for this empty project.
- After a successful compilation, click **Debug** > **Start Debugging**.
-  After a successful debugging, go to the next step.

**3. Import files**
- Copy the `MediaPlayerKit` and `sdk` (under the Agora Native SDK folder) folders to your project file.
- Click on **Source Files**, right click **Add** > **Existing Item**, and import the `MediaPlayerKit/videokit` file.

**4. Import paths**
Click on your project file and right click on **Properties**.

- Click **C/C++** > **General**, and fill in the following paths in the **Additional Include Directories**:
    - `./sdk/include;`
    - `./MediaPlayerKit/include;`
    - `./MediaPlayerKit/videokit;`
    
- Click **Linker** > **General**, and fill in the following paths in the **Additional Library Directories**:
    - `./MediaPlayerKit/lib;`
    - `./sdk/lib;`
    
- Click **Linker** > **Input**, and fill in the following paths in **Additional Dependencies**:
    - `./MediaPlayerKit/lib/re_sampler.lib;`
    - `./MediaPlayerKit/lib/VideoPlayerKit.lib;`
    - `./sdk/lib/agora_rtc_sdk.lib;`


**5. Compile the project**

- Click on your project file, right click on **Properties**, click **C/C++** > **Precompiled Headers**, and select **Not Using Precompiled Headers**.
- Click **Build** > **Build Solution**.

After a successful compilation, you can refer to [Call the interfaces](#1) or [API documentation](#2) to complete your project.

<a name="1"></a>
### Call the interfaces

**API sequence diagram**
![](https://web-cdn.agora.io/docs-files/1567498507910)

> - The diagram shows only the basic interfaces. If you have more complex requirements, you can also call more interfaces in the Agora Native SDK, such as [`enableDualStreamMode`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a72846f5bf13726e7a61497e2fef65972).
> - Because `setVideoView` in the MediaPlayer Kit interface can set the local video window, you don't need to call [`setupLocalVideo`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a744003a9c0230684e985e42d14361f28) in the Agora Native SDK interface beforehand.

1. Call the Agora Native SDK interface to complete initialization and the basic video setup.

	- Call the `create` method to create an AgoraRtcEngine instance and call the `initialize` method to complete the initialization. See [Implementation](https://docs.agora.io/en/Interactive%20Broadcast/initialize_windows_live?platform=Windows#implementation).

	- Call the `setChannelProfile` method to set the channel profile as Live Broadcast. See [Set the channel profile as Live Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/join_live_windows?platform=Windows#set-the-channel-profile-as-live-broadcast).
	- Call the `setClientRole` method to set the role as the host.
    ```C++
    Int nRet = m_lpAgoraEngine->setClientRole(role);
    ```

	- Call the `enableVideo` method to enable the video mode. See [Enable the video mode](https://docs.agora.io/en/Interactive%20Broadcast/publish_windows_live?platform=Windows#enable-the-video-mode).

2. Call the MediaPlayer Kit interface to complete initialization and other preparations.

	- Call the `createMediaPlayerKitWithRtcEngine` method to create the engine.
    ```C++
    mediaPlayerKit = createMediaPlayerKitWithRtcEngine(rtcEngine,sampleRate);
    ```
	- Call the `setEventHandler` and `setMediaInfoCallback` methods to set the event callbacks and the media information callbacks.
> You can refer to [API documentation](#2) for the four callbacks.
>
  ```C++
  mediaPlayerKit->setEventHandler(handler);
  mediaPlayerKit->setMediaInfoCallback(infoCallback);
  ```

	- Call the `setVideoView` method to set the local view.
  ```C++
  mediaPlayerKit->setVideoView(hwnd);
  ```

3. Call the Agora Native SDK interface to complete the channel management.

	- Call the `joinChannel` method to join the channel. See [Join a live broadcast channel](https://docs.agora.io/en/Interactive%20Broadcast/join_live_windows?platform=Windows#join-a-live-broadcast-channel).
> Call this method after the `setVideoView` method.

	- Call the [`setupRemoteVideo`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac166814787b0a1d8da5f5c632dd7cdcf) method to set the view of the remote user.

4. Call the MediaPlayer Kit interface to load the video.

	- Call the `load` method to load the video into the memory.
> You can load the video from the local or URL path to meet your requirements. See [Supported format](#format).
>
  ```C++
  mediaPlayerKit->load(url.toUtf8().data(), true);
  ```

<a name="3"></a>
5. Call the MediaPlayer Kit interface to complete the media player functions.

	- Call the `play`, `adjustPlaybackSignalVolume`, `seekTo`, `getCurrentPosition`, `pause`, and `resume` methods to adjust the media player in real time.
> You can learn about these methods by referring to [API documentation](#2).
>
  ```C++
  mediaPlayerKit->play();
  mediaPlayerKit->adjustPlaybackSignalVolume(volume);
  mediaPlayerKit->seekTo(msec);
  mediaPlayerKit->getCurrentPosition();
  mediaPlayerKit->pause();
  mediaPlayerKit->resume();
  ```

6. Call the MediaPlayer Kit interface to publish the audio/video to the remote user.

    - If you only want to publish the audio to the remote user:
        - Call the `publishAudio` method to publish the audio.
        ```C++
        mediaPlayerKit->publishAudio();
        ```
        - Adjust the media player in real time by calling the interfaces in [Step 5](#3).
        - Call the `adjustPublishSignalVolume` method to adjust the volume received by the remote user.
        ```C++
        mediaPlayerKit->adjustPublishSignalVolume(volume);
        ```

        - Call the `unpublishAudio` method to stop publishing the audio to the remote user.
        ```C++
        mediaPlayerKit->unpublishAudio();
        ```

    - If you want to publish the the video to the remote user:
        - Call the `publishVideo` method to publish the video.
        ```C++
        mediaPlayerKit->publishVideo();
        ```

        - Adjust the media player in real time by calling the interfaces in [Step 5](#3).

        - Call the `adjustPublishSignalVolume` method to adjust the volume received by the remote user.
        ```C++
        mediaPlayerKit->adjustPlaybackSignalVolume(volume);
        ```

        - Call the `unpublishVideo` method to stop publishing the video to the remote user.
        ```C++
            mediaPlayerKit->unpublishVideo();
        ```

7. Call the MediaPlayer Kit interface to stop playback and quit MediaPlayer Kit.
	- Call the `stop` method to stop the playback.
  ```C++
  mediaPlayerKit->stop();
  ```

	- Call the `unload` method to release the video loaded into the memory.
  ```C++
  mediaPlayerKit->unload();
  ```

	- Call the `destroy` method to destroy the MediaPlayerKit instance and quit MediaPlayer Kit.
> After calling the `destroy` method, the binding of the display view of the local video stream is not valid.
>
  ```C++
  mediaPlayerKit->destroy();
  ```

<a name="2"></a>
## API documentation
See [API documentation](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/mediaplayer_cpp/index.html).

## Considerations

If you get an error in MediaPlayer Kit when playing video from the URL path due to network problems, you need to call the `load` method again to reload the video after the network conditions improve.
