
---
title: Release Notes for the Recording SDK
description: 
platform: Linux
updatedAt: Mon Oct 28 2019 03:49:35 GMT+0800 (CST)
---
# Release Notes for the Recording SDK
## Overview

The Agora On-premise Recording SDK records communication and live broadcast contents based on the Agora Native SDK.

### Compatibility

This component package is compatible with the following SDKs:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>SDK</th>
<th>Description</th>
</tr>
<tr><td>The Agora Native SDK</td>
<td>The Agora On-premise Recording SDK is compatible with the Agora Native SDK v1.7.0 or later. If, for example, any user in the channel uses the Agora Native SDK v1.6, the whole channel cannot record.</td>
</tr>
<tr><td>The Agora Web SDK</td>
<td>The On-premise Recording SDK is compatible with the Agora Web SDK v1.12.0 or later.</td>
</tr>
</tbody>
</table>



### Known Issues and Limitation

-   When you record using an Android client, the image is turned upside down when you switch from a front-facing camera to a rear one.
-   If the user calls the <code>leaveChannel</code> method in the channel to stop recording, the recording file ends with a period of silence determined by the <code>idleLimitSec</code> field set for the configuration when calling the <code>joinChannel</code> method.
-   The recorded voice or video files are not encrypted. To be compliant with HIPPA, encrypt the disk with a disk encryption tool, such as cryptsetup.


> The Agora On-premise Recording SDK supports both Java and C++ from v2.2.0.

## v2.3.4

v2.3.4 is released on August 5, 2019.

**Improvements**

- The following private Java API methods are now public:
  - The `setUserBackground` method which sets the background image of a specified user. When the user is online but does not send any video stream, the background image is displayed.
  - The `setLogLevel` method which sets the logging level. The SDK generates logs in the selected level and in the levels preceding the selected level.
- Improves the robustness of Java API methods.
- Enhances the SDK's ability to collect the information of abnormal crashes.

**Issues fixed**

- File splitting in composite recording mode caused by switching channel profiles.
- Failure to only record audio in composite recording mode in the Live Broadcast profile.
- Libyuv crashes.
- Crashes when calling the `leaveChannel` method to leave a channel.

## v2.3.3

v2.3.3 is released on April 1, 2019. 

**New features**

#### 1. Supports monitoring the network connection status

v2.3.3 adds the following callbacks to inform the app of the connection status between the recording SDK and Agora's edge server:

- `onConnectionLost`: Informs that the SDK loses connection to the server.
- `onConnectionInterrupted`: Informs that the connection between the SDK and the server is interrupted.

#### 2. Informs the app of receiving the first frame 

v2.3.3 adds the following callbacks to inform the app of receiving the first remote audio or video frame:

- `onFirstRemoteAudioFrame`: Informs that the first remote audio frame is received.
- `onFirstRemoteVideoDecoded`: Informs that the first remote video frame is decoded. 

#### 3. Supports monitoring status changes of the receiving stream 

v2.3.3 adds the `onReceivingStreamStatusChanged` callback to get status changes of the receiving audio or video stream.

#### 4. Supports the volume indication

v2.3.3 adds the `onAudioVolumeIndication` callback to get the list of users who are speaking and their volumes. 

**Issues fixed**

- Fails to run the transcoding script when the recording SDK exits the channel and then enters the channel again in manual mode.
- Occasional crashes when using invalid parameters in the `mixedVideoAudio` method.
- Freezes in HD video recordings.
- Setting the recording video frame rate does not take effect.
- Occasional crashes when compiling the recording Java sample code in Manual Mode.
- Fails to run the transcoding command `-f . -m 3`.


## v2.3.0

v2.3.0 is released on January 14, 2019. 

> The calculation of the usage of the On-premise Recording SDK v2.3.0 or later is separated from that of the Agora Voice Call/Voice Interactive Broadcast SDK and the Agora Video Call/Video Interactive Broadcast SDK. Contact [sales-us@agora.io](mailto:sales-us@agora.io) for details.

**New features**

#### 1. Supports Stereo Audio in Composite Recording Mode

v2.3.0 supports high audio quality (a sample rate of 48 kHz, stereo, and a bitrate of up to 192 Kbps) in Composite Recording Mode.

> Stereo is not applicable to raw data. 

`audio profile` can be set as `0` or `1`:

- Individual recording (file mode):
  - By default or set `audio profile=0`: A sample rate of 48 KHz, the same number of audio channels as the original audio, and an adaptive bitrate.
- Individual recording (raw data):
  - The sample rate is fixed at 48 KHz and the number of audio channels is the same as the original audio:
    - PCM: The bitrate may change.
    - AAC: The bitrate is the same as in the file mode.
- Composite recording (file mode): 
  - `audio profile=0`: A sample rate of 48 KHz, mono, and a bitrate of 48 Kbps
  - `audio profile=1`: A sample rate of 48 KHz, mono, and a bitrate of 128 Kbps
  - `audio profile=2`: A sample rate of 48 KHz, stereo, and a bitrate of 192 Kbps
- Composite recording (raw data): 
  - A fixed sample rate of 48 KHz and mono.

#### 2. Supports Web Video Players 

Video files transcoded in real time or transcoded by the transcoding script can be played by web browsers on Android, iOS/macOS, and Windows. The following is a list of compatible web browsers on each platform.

<table>
  <tr>
    <th>Platform</th>
    <th>Chrome </th>
    <th>Safari</th>
  </tr>
  <tr>
    <td>Android </td>
    <td>Chrome 49+</td>
    <td>N/A</td>
  </tr>
  <tr>
    <td>iOS</td>
    <td>✘</td>
    <td>Safari 9+</td>
  </tr>
  <tr>
    <td>macOS 10+</td>
    <td>Chrome 47+</td>
    <td>Safari 11+</td>
  </tr>
  <tr>
    <td>Windows</td>
    <td>Chrome 49+</td>
    <td>N/A</td>
  </tr>
</table>

#### 3. Supports Custom Background Images in Composite Recording Mode 

v2.3.0 adds the function for users to add a background image while recording audio in Composite Recording Mode. A custom image is used for any user who does not have video while recording video in Composite Recording Mode.

> This function is not applicable to raw data and screenshots.

#### 4. Adds Two Pre-defined Layout Templates to the Sample Code

v2.3.0 provides two pre-defined layout templates for composite recording to make the On-premise Recording SDK sample code easier to use.

##### Best Fit (bestFit)

v2.3.0 adds <code>bestFit</code> as the default layout supporting up to 17 video streams.

This layout automatically scales according to the number of video streams. The number of columns and rows varies depending on the number of video streams. For example:

![](https://web-cdn.agora.io/docs-files/1542680053900)

##### Vertical Presentation（verticalPresentation）

v2.3.0 adds the function to display a specified user's video in a large window on the left of the screen, while displaying the other users' videos in small windows arranged vertically on the right. At most, two columns with eight video windows in each column are supported. 

For example:

![](https://web-cdn.agora.io/docs-files/1542680070362)

#### 5. Screenshots in Composite Recording Mode

v2.3.0 adds the function to capture the screen of every user while recording video in Composite Recording Mode. Recording and screen capturing use the same video stream.

#### 6. Speaker Detection

v2.3.0 adds the `onActiveSpeaker` callback to return the UID of the active speaker in the channel.

This function is disabled by default and can be enabled by setting the `audioIndicationInterval` parameter in `RecordingConfig`. The On-premise Recording SDK returns the `onActiveSpeaker` callback at set intervals.

**Improvements**

- Improves audio and video synchronization in Individual Recording Mode.
- Supports automatic rotation of recorded video streams that are not transcoded in real time in Individual Recording Mode.
- Changed the idle behavior in Manual Mode:
     - For versions earlier than v2.3.0 in Manual Mode, the idle mechanism takes effect immediately after joining a channel, which might cause the recording SDK to exit the channel.
     - For v2.3.0 or later, the idle mechanism does not take effect until the `startService` method is called.
- Optimizes the log levels and split some logs in INFO into NOTICE, WARN, and ERROR.
- Optimizes the naming of the recording directory to ensure the uniqueness of each folder.

## v2.2.3 

v2.2.3 is released on October 18, 2018. 

**Issues fixed**

-   Coredump loss caused by .backtrace.
-   System crashes caused by Java jni. and optimize stability.
-   Transcoding script issues in Manual Mode. 
-   The recording video file splits into two files after a web client joins the channel.
-   Occasional system crashes caused by the subthread being active after the main thread is released.

## v2.2.2 

v2.2.2 is released on August 1, 2018. 

**Improvements**

-   The screenshots are named `uidYmdHMSms.jpg` instead of `uidYmdHMS.jpg`.
-   The transcoding script supports autorotation.
-   The structure of the Java folder is modified.


**Issues fixed**

-   Abnormal webm recording times.
-   Memory leaks.
-   Multi-party intercommunication issues.
-   H.264 parser issues.
-   Audio and video out of sync.


## v2.2.1

v2.2.1 is released on June 5, 2018. 

**Improvements**

-   Improves the performance of the Communication profile. The number of recording channels that a system supports with the same performance increases by 150%.
-   Improves the efficiency to find the port. The time to find the port is no longer part of the idle time.


**Issues fixed**

Port conflicts when the search for the port takes too long and exceeds the idle time. As a result, the port is not connected.


## v2.2.0

v2.2.0 is released on May 4, 2018.

**New features**

> Support for both Java and C++.

**Issues fixed**

-   Issues of oversized logs.
-   Abnormal issues when recording fast-forward videos.
-   Intermittent failures.
-   Performance issues.
-   Some crash issues.


## v2.1.0

v2.1.0 is released on March 7, 2018. 


**New features**

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Function</th>
<th>Description</th>
</tr>
<tr><td>Recording Mode</td>
<td>Supports selecting auto or manual mode when joining a channel to flexibly control the recording.</td>
</tr>
<tr><td>Control Recording</td>
<td>Adds interfaces to unbind the operations of joining a channel and recording. If the auto recording mode is used, the recording starts when a user joins the channel. If the manual recording mode is used, you can control when to start and stop the recording.</td>
</tr>
<tr><td>Mix Raw Audio</td>
<td>Supports the function of mixing raw audio data.</td>
</tr>
<tr><td>Java Recording API</td>
<td>Supports Java APIs for recording.</td>
</tr>
</tbody>
</table>



**Improvements**

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Improvement</th>
<th>Description</th>
</tr>
<tr><td>Transcoding Script</td>
<td>Supports setting the transcoding frame rate and resolution, either in picture-in-picture or single-stream mode.</td>
</tr>
</tbody>
</table>



**Issues fixed**

-   Occasional recording file transcoding failures.
-   The video screen occasionally turns upside down.
-   Occasional abnormal audio during recording.


## v2.0.0

v2.0 is released on November 21, 2017. 

-   Optimizes the raw data to support various formats:
    -   Modifies the <code>decodeAudio</code> and <code>decodeVideo</code> parameters, and adds the <code>VideoJpgFrame</code> struct.
    -   Modifies the <code>getAudioFrame</code> and <code>getVideoFrame</code> parameters. 
-   Adds the <code>captureInterval</code> parameter to set the time interval of capturing screenshots. 
-   Adds the <code>streamType</code> parameter to support different video stream types. 
-   Adds the <code>isVideoOnly</code> parameter. 
-   The transcoding scripts, once used, generate a convert.log file under the same path as the voice/video file. 
-   Adds the video rotating information in `UID_HHMMSSMS.txt`. 

## v1.3.0

v1.3 is released on October 20, 2017.

**New features**:

-   Adds mixing the audio and video recording functions by adding the <code>mixedVideoAudio</code> and <code>cfgFilePath</code> parameters in the <code>joinChannel</code> method.
-   Adds the function of merging the audio and video file of the same uid as one, see [Play the Recording Files](https://docs.agora.io/en/1.14/addons/Recording/tutorial/recording?platform=All%20Platforms).
-   Adds the <code>getProperties</code> method to get the recording path immediately after recording is started before joining any channel.
-   Modifies the <code>onError</code> and <code>onLeaveChannel</code> callbacks.


## v1.2.0

v1.2 is released on August 21, 2017. 

**New features**:

-   Supports getting the audio and video raw data.
-   Supports detecting sexually explicit content.
-   Provides a `recordingsys.log` log file to check for failures.
-   Supports the recording channel timestamp, that is, the first user who starts recording in the channel.


## v1.1.0

v1.1 is released on July 25, 2017. 

**New features**:

- Adds recording at the web client.
- Adds the real-time video mixing function.
- Adds a set of callback functions.
- Modifies the file format after transcoding.
- Enables configuring the UDP port.
- 
**Issues fixed**:

- Wrong timestamps.
- Transcoding failures.
- Unable to playback VLC files.


## v1.0.1

v1.0.1 is released on June 27, 2017, and fixes the crash when you set the channel profile of the recording as live broadcast.

## v1.0.0

v1.0.0 is released on June 15, 2017. 

This is the first release of the On-premise Recording SDK with the following functions:

- Communication and live broadcast scenarios.
- Recording the voice and video of all users in a channel.
- Recording the voice and video of all users in multiple channels simultaneously.
- Recording the voice of all users in a channel or in multiple channels simultaneously.
- Recording an encrypted channel if the application has integrated the Agora built-in encryption.
