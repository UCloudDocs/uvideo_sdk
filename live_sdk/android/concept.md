{{indexmenu_n>4}}

\#\#\# 设计规则

SDK 的接口设计，遵循了如下的原则：

\- 接口类均以 \`U\` 开头 - 参数类均以 \`UxxxParam\` 命名 - 回调类均以 \`UxxxListener\` 命名

\#\#\#\# 接口类

|                   |              |    |
| ----------------- | ------------ | -- |
| 类名                | 说明           | 备注 |
| UStreamingManager | 负责 RTMP 直播推流 | 无  |

\#\#\#\# 参数类

|                   |             |    |
| ----------------- | ----------- | -- |
| 类名                | 说明          | 备注 |
| UCameraParam      | Camera 采集参数 | 无  |
| UMicrophoneParam  | 麦克风采集参数     | 无  |
| UVideoEncodeParam | 视频编码参数      | 无  |
| UAudioEncodeParam | 音频编码参数      | 无  |

\#\#\#\# 回调类

|                         |                |                |
| ----------------------- | -------------- | -------------- |
| 类名                      | 说明             | 备注             |
| UVideoFrameListener     | 视频数据回调         | 纹理数据，支持第三方特效处理 |
| UAudioFrameListener     | 音频数据回调         | PCM 数据，支持音频处理  |
| UStreamingStateListener | 用于监听推流过程中的状态变化 | 无              |
