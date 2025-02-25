
---
title: 当使用命令行进行录制时，如何控制录制进程
description: How to stop recording in cmd line
platform: Linux
updatedAt: Mon Aug 12 2019 10:51:37 GMT+0800 (CST)
---
# 当使用命令行进行录制时，如何控制录制进程
### 开始和暂停录制

如果录制模式为自动（默认），录制实例加入频道后，监测到频道內有用户即开始录制，不支持暂停录制。

如果录制模式为手动（`triggerMode` 设为 1），可以用以下方法控制录制进程：

- 控制所有录制进程：
  - 开始录制：`killall -s 10 recorder_local`
  - 暂停录制：`killall -s 12 recorder_local`
- 控制单个录制进程：
  1. 获取对应频道号的进程号：`ps aux | grep 'channelName'`
  2. 控制单个录制进程（PID 为获取的进程号）
     - 开始录制：`kill -s 10 PID`
     - 暂停录制：`kill -s 12 PID`

### 停止录制

频道内无用户的状态超过空闲频道超时退出时间（`idle` 设置的值），录制程序会自动退出。默认值为 300 秒。

如果想主动停止录制，可以在键盘上同时按下 **Ctrl** 和 **C** 键结束当前进程。
