
---
title: Agora Cloud Recording Changelog
description: 
platform: Linux
updatedAt: Tue Dec 10 2019 08:05:23 GMT+0800 (CST)
---
# Agora Cloud Recording Changelog
## Overview

Agora Cloud Recording is an add-on service to record and save voice calls, video calls, and interactive broadcasts on your cloud storage. Compared with Agora On-premise Recording, Agora Cloud Recording is more efficient and convenient as it does not require deploying Linux servers.

### Compatibility

Agora Cloud Recording is compatible with the following SDKs:

- Agora Native SDK v1.7.0 or later.
- Agora Web SDK v1.12 or later.

## 2019.11.15

You can now use the `query` method to get the names of the recorded files immediately after the recording starts.

## 2019.10.24

This release supports Tencent Cloud as one of the third-party cloud storages.

## 2019.10.08

**New features**

#### 1. Individual recording

The RESTful API adds individual recording mode, which supports recording the audio and video of each UID separately. See [Individual Recording](../../en/cloud_recording/cloud_recording_individual_mode.md) for details. Meanwhile, Agora provides the Audio & Video File Merging script, which you can use to merge the audio and video files generated in individual mode. See [Merge Audio and Video Files](../../en/cloud_recording/cloud_recording_merge_files.md) for details.

#### 2. Record specified UIDs

The RESTful API adds the `subscribeAudioUids` and `subscribeVideoUids` parameters, which allow you to record only specified UIDs.

#### 3. Customize the directory of the recorded files

The RESTful API adds the `fileNamePrefix` parameter, which allows you to specify the directory where you want to store the recorded files in the third-party cloud storage.

#### 4. Timestamp when the stream state changes

The RESTful API callback service adds the `recorder_audio_stream_state_changed` and `recorder_video_stream_state_changed` events to report the time when the state of the audio or video stream changes, as well as the corresponding UID.

#### 5. Format Converter script

Agora provides the Format Converter script, which you can use to convert between multiple file formats, such as TS, MP3, and MP4. See [Convert File Format](../../en/cloud_recording/cloud_recording_convert_format.md) for details.

#### 6. Synchronized playback

You can achieve synchronized playback between the recorded files and other stream files, such as online whiteboards, courseware, and messages, by using the timestamp when the recording starts. You can get the timestamp by using the RESTful API callback service, or by parsing the M3U8 file. See [Synchronized Playback](../../en/cloud_recording/cloud_recording_playback.md) for details.

**Improvement**

When an error occurs, you receive the error message in the HTTP response body, instead of just the error code. See [Agora Cloud Recording RESTful API](../../en/cloud-recording/cloud_recording_api_rest.md) for detailed information about the error codes.

**Fixed issue**

When uploading fails after you use the wrong `bucket` and `key` values of the third-party cloud storage, Agora Cloud Recording returns an error instead of uploading the recorded files to Agora Cloud Backup.


## 2019.07.22

**New features**

#### Customized video layout

The RESTful API adds a customized layout for the recording video. See [Set Video Layout](../../en/cloud-recording/cloud_layout_guide.md) for details.

You can set the `mixedVideoLayout` parameter as `3` and set the regions for each user in the `layoutConfig` parameter when starting a recording.

You can update the layout anytime during the recording by the `updateLayout` method.

#### Customized background color

The RESTful API adds the `backgroundColor` parameter to support customized background colors for the video layout. 

#### Timestamps

To get the accurate starting time of a recording, the RESTful API provides the Unix timestamp of when the first slicing starts in the response of the `query` method.  The RESTful API callback service adds the `recorder_slice_start` event to report the time when the first slicing starts and the time when the last recording fails.

**Improvement**

Optimizes the verification of whether `resourceId` corresponds with `uid` and `cname` when calling the RESTful API.

**Fixed issue**

Fixed minor issues in the default video layout (floating layout).

## 2019.07.02

- Changes the default background color in the composite layout to black.
- Reduces video freeze under poor network conditions.

## 2019.06.13

This release supports RESTful APIs. With the RESTful APIs, you can use Agora Cloud Recording through HTTP requests without integrating the SDK.

See the following documents for details:

- [RESTful API Quickstart](../../en/cloud-recording/cloud_recording_rest.md): Start cloud recording with RESTful APIs.
- [RESTful API Reference](../../en/cloud-recording/cloud_recording_api_rest.md): Details of the RESTful API methods.
- [RESTful API Callback Service](../../en/cloud-recording/cloud_recording_callback_rest.md): Enable the callback service to receive notifications of Agora Cloud Recording. 

## 2019.04.30

This is the first release of Agora Cloud Recording with the following functions:

- High-quality voice and video recordings.
- Mixed-stream voice and video recordings of all users in a channel.
- Three composite video layouts: float (default), best fit, and vertical, see [Set Video Layout](../../en/cloud-recording/cloud_layout_guide.md) for details.
- Third-party cloud storage. Agora Cloud Recording supports Amazon S3, Alibaba Cloud, and Qiniu Cloud.
- Provides C++ and Java SDK packages.
