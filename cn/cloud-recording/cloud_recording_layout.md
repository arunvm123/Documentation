
---
title: 设置合流布局
description: 
platform: Linux
updatedAt: Mon Sep 30 2019 09:50:46 GMT+0800 (CST)
---
# 设置合流布局
## 功能描述

当一个频道中有多个用户时，Agora 云端录制会进行**合流转码**，把多路音视频流混合成一路。

下图为合流后的视频示例。我们将视频的背景称为**画布**，每个用户的视频占据一个**画面**。

![](https://web-cdn.agora.io/docs-files/1562906845265)

对于合流录制，你需要设置频道内用户画面的大小及其在视频画布上的位置，进行**合流布局**。

Agora 云端录制支持两种合流布局的方式：

- [**选择预设的合流布局样式**](#predefined)：Agora 云端录制服务提供悬浮布局、自适应布局和垂直布局三种布局样式，预设用户画面的大小和位置，无需你自行设定。你只需选择其中一种样式，就能实现预设的布局效果。
- [**自定义合流布局**](#custom)：灵活设置用户画面的大小，指定用户画面在视频画布上的相对位置。

录制方式与合流布局方式的具体实现如下：

|                       | 预设合流布局 | 自定义合流布局 |
| :-------------------- | :----------- | :------------- |
| 调用 API 录制         | 支持         | 不支持         |
| 命令行录制            | 支持         | 不支持         |
| 调用 RESTful API 录制 | 支持         | 支持           |

下文具体介绍两种合流布局方式。

> - 合流视频画布上最多显示 17 个用户画面。
> - 如果某用户只发送音频，不发送视频，仍然会占据一个画面。

## <a name="predefined"></a>选择预设的合流布局样式

Agora 云端录制服务提供三种预设的合流布局样式：

- [悬浮布局](#float)（默认）。第一个加入频道的用户在画布上会显示为大视窗，铺满整个画布；其他用户按照加入的顺序从画布左下角开始依次水平排列，显示为小视窗，最多 4 行，每行 4 个画面。小视窗会悬浮在大视窗上面。最多支持共 17 个用户画面。
- [自适应布局](#bestfit)。用户画面平铺在画布上。根据用户的数量自动调整每个用户画面的大小，每个用户画面的大小一致。最多支持 17 个用户画面。
- [垂直布局](#vertical)。指定一个用户在画布左侧显示大视窗画面，其他用户的小视窗画面在右侧垂直排列，最多两列，每列最多 8 个画面。最多支持共 17 个用户画面。

### 实现方法

通过设置合流布局参数选择一种预设的布局，0 为悬浮布局，1 为自适应布局，2 为垂直布局。

- 如果你使用云端录制 SDK [调用 API 录制](../../cn/cloud-recording/cloud_recording_quickstart.md)，在开始云端录制时设置 [`TranscodingConfig`](../../cn/cloud-recording/cloud_recording_api.md) 中的 `layout` 参数

- 如果你使用 Demo 通过[命令行录制](../../cn/cloud-recording/cloud_recording_demo.md)，在开始云端录制时设置 `--mixedVideoLayoutType` 参数

- 如果你使用 [RESTful API 进行录制](../../cn/cloud-recording/cloud_recording_rest.md)，你可以：

  - 在开始云端录制时设置 `transcodingConfig` 里的 `mixedVideoLayout` 参数
  - 在录制过程中随时调用 `updateLayout` 方法传入 `mixedVideoLayout` 参数

### 预设布局样式介绍

本节详细介绍三种预设布局样式。

下列布局示例中的数字表示用户加入频道的顺序。如果用户 1 退出频道，用户 2 会占据用户 1 的画面，依次替补。

#### <a name="float"></a>悬浮布局

默认布局方式。第一个加入频道的用户在屏幕上会显示为大视窗，铺满整个画布；其他用户按照加入的顺序从画布左下角开始依次水平排列，显示为小视窗，最多 4 行，每行 4 个画面。小视窗会悬浮在大视窗上面。

对于悬浮布局，有以下注意事项：

- 如果某用户只发送音频，仍然会占据布局位，显示大视窗画面。
- 如果用户的实际视频流分辨率与视窗的长宽比不一致，视频会裁剪以适配视窗的大小。
- 每个小画面的宽和高分别为整个画布宽和高的 0.235，相邻小画面的左右和上下间距分别为整个画布宽和高的 0.012。小画面距离画布的水平和垂直边距也分别为整个画布宽和高的 0.012。

不同人数的实际布局效果如下所示。

**1 人**

![](https://web-cdn.agora.io/docs-files/1560823829265)

**2 - 5 人**

![](https://web-cdn.agora.io/docs-files/1560823855095)

**6 - 9 人**

![](https://web-cdn.agora.io/docs-files/1560823875104)

**10 - 13 人**

![](https://web-cdn.agora.io/docs-files/1560823889282)

**14 - 17 人**

![](https://web-cdn.agora.io/docs-files/1560823910236)

#### <a name="bestfit"></a>自适应布局

布局样式会根据频道人数自适应。每个用户画面平铺在画布上，大小一致。

对于自适应布局，有以下注意事项：

- 在下面不同人数的合流布局下，如果频道中人数不足，则剩余的布局位置会空着，显示背景色。
- 如果某用户只发送音频，仍然会占据布局，显示背景色。
- 若实际视频流分辨率与视窗的长宽比不一致，视频画面会裁剪以适配视窗的大小。

不同人数的实际布局效果如下所示。

**1- 4 人**

![](https://web-cdn.agora.io/docs-files/1558062852403)

![](https://web-cdn.agora.io/docs-files/1558063212804)

![](https://web-cdn.agora.io/docs-files/1560824305413)

![](https://web-cdn.agora.io/docs-files/1558063229612)

**5 - 9 人**

![](https://web-cdn.agora.io/docs-files/1560824329767)

**10 - 16 人**

![](https://web-cdn.agora.io/docs-files/1560824340228)

**17 人**

17 人的画面布局与上面的情况不同，用户画面没有铺满整个画布。

每个用户画面的宽和高分别为总宽度和总高度的 0.2，前四行每行四个用户，画布左右边距均为总宽度的 0.1。第五行居中显示第 17 个用户画面。

![](https://web-cdn.agora.io/docs-files/1560824353055)

#### <a name="vertical"></a>垂直布局

如果你选择了垂直布局，需要指定一个 UID 作为大视窗画面。
- 如果你使用云端录制 SDK [调用 API 录制](../../cn/cloud-recording/cloud_recording_quickstart.md)，在开始云端录制时设置 [`TranscodingConfig`](../../cn/cloud-recording/cloud_recording_api.md) 中的 `max_resolution_uid` 参数指定；
- 如果你使用 demo 通过[命令行录制](../../cn/cloud-recording/cloud_recording_demo.md)，在开始云端录制时设置 `—maxResolutionUid` 参数指定。
- 如果你使用 [RESTful API 进行录制](../../cn/cloud-recording/cloud_recording_rest.md)，在开始云端录制时或调用 `updateLayout` 方法时传入 `maxResolutionUid` 参数指定。

对于垂直布局，有以下注意事项：
- Large 表示大视窗画面，显示指定的 UID 用户的视频.
 - 如果未指定或者指定用户未进入频道，Large 区域显示背景色。
 - 如果实际视频流分辨率与视窗的长宽比不一致，视频画面会缩放以保证视频的完整性。
- 小视窗画面的排列顺序，是按照加入频道的时间先后顺序。
 - 如果用户 small 1 退出频道，用户 small 2 会占据用户 small 1 的视窗，依次替补。
 - 如果实际视频流分辨率与视窗的长宽比不一致，视频会裁剪以适配视窗的大小。
- 在下面不同人数的合图布局下，若频道中人数不足，则剩余的布局位置会空着，显示背景色。
- 如果某用户只发送音频，仍然会占据布局位，显示背景色。

**1 - 5 人**

small 1 - small 4 依次显示在画布右侧, 且不会覆盖 Large 画面。小视窗画面宽度是总宽的 1/5，画面高度是总高的 1/4。

![](https://web-cdn.agora.io/docs-files/1558060680455)

**6 - 7 人**

small 1 - small 6 依次显示在画布右侧, 且不会覆盖 Large 画面。小视窗画面宽度是总宽的 1/7，画面高度是总高的 1/6。

![](https://web-cdn.agora.io/docs-files/1558060697541)

**8 - 9 人**

small1 - small 8 依次显示在画布右侧, 且不会覆盖 Large 画面。小视窗画面宽度是总宽的 1/9，画面高度是总高的 1/8。

![](https://web-cdn.agora.io/docs-files/1558060714296)

**10 - 17 人**

small 1 - small 16 依次显示在画布右侧, 且不会覆盖 Large 画面。小视窗画面宽度是总宽的 1/10，画面高度是总高的 1/8。

![](https://web-cdn.agora.io/docs-files/1558060732460)


## <a name="custom"></a>自定义合流布局

如果你使用 RESTful API 通过网络请求进行云录制，你可以自定义合流布局，灵活设置用户画面的大小，指定用户画面在视频画布上的相对位置。

### 实现方法

#### 开始录制时自定义合流布局

获取 resource ID 后，调用 [start](../../cn/cloud-recording/cloud_recording_api_rest.md) 方法开始录制时在 `clientRequest` 中传入 `mixedVideoLayout` 和 `layoutConfig` 参数。这两个参数的具体位置如下。详见 start 方法的 HTTP [请求示例](https://docs.agora.io/cn/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#start-请求示例)。

```
Body:
{
  ......
  "clientRequest": {
    .......
    "recordingConfig": {
      ........
      "transcodingConfig": {
        .......
        "mixedVideoLayout": 3
        "layoutConfig":[
        {
          ........
        }]
      }
    },
  .......
  }
}
```

`mixedVideoLayout` 参数设为 3（自定义布局）。

`layoutConfig` 是一个由多个用户画面设置组成的数组，最多可以设置 17 个用户画面。`layoutConfig` 中可以传入的参数如下：

| 参数        | 类型   | 描述                                                         |
| :---------- | :----- | :----------------------------------------------------------- |
| `uid`         | String |（选填）待显示在该画面上的用户的 UID。如果不指定 UID，会按照用户加入频道的顺序自动匹配 `layoutConfig` 中的画面设置。 |
| `x_axis`      | Float  |（必填）画布上该画面左上角的横坐标的相对值，范围是  [0.0,1.0]。从左到右布局，0.0 在最左端，1.0 在最右端。 |
| `y_axis`    | Float  |（必填）画布上该画面左上角的纵坐标的相对值，范围是  [0.0,1.0]。从上到下布局，0.0 在最上端，1.0 在最下端。 |
| `width`     | Float  |（必填）画面宽度的相对值，取值范围是 [0.0,1.0]。             |
| `height`      | Float  |（必填）画面高度的相对值，取值范围是 [0.0,1.0]。             |
| `alpha`       | Float  |（选填）画面的透明度。取值范围是 [0.0,1.0] 。0.0 表示图像为透明的，1.0 表示图像为完全不透明的。默认值为 1.0。 |
| `render_mode` | Number |（选填）画面显示模式：<br><li>0：（默认）裁剪模式。<br><li>1：缩放模式。     |

其中，`x_axis`，`y_axis`，`width`，`height` 四个参数为必填，都是 0.0 ~ 1.0 之间的 float 值。

以**视频画布左上角**为原点，`x_axis` 和 `y_axis` 设置用户画面在视频画布上的**相对位置**，分别代表**用户画面左上角**到原点的水平相对距离和垂直相对距离。`width` 和 `height` 设置用户画面的**相对宽度**和**相对高度**，即用户画面的宽度和高度占画布的宽度和高度的比例。确保 x_axis+width &lt;= 1，y_axis+height &lt;=1。

如下图所示：

![](https://web-cdn.agora.io/docs-files/1562917679008)

下面结合示例代码来介绍在实际场景中如何设置 `layoutConfig` 参数。

假设直播间中有 1 个主播，2 个连麦者。主播的 UID 是确定的，但连麦者的 UID 会经常变化。因此我们可以在开始录制时设置 3 个 `layoutConfig` 实现 1 大 2 小的布局，主播占据大画面，连麦者按照时间顺序先后进入小画面。

示例代码：

```
"transcodingConfig": {
  "mixedVideoLayout": 3,
  "backgroudColor": "#FF0000",
  "layoutConfig": [
  {
    "uid": "1",
    "x_axis": 0.0,
    "y_axis": 0.0,
    "width": 1.0,
    "height": 0.5,
    "alpha": 1.0,
    "render_mode": 1
  },
  {
    "x_axis": 0.0,
    "y_axis": 0.5,
    "width": 0.5,
    "height": 0.5,
    "alpha": 1.0,
    "render_mode": 1
  },
  {
    "x_axis": 0.5,
    "y_axis": 0.5,
    "width": 0.5,
    "height": 0.5,
    "alpha": 1.0,
    "render_mode": 1
  }]
}
```

上述示例代码的效果如下：

![](https://web-cdn.agora.io/docs-files/1562917761873)

#### 录制过程中更新合流布局

在录制过程中，如果频道中的用户数量、大小流或用户角色发生变化，你可以随时调用 `updateLayout` 方法更新录制合流布局。详见 [`updateLayout`](https://docs.agora.io/cn/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#a-nameupdatea%E6%9B%B4%E6%96%B0%E5%90%88%E6%B5%81%E5%B8%83%E5%B1%80%E7%9A%84-api) 的参数介绍、完整请求和响应示例。

需要注意的是，调用该方法传入参数会覆盖原来的布局设置。举例来说，如果你在开始录制时设置了 `backgroundColor` 为 "#FF0000"（红色），调用该方法更新合流布局时如果不设置 `backgroundColor` 参数，背景色会变为默认值黑色。

要更新自定义合流布局，你需要将 `clientRequest` 中的 `mixedVideoLayout` 参数设为 3（自定义布局），然后设置 `layoutConfig` 数组。

示例代码：

```
"clientRequest": {
  "mixedVideoLayout": 3,
  "backgroundColor": "#0000FF",
  "layoutConfig": [
  {
    "uid": "1",
    "x_axis": 0.0,
    "y_axis": 0.1,
    "width": 0.1,
    "height": 0.1,
    "alpha": 1.0,
    "render_mode": 1
  },
  {
    "uid": "2",
    "x_axis": 0.2,
    "y_axis": 0.2,
    "width": 0.1,
    "height": 0.1,
    "alpha": 1.0,
    "render_mode": 1
  }]
},
```

### 开发注意事项

- 选填的参数如果没有填写，会自动填入默认值。
- 参数 `mixedVideoLayout` 和 `layoutConfig` 的设置互相影响。
  - `mixedVideoLayout` 不为 3 时，填入 `layoutConfig` 会报错。
  - `mixedVideoLayout` 为 3 时，不填 `layoutConfig` 会报错。

## 设置画布颜色

如果你使用云端录制 SDK [调用 API 录制](../../cn/cloud-recording/cloud_recording_quickstart.md) 或使用 Demo 通过[命令行录制](../../cn/cloud-recording/cloud_recording_demo.md)，视频画布颜色默认为黑色，暂不支持修改。

如果你[使用 RESTful API 进行录制](../../cn/cloud-recording/cloud_recording_rest.md)，你可以在调用 `start` 或 `updateLayout` 方法时传入 `backgroundColor` 参数设置视频画布颜色。支持 RGB 颜色表，字符串格式为 # 号后 6 个十六进制数。默认值 "#000000" 黑色。
