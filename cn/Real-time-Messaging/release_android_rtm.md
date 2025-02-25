
---
title: 发版说明
description: 
platform: Android
updatedAt: Fri Nov 29 2019 11:08:05 GMT+0800 (CST)
---
# 发版说明
## 简介

Agora RTM SDK 提供了稳定可靠、低延时、高并发的全球消息云服务，帮助你快速构建实时通信场景,  可实现消息通道、呼叫、聊天、状态同步等功能。点击 [实时消息产品概述](../../cn/Real-time-Messaging/product_rtm.md) 了解更多详情。


## 1.2.1 版

该版本于 2019 年 11 月29 日发布。

### 新增功能

**支持与老信令 SDK 的 endCall 方法兼容** 

你可以调用 `sendMessageToPeer` 方法在发送<i>文本</i>消息时将消息头设为 AGORA_RTM\_ENDCALL\_PREFIX\_\<channelId\>\_\<your additional information\> 格式即可。请以 endCall 对应频道的 ID 替换 \<channelId\>， \<your additional information\> 为附加文本信息。请注意：附加文本信息中不可使用下划线 "_" ，附加文本信息可以设为空字符串 "" 。

### 问题修复

- 修复了一个用户使用 VPN 登陆 RTM，关闭 VPN 后 RTM 重连失败的问题。
- 修复了一个频道中用户断线重连，频道中其它用户有概率收到两次 `onMemberJoin` 回调的问题。




## 1.2.0 版

该版本于 2019 年 11 月6 日发布。新增如下功能：

- [订阅或退订指定单个或多个用户的在线状态](#subscribe)
- [获取某特定内容被订阅的用户列表](#list)
- [创建自定义二进制消息](#raw)
- [创建文本消息](#text)



### 新增功能

#### 1. <a name="subscribe"></a>订阅或退订指定单个或多个用户的在线状态。

<div class="alert note"> 该功能必须在登录 Agora RTM 系统成功（收到 onSuccess 回调）后才能调用。</div>

本版本支持订阅或退订最多 512 个用户的在线状态，SDK 会通过 `onSuccess` 返回订阅或退订结果。首次订阅成功时，SDK 会通过 `onPeersOnlineStatusChanged` 回调返回所有被订阅用户的在线状态；之后每当有被订阅用户的在线状态出现变化，SDK 都会通过 `onPeersOnlineStatusChanged` 回调通知订阅方。

<div class="alert note"> <sup>1</sup>用户登出 Agora RTM 系统后，所有之前的订阅内容都会被清空；重新登录后，如需保留之前订阅内容则需重新订阅。</div>

<div class="alert note"> <sup>2</sup>重复订阅同一用户不会报错。</div>


#### 2. <a name="list"></a>获取某特定内容被订阅的用户列表

<div class="alert note"> 该功能必须在登录 Agora RTM 系统成功（收到 onSuccess 回调）后才能调用。</div>

本版本支持根据被订阅类型获取被订阅用户列表。现实情况中，你可能多次订阅或退订，可能重复订阅了相同用户，可能出现订阅或退订不成功的情况，也可能根据不同的订阅类型订阅了不同的用户。这时，你可以通过本功能根据订阅类型获取当前被订阅用户列表。

被订阅类型由枚举类型 `PeerSubscriptionOption` 定义。本版本仅支持用户在线状态订阅一种类型，后继会不断扩展。

#### 3. <a name="raw"></a>创建自定义二进制消息

本版本支持创建自定义二进制消息，支持以点对点消息或频道消息形式发送多种文件格式。

本版本支持：

- 创建不带文本描述信息的自定义二进制消息
- 创建包含文本描述信息的自定义二进制消息

如果在创建自定义二进制消息时未设置文本描述，你可以在消息实例 `RtmMessage` 创建成功后通过调用 `setText` 方法设置自定义二进制消息的文本描述。

<div class="alert note"> 我们不对二进制消息的文本描述的大小单独进行限制，但是我们要求自定义二进制消息的总大小不超过 32 KB。</div>

#### 4. <a name="text"></a>创建文本消息

之前版本中，我们先通过 `createMessage` 方法创建一个空文本消息实例再通过调用 `RtmMessage` 类的对象方法 `setText` 设置文本内容，本版本提供了更加便利的 `createMessage` 方法可以在创建文本消息的同时直接设定文本消息内容。

<div class="alert note"> 文本消息大小不得超过 32 KB。</div>

### 问题修复

- 以相同 `channelId` 创建频道后调用 `join` 或 `leave` 时系统返回 `rtm native not ready` 。

## 1.1.0 版

该版本于 2019 年 9 月 18 日发布。新增如下功能：

- [查询频道成员人数](#getcount)
- [频道成员人数自动更新](#oncount)
- [频道属性增删改查](#channelattributes)



### 兼容性改动

1. 废弃点对点消息发送方法 [sendMessageToPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a25ab5c0126e1dc51c78b2b705de68b7a)，改由重载方法 [sendMessageToPeer(const char \*, const IMessage \*, const SendMessageOptions \&)](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a729079805644b3307297fb2e902ab4c9) 替代。
2. [RtmMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html) 对象的 [getServerReceivedTs](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html#a7994de6da26269c3137e93ddf7a2c2be) 方法由仅支持点对点消息改为同时支持点对点消息和频道消息。
3. 点对点消息的超时时间由 5 秒延长为 10 秒。详见： [PEER_MESSAGE_ERR_TIMEOUT ](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_peer_message_error.html#a9aaaa5b9fa46cc15327abd6c2825bc4d)
4. 针对 [join](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#ad7b321869aac2822b3f88f8c01ce0d40) 方法调用增加了[加入相同频道的频率限制：每 5 秒 2 次](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_join_channel_error.html#a2040b572e1ef4f593f234a20c84a22c7)。

### 新增功能

<a name="getcount"></a>
#### 1. 查询频道成员人数

支持在不加入频道的情况下通过主动调用 [getChannelMemberCount](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aff0384f2a004ed75498e20e1917352e4) 接口查询单个或多个频道的频道人数。一次最多可查询 32 个频道的成员人数。

<a name="oncount"></a>
#### 2. 频道成员人数自动更新

如果你已经加入某频道，你无需调用 `getChannelMemberCount` 接口查询当前频道人数。我们也不建议你通过监听 `onMemberJoined` 和 `onMemberLeft` 统计频道成员人数。从本版本开始，SDK 会在频道成员人数发生变化时通过 [onMemberCountUpdated](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#ad778e702e026a79460f45a992bb8576d) 回调接口通知频道成员并返回当前频道成员人数：

- 频道成员人数小于等于 512 时，最高触发频率为每秒 1 次。
- 频道成员人数超过 512 时，最高触发频率为每 3 秒 1 次。

> 请将该回调与 [getMembers](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a567aca5f866cf71c3b679ae09b4bf626) 方法进行区分：
> - 前者为主动回调。SDK 向频道成员返回最新频道成员人数。
> - 后者由 [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5) 返回频道成员列表，且当频道成员人数超过 512 时仅返回随机的 512 个成员列表。

<a name="channelattributes"></a>
#### 3. 频道属性增删改查

支持设置或查询某个指定频道的属性。你可以用频道属性实现群公告、上下麦同步等功能。

每个频道属性为 key 和 value 的键值对。详见：[RtmChannelAttribute](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel_attribute.html)。其中：
- 每个属性的 key 为 32 字节可见字符，每个属性的 value 的字符串长度不得超过 8 KB。
- 某个频道的全部属性长度不得超过 32 KB。
- 某个频道属性的全部属性个数不得超过 32 个。

支持功能包括：

- 全量设置某指定频道的属性。
- 添加或更新某指定频道的属性。
- 删除某指定频道的指定属性。
- 清空某指定频道的属性。
- 查询某指定频道的全部属性。
- 查询某指定频道指定属性名的属性。



### 性能改进

#### 点对点消息重发

本版本优化了点对点消息在弱网情况下的重发机制，并延长点对点消息超时时间为 10 秒，提高了在弱网情况下点对点消息的发送成功率。

#### 频道消息缓存

Agora RTM 系统会对短期掉线后重连成功的频道成员补发最长 30 秒最多 32 条的频道消息，提高了弱网情况下频道消息的到达率。


### API 变更

#### 新增方法

- [setChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ad25f51a3671db50e348ec6c170044ec6)：全量设置某指定频道的属性。
- [addOrUpdaeChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a765b186d62ed3ef6d67a5e875b040875)：添加或更新某指定频道的属性。
- [deleteChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a2477533989c1bb9ced831af210f1dba4)：删除某指定频道的指定属性。
- [clearChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ae0c6c5c5bae6020e69009441d8a41785)：清空某指定频道的属性。
- [getChannelAttributes](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a81f14a747a4012815ab4ba8d9e480fb6)：查询某指定频道的全部属性。
- [getChannelAttributesByKeys](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a3b927c35cca5ebd31afb976d60e99193)：查询某指定频道指定属性名的属性。
- [getChannelMemberCount](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aff0384f2a004ed75498e20e1917352e4)：查询单个或多个频道的成员人数。

#### 新增回调

- [onAttributesUpdated](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#a2904a1f1f78c497b9176fffb853be96f)：频道属性更新回调。返回所在频道的所有属性。
- [onMemberCountUpdated](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#ad778e702e026a79460f45a992bb8576d)：频道成员人数更新回调。返回最新频道成员人数。

#### 新增错误码 

- [GetChannelMemberCountErrCode](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_get_channel_member_count_err_code.html)：获取指定频道成员人数的相关错误码。
- [JOIN_CHANNEL_ERR_JOIN_SAME_CHANNEL_TOO_OFTEN](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_join_channel_error.html#a2040b572e1ef4f593f234a20c84a22c7)：加入相同频道的频率超过每 5 秒 2 次的上限。

#### 废弃方法

- [sendMessageToPeer](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a25ab5c0126e1dc51c78b2b705de68b7a)：被重载方法 [sendMessageToPeer(const char \*, const IMessage \*, const SendMessageOptions \&)](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a729079805644b3307297fb2e902ab4c9) 替代。

#### 废弃错误码

- [ATTRIBUTE_OPERATION_ERR_NOT_READY](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_attribute_operation_error.html#ac6a33aef7c62a132ba79630219d548a7)：被错误码 [ATTRIBUTE_OPERATION_ERR_USER_NOT_LOGGED_IN](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_attribute_operation_error.html#a9f329760056976289e49ad1dc69c598f) 替代。




## 1.0.1 版

该版本于 2019 年 8 月 1 日发布。

### 问题修复

- 断网后 SDK 未返回 `onConnectionStateChanged` 回调。

## 1.0.0 版

该版本于 2019 年 7 月 24 日发布。

### 新增功能

#### 新老互通

支持与 Agora Signaling SDK 互通。

本版本在 `LocalInvitation` 类中实现了 `setChannelId` 和 `getChannelId` 方法。

> - 如需与 Agora Signaling SDK 互通，则必须调用 `setChannelId` 方法设置频道 ID。不过即使被叫成功接受呼叫邀请，Agora RTM SDK 也不会将主叫或被叫加入指定频道。
> - 如果你的应用不涉及 Agora Signaling SDK，我们推荐使用 `LocalInvitation.setContent` 或者 `RemoteInvitation.setResponse` 方法设置自定义内容。

#### 设置日志文件地址

支持通过调用 `setLogFile` 方法变更本地日志的默认地址。该方法无需在 `login` 成功之后调用，我们建议在创建 `RtmClient` 后即调用该方法，否则会造成日志文件显示不完整。

#### 设置日志输出等级

支持通过调用 `setLogFilter` 方法将日志内容按照 OFF、CRITICAL、ERROR、WARNING 和 INFO 不同等级输出 。

> 该方法无需在 `login` 成功之后调用。

#### 设置日志文件大小

支持通过 `setLogFileSize` 方法设置日志文件大小。日志的默认大小为 512 KB。低于该默认大小的设置无效。

> 该方法无需在 `login` 成功之后调用。

###  功能改进

针对以下不同错误情况细化了错误代码

- Agora RTM 服务未初始化
- 调用频率超过上限
- 未调用 `login` 方法或 `login` 方法未调用成功

### 问题修复

- 修复了一个可以用静态 App ID 和一个通过动态 App ID 生成的 Token 登录Agora RTM 系统的问题。


### API 变更

- [setLogFile](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ad44bd79d005d25c68712cc35d16d934b)：设定日志文件的默认地址。
- [setLogFilter](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a6726b3a3eafee4528280d3b0d1c6316f)：设置日志输出等级。
- [setLogFileSize](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a85a6365227adc43f8c3e07042dec6723)：设置日志文件大小。
- [getSdkVersion](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#af3cc54b4d456a67d912786f61619c065)：获取 Agora RTM SDK 的版本信息。




## 0.9.3 版

该版本于 2019 年 6 月 7 日发布。

### 新增功能

#### 发送（离线）点对点消息

本版本支持发送离线消息。在开通离线消息后，用户不必等到接收端上线才能发送点对点消息。如果对端离线，消息服务器会为每个接收端存储 200 条离线消息长达七天。消息以队列形式存储。当离线消息超限时，最新存储的消息会导致最老的消息丢失。当发送端设置了离线消息，而此时消息接收端不在线，发送端会收到错误码：[PEER_MESSAGE_ERR_CACHED_BY_SERVER](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_peer_message_error.html#a723c2bc0bb1d4b045edc4044f026e30c)

> 该方法的调用频率限制为每秒 60 条（点对点消息和频道消息一并计算在内）。

#### 设置本地用户属性、查询指定用户属性

本版本支持设置和查询用户属性。每个用户属性为 key 和 value 的键值对。每个属性的 key 为 32 字节可见字符，每个属性的 value 的字符串长度不得超过 8 KB。单个用户的全部属性长度不得超过 16 KB。以下为本版本支持内容：

   - 全量设置本地用户属性
   - 增加或更新本地用户属性
   - 删除本地用户指定属性
   - 清空本地用户属性
   - 全量获取指定用户属性
   - 获取指定用户指定属性。


> - 用户属性的相关操作必须在登录 Agora RTM 系统成功后才能进行，否则 SDK 会返回错误码：`ATTRIBUTE_OPERATION_ERR_NOT_READY`
> - 设置的用户属性会在用户登出 Agora RTM 系统后自动失效。
> - 单次用户属性设置操作不得超过 16 KB，否则 SDK 会返回错误码：`ATTRIBUTE_OPERATION_ERR_SIZE_OVERFLOW` 。
> - 用户属性设置相关操作的调用频率限制为每 5 秒 10 条，超限则 SDK 会返回错误码：`ATTRIBUTE_OPERATION_ERR_TOO_OFTEN`。
> - 获取用户属性相关操作的调用频率为每 5 秒 40 条，超限则 SDK 会返回错误码：`ATTRIBUTE_OPERATION_ERR_TOO_OFTEN` 。

### API 变更

#### 新增：

- [sendMessageToPeer()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a729079805644b3307297fb2e902ab4c9):  发送（离线）点对点消息给指定用户。
- [getServerReceivedTs()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html#a7994de6da26269c3137e93ddf7a2c2be)： 消息接收者查询服务器的接收消息时间。
- [isOfflineMessage()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html#a463b5df7c54bf45dc39aa42457e4b5bd)：消息接收者查询消息是否为离线消息。
- [PEER_MESSAGE_ERR_CACHED_BY_SERVER](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_peer_message_error.html#a723c2bc0bb1d4b045edc4044f026e30c)：对端用户不在线，离线消息会被消息服务器存储。
- [setLocalUserAttributes()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a339b7b2371ff2b86137b6db6c1c66294)：全量设置本地用户属性
- [addOrUpdateLocalUserAttributes()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a765b186d62ed3ef6d67a5e875b040875)：增加本地用户属性或更新本地用户属性
- [deleteLocalUserAttributesByKeys()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a2477533989c1bb9ced831af210f1dba4)：删除本地用户指定属性
- [clearLocalUserAttributes()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ae0c6c5c5bae6020e69009441d8a41785)：清空本地用户全部属性
- [getUserAttributes()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aee9a6c027f35b652781f654a89433755)：获取指定用户的全部属性
- [getUserAttributesByKeys()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a3b927c35cca5ebd31afb976d60e99193): 获取指定用户的指定属性
- [	AttributeOperationError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_attribute_operation_error.html)：属性操作相关错误码

### 性能改进

- 支持在登录 Agora RTM 系统之前创建频道实例。
- 取消创建 RTM 频道最多 20 个的限制，但是同一用户只能同时加入 20 个频道，超限后会收到错误码 `JOIN_CHANNEL_ERR_FAILURE ` 。


### 问题修复

- 偶现的系统崩溃。
- 用户登出后，其它用户查询该用户仍然显示在线，30 秒后查询不在线。

## 0.9.2 版

该版本于 2019 年 5 月 8 日发布。

> 当前版本不支持在 [登录](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a) RTM 系统前 [创建频道实例](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a95ebbd1a1d902572b444fef7853f335a) 。

### 新增功能

#### 查询用户在线状态

本版本引入了新的概念：在线和离线。一般情况下：

- 在线：用户已登录到 Agora RTM 系统。
- 离线：用户已登出 Agora RTM 系统或因其他原因，比如权限或网络原因，与 Agora RTM 系统断开连接。

本版本增加了查询用户在线状态功能。你可以在登录 Agora RTM 系统后查询最多 256 个指定用户的在线状态。详见： [queryPeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ac711f981405648ed5ef1cb07436125f3) 接口。

> - 返回的 Map 的 Use ID 的顺序以输入顺序为准。
> - 返回的 Map 中与每个 User ID 对应的 BOOLEAN 值表示用户在线与否。
> - 该方法的调用频率限制为每 5 秒 10 次。详见 [限制条件](../../cn/Real-time-Messaging/RTM_limitations_ios.md) 。


#### 更新 Token

在安全性要求较高的情况下，你需要输入 Token [登录](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a) Agora RTM 系统。 每个 Token 都会在生成 24 小时后过期，本版本提供了 [更新 Token](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a9a6d33282509384165709107d7a89353) 的功能。

- 在登录 Agora RTM 系统时，如果你的 Token 已过期，SDK 会返回 [LOGIN_ERR_TOKEN_EXPIRED](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_login_error.html#a4a15940de40fe029ba9821e406f3d875) 错误码
- 在已登录 Agora RTM 系统的情况下，Token 过期不会导致用户立即被踢出，但当用户重新登录 Agora RTM 系统时需要调用。所以，我们建议你在收到 [onTokenExpired](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_client_listener.html#aef74f37ed8797d274115d7f13785134e) 回调后尽快更新 Token。


> - [renewToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a9a6d33282509384165709107d7a89353) 方法必须在 [创建 RtmClient 实例](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a6411640143c4d0d0cd9481937b754dbf) 后才能调用。
> - Agora RTM Token 的生成方式、输入参数与 Agora 媒体 SDK 不同，详情请见： [校验用户权限](../../cn/Real-time-Messaging/RTM_key.md) 。
> - `RenewToken` 方法的调用频率限制为 2 次 / 秒。详见 [限制条件](../../cn/Real-time-Messaging/RTM_limitations_ios.md) 。

### 性能改进

- 支持以空格开头的 `userId` 参数。


### API 变更

#### 查询用户在线状态相关

##### 新增

- 方法： [queryPeersOnlineStatus](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ac711f981405648ed5ef1cb07436125f3)
- 错误码：[QueryPeersOnlineStatusError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_query_peers_online_status_error.html)

#### 更新 Token 相关

##### 新增

- 方法： [renewToken](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a9a6d33282509384165709107d7a89353)
- 回调： [onTokenExpired](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_client_listener.html#aef74f37ed8797d274115d7f13785134e)
- 错误码： [RenewTokenError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_renew_token_error.html)

#### 呼叫邀请相关

新增以下错误码覆盖用户在没有登录 Agora RTM 系统的情况下发送呼叫邀请的错误情况：

- [LOCAL_INVITATION_ERR_NOT_LOGGEDIN](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_local_invitation_error.html#aa717afb5d4809544e6d66e1c0538f2eb)


## 0.9.1 版

该版本于 2019 年 4 月 4 日发布。

> 本版本不包含 `setLogFile` 和 `setLogFilter` 方法。所有的日志信息默认保存在 **/sdcard/\<AppName\>/agorartm.log** 。

### 新增功能

本版本增加了呼叫邀请功能。结合音视频一对一或一对多通话场景，你可以创建、发送、取消、接受或拒绝一个呼叫邀请。

### 性能改进

- 优化了对象关系帮助开发者快速搭建应用。
- 重命名部分接口以遵循 Java 命名规范。
- 删除状态 `ChannelMessageState` 和 `PeerMessageState` 以简化发送频道消息和点对点消息流程。以错误码 `ChannelMessageError` 和 `PeerMessageError` 代替。
- 删除用于监听消息状态的 `IStateListener` 类。改用 `ResultCallback` 类返回成功和错误回调。

### API 变更

#### 新增

- [LocalInvitation.setContent()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html#a4cec28ff6d356242329b1034c7531445): 用于主叫设置发出的呼叫邀请内容。
- [LocalInvitation.getContent()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html#a97294ce1b9b591f9d93e497869b1ad90): 用于主叫获取发出的呼叫邀请内容。 
- [LocalInvitation.getResponse()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html#a268c738458538a266d440b0e281328ee): 用于主叫获取被叫对发出呼叫邀请的响应内容。
- [LocalInvitation.getState()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html#a59608fbac8050f17ec0f855f28598d20): 用于主叫获取发出的呼叫邀请的状态。 
- [RemoteInvitation.getCallerId()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#ae38c5740aa9edb09749f0febb2663926): 用于被叫获取主叫的用户 ID。
- [RemoteInvitation.getContent()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#aeca3b3e981c69c44c7a618d4fdfb3b87): 用于被叫获取主叫设置的呼叫邀请内容。
- [RemoteInvitation.setResponse()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#a229b8cf773eaa0e79b0d67815fd6b6f1): 用于被叫设置呼叫邀请响应。 
- [RemoteInvitation.getResponse()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#a12a70e7d8a77eee21d37bbb65b2f9d3e): 用于被叫获取设置的呼叫邀请响应。
- [RemoteInvitation.getState()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#af77a4afabb19ff1468edf29720361a0f): 用于被叫获取收到的呼叫邀请的状态。
- [RtmCallEventListener.onLocalInvitationReceivedByPeer()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a24e1cb71d3e752963da49bdf91847788): 被叫收到呼叫邀请回调。 
- [RtmCallEventListener.onLocalInvitationAccepted()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a4dece02a62a187a66c2415fecf6b75dc): 被叫已接受呼叫邀请回调。 
- [RtmCallEventListener.onLocalInvitationRefused()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a6224643c400268d356cb5d489825bdd0): 被叫已拒绝呼叫邀请回调。 
- [RtmCallEventListener.onLocalInvitationCanceled()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#ae3164e81772cd4d6171165b1705adcaa): 呼叫邀请已取消回调。 
- [RtmCallEventListener.onLocalInvitationFailure()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#acfefb97eaca497cbd71a0c1cbf5067b0): 发出的呼叫邀请进程失败回调。 
- [RtmCallEventListener.onRemoteInvitationReceived()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a8d01498a993c4016aa45ccb9bf4e9097): 收到呼叫邀请回调。 
- [RtmCallEventListener.onRemoteInvitationAccepted()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a81d9d3de89d08c41408d8a94c8309d29): 已接受呼叫邀请回调。 
- [RtmCallEventListener.onRemoteInvitationRefused()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a7a21eaa9ff49bcf39e3c49b94f6e6ac7): 已拒绝呼叫邀请回调。 
- [RtmCallEventListener.onRemoteInvitationCanceled()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a9d0409c87455d4d2b1315f67a5f7aa12): 主叫已取消呼叫邀请回调。 
- [RtmCallEventListener.onRemoteInvitationFailure()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a6f9f2bbbfbcb0a766c6f1b2e4a8314a1): 发给被叫的呼叫邀请进程失败回调。 
- [RtmCallManager.setEventListener()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a934eee4922584707a1a7ef9ac6999cf2): 设置 `RtmCallManager` 实例的事件监听。
- [RtmCallManager.createLocalInvitation()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a1756dca077267acaa407c6901daa2248): 创建呼叫邀请。 
- [RtmCallManager.sendLocalInvitation()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#af899697061305ca840e829b92c78e353): 向指定用户发送呼叫邀请。 
- [RtmCallManager.acceptRemoteInvitation()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a5f6f97c84e426e2fbd8a5dda71e2fc6c): 用于被叫接受呼叫邀请。 
- [RtmCallManager.refuseRemoteInvitation()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a2ce4af944183976d18c055816f756bf6): 用于被叫拒绝呼叫邀请。 
- [RtmCallManager.cancelLocalInvitation()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a5f03bfe1cfd6987fbc7b5a4dc484f564): 用于主叫取消呼叫邀请。 
- [RtmStatusCode#LocalInvitationState](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_local_invitation_state.html): 返回给主叫的呼叫邀请状态码。
- [RtmStatusCode#RemoteInvitationState](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_remote_invitation_state.html): 返回给被叫的呼叫邀请状态码。 
- [RtmStatusCode#LocalInvitationError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_local_invitation_error.html): 返回给主叫的呼叫邀请错误码。 
- [RtmStatusCode#RemoteInvitationError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_remote_invitation_error.html): 返回给主叫的呼叫邀请错误码。 
- [RtmStatusCode#InvitationApiCallError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_invitation_api_call_error.html): 呼叫邀请相关 API 方法调用的错误码。 
- [ConnectionChangeReason#CONNECTION_CHANGE_REASON_REMOTE_LOGIN](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_connection_change_reason.html): 另一个实例已用相同的用户 ID 登录 Agora RTM 系统。

#### 重命名

- 将用于释放 `RtmClient` 实例的所有资源的 `RtmClient.destroy()` 方法更名为 [RtmClient.release()](../../API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html.md) 。
- 将 `IResultCallback` 类更名为 [ResultCallback](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html) 类。

#### 删除

- 从 [PeerMessageError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_peer_message_error.html) 删除 `PEER_MESSAGE_RECEIVED_BY_SERVER` ，改用错误码  `PEER_MESSAGE_ERR_OK` 。
- 从 [ChannelMessageError](../../API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_channel_message_error.html.md) 删除 `CHANNEL_MESSAGE_RECEIVED_BY_SERVER`  ，改用错误码 `CHANNEL_MESSAGE_OK` 。
- 删除 `PeerMessageState` 接口，改用 [PeerMessageError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_peer_message_error.html) 。
- 删除 `ChannelMessageState` 接口，改用 [ChannelMessageError](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_channel_message_error.html) 返回频道消息错误码。
- 删除用于监听消息状态的 `IStateListener` 类，改用 [ResultCallback](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html) 类监听点对点消息和频道消息结果。
  - 成功：SDK 返回 [ResultCallback.onSuccess()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5) 回调。
  - 失败：SDK 返回 [ResultCallback.onFailure()](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396) 回调以及相应的错误码

## 0.9.0 版

该版本于 2019 年 2 月 4 日发布。

首次发布。

关键功能

- 发送或接收点对点消息。
- 加入或离开频道。
- 发送或接收频道消息。
