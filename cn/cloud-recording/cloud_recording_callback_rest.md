
---
title: 云端录制 RESTful API 回调服务
description: Cloud recording restful api callback
platform: All Platforms
updatedAt: Fri Dec 06 2019 06:23:41 GMT+0800 (CST)
---
# 云端录制 RESTful API 回调服务
Agora 提供消息通知服务，你可以配置一个接收回调的 HTTP/HTTPS 服务器地址来接收云端录制的事件通知。当事件发生时，Agora 云端录制服务会将事件消息发送给 Agora 消息通知服务器，然后 Agroa 消息通知服务器会通过 HTTP/HTTPS 请求将事件投递给你的服务器。

> 如果你在 2019 年 8 月 27 日前已经配置过消息通知服务，请参考旧版[云端录制 RESTful API 回调服务](../../cn/cloud-recording/cloud_recording_callback_rest_old.md)。新的回调服务对消息通知中的字段进行了优化，我们推荐你进行[升级](../../cn/cloud-recording/cloud_recording_callback_rest_old.md)。

## Agora 消息通知服务

使用 Agora 消息通知服务前需要申请开通并进行配置，关于如何开通服务以及消息通知回调的数据格式详见[消息通知服务](https://docs-preview.agoralab.co/cn/Agora%20Platform/ncs)。

## 回调内容

开通消息通知服务后，当你指定要接收通知的事件发生时，Agora 消息通知服务器会以 HTTP 请求形式向你发送回调。回调的具体内容在请求包体中以 JSON 对象形式返回，根据事件不同，请求包体中 JSON 对象所包含的字段也不一样。

下面以一个示例说明请求包体中的字段：
![](https://web-cdn.agora.io/docs-files/1567589432439)
- 红色框内字段为消息通知服务请求包体的公共字段，所有回调中都包含这些字段，公共字段的含义详见[消息通知回调格式](https://docs-preview.agoralab.co/cn/Agora%20Platform/ncs?platform=All%20Platforms#消息通知回调格式)。
- 蓝色框内字段为云端录制 `payload` 中的[公共字段](#payload)，所有的云端录制事件的回调中 `payload` 都包含这些字段。
- `eventType`，`serviceType` 和 `details` 的内容取决于发生的事件，详见[回调事件](#event)。

### <a name="payload"></a>payload 字段

 `payload` 字段是一个 JSON 对象，包含事件的具体内容。云端录制每种类型的事件通知中 `payload` 都会包含以下字段：
  - `cname`：String 类型，录制的频道名称。
  - `uid`：String 类型，录制使用的 UID。
  - `sid`：String 类型，录制 ID，一次云端录制的唯一标识。
  - `sequence`：Number 类型，消息序列号，从 0 开始计数。消息可能乱序到达或者丢失重发，可以通过该参数标识消息。
  - `sendts`：Number 类型， 事件发生的时间 （UTC 时间）。Unix 时间戳，精确到毫秒。
  - `serviceType`：Number 类型，回调事件服务的类型。
    - 0：云端录制服务。
    - 1：录制模块。
    - 2：上传模块。
  - `details`：JSON 类型，具体的消息内容，详见下面每个事件的描述。

## <a name="event"></a>回调事件

本节详细介绍云端录制每种回调事件对应的 `serviceType` 以及 `details` 包含的具体字段。

| eventType | serviceType       | 事件描述                                                     |
| --------- | ----------------- | ------------------------------------------------------------ |
| [1](#1)   | 0（云端录制服务） | 云端录制服务发生错误                                         |
| [2](#2)   | 0（云端录制服务） | 云端录制服务发生警告                                         |
| [3](#3)   | 0（云端录制服务） | 云端录制服务状态发生变化                                     |
| [4](#4)   | 0（云端录制服务） | 生成录制索引文件                                             |
| [30](#30) | 2（上传模块）     | 上传服务已启动                                               |
| [31](#31) | 2（上传模块）     | 所有录制文件已上传至指定的第三方云存储                       |
| [32](#32) | 2（上传模块）     | 所有录制文件已经全部上传完成，但至少有一片上传到 Agora 备份云 |
| [33](#33) | 2（上传模块）     | 录制文件上传到第三方云存储的进度                             |
| [40](#40) | 1（录制模块）     | 录制服务已启动                                               |
| [41](#41) | 1（录制模块）     | 录制服务已退出                                               |
| [42](#42) | 1（录制模块）     | 同步录制文件信息                                       |
| [43](#43) | 1（录制模块）     | 音频流状态变化                                       |
| [44](#44) | 1（录制模块）     | 视频流状态变化                                       |


### <a name="1"></a>1 cloud_recording_error

`eventType` 为 1 表示云端录制服务发生错误， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `"cloud_recording_error"`。
- `module`：Number 类型，发生错误的模块。
  - `0`：录制模块
  - `1`：上传模块
  - `2`：云端录制服务
- `errorLevel`：Number 类型，错误级别。
  - `1`：debug
  - `2`：minor
  - `3`：medium
  - `4`：major
  - `5`：fatal。fatal 级别的错误很可能导致录制退出，如果收到该级别的消息请及时调用 [query](../../cn/cloud-recording/cloud_recording_api_rest.md) API 查询当前状态，并结合错误消息的内容进行处理。
- `errorCode`：Number 类型，错误码。如果错误发生在录制模块，请参考[错误码和警告码](https://docs.agora.io/cn/Recording/the_error_native)；如果错误发生在上传模块，请参考[上传错误码](#uploaderr)；如果错误发生在云端录制平台模块，请参考[云端录制平台错误码](#clouderr)；如果错误码未列出，请联系 Agora 技术支持。
- `stat`：Number 类型，事件状态，0 表示正常，其他值表示异常。
- `errorMsg`：String 类型，具体的错误信息。

### <a name="2"></a>2 cloud_recording_warning

`eventType` 为 2 表示云端录制服务发生警告， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `cloud_recording_warning`。
- `module`：Number 类型，发生警告的模块名。
  - `0`：录制模块
  - `1`：上传模块
- `warnCode`：Number 类型，警告码。如果警告发生在录制模块，请参考[错误码和警告码](https://docs.agora.io/cn/Recording/the_error_native)；如果警告发生在上传模块，请参考[上传警告码](#uploadwarn)。

### <a name="3"></a>3 cloud_recording_status_update

`eventType` 为 3 表示云端录制服务状态发生改变， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `cloud_recording_status_update`。
- `status`：Number 类型，云端录制当前的状态，请参考[云端录制服务状态码](#state)。
- `recordingMode`：Number 类型，录制模式，目前仅支持合流模式，该值为 0。
- `fileList`：String 类型，录制生成的 M3U8 索引文件名。

### <a name="4"></a>4 cloud_recording_file_infos

`eventType` 为 4 表示已生成录制索引文件并上传。每次录制均会生成一个 M3U8 文件，用于索引该次录制所有的切片 TS/WebM 文件。你可以通过 M3U8 文件播放和管理录制文件。

 `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `cloud_recording_file_infos`。
- `fileList`：String 类型，生成的 M3U8 文件名。

### <a name="30"></a>30 uploader_started

`eventType` 为 30 表示上传服务已启动， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `uploader_started`。
- `status`：Number 类型，事件状态，0 表示正常，其他值表示异常。

### <a name="31"></a>31 uploaded

`eventType` 为 31 表示所有录制文件已上传至指定的第三方云存储， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `uploaded`。
- `status`：Number 类型，事件状态，0 表示正常，其他值表示异常。

### <a name="32"></a>32 backuped

`eventType` 为 32 表示所有录制文件已经全部上传完成，但至少有一片上传到 Agora 备份云， Agora 备份云会自动将这部分文件上传到指定的第三方云存储。 `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `backuped`。
- `status`：Number 类型，事件状态，0 表示正常，其他值表示异常。

### <a name="33"></a>33 uploading_progress

`eventType` 为 33 表示当前上传的进度，录制开始后每分钟会通知一次。 `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `uploading_progress`。
- `progress`：Number 类型，0 到 10000 之间的数字，当前已上传文件与已录制的文件的比例乘以 10000。这个数字是不断变动的，录制退出后，到达 10000 表示上传完成。

### <a name="40"></a>40 recorder_started

`eventType` 为 40 表示录制服务已启动， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `recorder_started`。
- `status`：Number 类型，事件状态，0 表示正常，其他值表示异常。

### <a name="41"></a>41 recorder_leave

`eventType` 为 41 表示录制服务已退出， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `recorder_leave`。
- `leaveCode`：Number 类型，退出码，为以下多个原因的组合。
  - 0：初始化失败
  - 1：由信号触发的退出
  - 2：频道内除录制 App 外，没有其他用户
  - 4：捕获到信号错误
  - 8：wrapper 层主动退出

### <a name="42"></a>42 recorder_slice_start

`eventType` 为 42 表示同步录制文件信息， `details` 中包含以下字段：

- `msgName`：String 类型，消息名称，即 `recorder_slice_start`。
- `startUtcMs`：Number 类型，录制开始时间（即第一个录制切片的开始时间），UTC 时间，精确到毫秒。
- `discontinueUtcMs`：Number 类型，UTC 时间，精确到毫秒，正常情况下该字段值与 `startUtcMs` 一致。当录制发生异常中断时， Agora 云端录制会自动恢复录制，此时也会收到该事件通知，且该字段表示上一个正常的录制切片结束的时间。
- `mixedAllUser`：String 类型，是否将每个用户分开录制。
  - `"true"`：所有用户合并在一个录制文件中。
  - `"false"`：每个用户分开录制。
- `streamUid`：String 类型，用户 UID，表示录制的是哪个用户的音频流或视频流。在合流模式下，`streamUid` 为 `"0"`。
- `trackType`：String 类型，录制文件的类型。
  - `"audio"`：纯音频文件。
  - `"video"`：纯视频文件。
  - `"audio_and_video"`：音视频文件。

举例来说，某次录制生成第一个切片文件时，会收到回调通知该事件，其中`startUtcMS` 为第一个切片文件开始的时间。假设第 2 个 到第 N 个切片文件都是正常的，不会收到该事件通知，到第 N + 1 个切片时发生故障，导致该切片文件丢失且录制中断，此时重新开始录制后生成第 N + 2 个切片，会再次收到回调通知该事件，其中 `startUtcMs` 为第 N + 2 个切片开始的时间， `discontinueUtcMs` 为第 N 个切片结束的时间。

### <a name="43"></a>43 recorder_audio_stream_state_changed

`eventType` 为 43 表示录制的音频流状态变化，`details` 中包含以下字段：

- `streamUid`: String 类型，用户 UID，表示录制的是哪个用户的音频流。
- `state`：Number 类型，云端录制服务是否正在接收音频流。
  - `0`：云端录制服务正在接收音频流
  - `1`：云端录制服务未在接收音频流
- `UtcMs`：Number 类型，音频流状态变化时的时间，UTC 时间，精确到毫秒。

### <a name="44"></a>44 recorder_video_stream_state_changed

`eventType` 为 44 表示录制的视频流状态变化，`details` 中包含以下字段：

- `streamUid`: String 类型，用户 UID，表示录制的是哪个用户的视频流。
- `state`：Number 类型，云端录制服务是否正在接收视频流。
  - `0`：云端录制服务正在接收视频流
  - `1`：云端录制服务未在接收视频流
- `UtcMs`：Number 类型，视频流状态变化时的时间，UTC 时间，精确到毫秒。


## 参考

### <a name="uploaderr"></a>上传错误码

| 错误码 | 描述                                             |
| :----- | :----------------------------------------------- |
| 32     | 第三方云存储信息错误                             |
| 47     | 文件上传失败                                     |
| 51     | 上传时文件操作发生错误                           |

### <a name="uploadwarn"></a>上传警告码

| 警告码 | 描述                                     |
| :----- | :--------------------------------------- |
| 31     | 重传到指定的云存储                       |
| 32     | 重传到 Agora 备份云                      |

### <a name="clouderr"></a>云端录制平台错误码

| 错误码 | 描述                 |
| :----- | :------------------- |
| 50     | 上传超时             |
| 52     | 云端录制服务启动超时 |

### <a name="state"></a>云端录制服务状态码

| 状态码 | 描述                 |
| :----- | :------------------- |
| 0      | 没有开始云端录制     |
| 1      | 云端录制初始化完成   |
| 2      | 录制组件开始启动     |
| 3      | 上传组件已启动       |
| 4      | 录制组件启动完成     |
| 5      | 已成功上传第一个文件 |
| 6      | 已经停止录制         |
| 7      | 云端录制服务全部停止 |
| 8      | 云端录制准备退出     |
| 20     | 云端录制异常退出     |
