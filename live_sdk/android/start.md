{{indexmenu_n>3}}

\#\#\# 开发环境

\- Android Studio 集成 IDE
开发平台，\[下载\](<http://developer.android.com/intl/zh-cn/sdk/index.html>)
- Android 开发
SDK，\[下载\](<https://developer.android.com/intl/zh-cn/sdk/index.html#Other>)

\#\#\# 系统要求

\- Android 4.4 (API 19) 及其以上

\#\#\# 文件说明

SDK 主要包含 demo 代码、jar 包，以及依赖的资源文件，说明如下：

|                      |          |       |             |
| -------------------- | -------- | ----- | ----------- |
| 文件名称                 | 功能       | 大小    | 备注          |
| ustreaming-x.x.x.jar | SDK 库    | 209KB | 必须依赖        |
| assets               | 内置美颜资源文件 | 6KB   | 不使用内置美颜可以去掉 |

\#\#\# 添加依赖

打开工程目录下的 \`build.gradle\`，添加如下依赖：

\`\`\`java dependencies {

``` 
  implementation files('libs/ustreaming-x.x.x.jar')
```

} \`\`\`

\#\#\# 添加权限

在 AndroidManifest.xml 中添加权限：

\`\`\`java \<uses-permission android:name="android.permission.CAMERA"
/\> \<uses-permission android:name="android.permission.FLASHLIGHT" /\>
\<uses-permission android:name="android.permission.RECORD\_AUDIO" /\>
\<uses-permission android:name="android.permission.INTERNET" /\>
\<uses-permission
android:name="android.permission.READ\_EXTERNAL\_STORAGE" /\>
\<uses-permission
android:name="android.permission.WRITE\_EXTERNAL\_STORAGE" /\> \`\`\`

\#\#\# 初始化 SDK

\`\`\`java // 初始化 SDK 运行环境，建议在应用启动时调用 UStreamingEnv.init(context);
\`\`\`

\#\#\# 创建推流核心类对象

\`\`\`java UStreamingManager mStreamingManager = new
UStreamingManager(); mStreamingManager.setStreamingStateListener(this);
// 推流过程中状态信息回调 mStreamingManager.setVideoFrameListener(this); // 视频数据回调
mStreamingManager.setAudioFrameListener(this); // 音频数据回调
mStreamingManager.setStatisticsInfoListener(this); // 推流过程中实时码率、帧率信息回调
\`\`\`

\#\#\# 配置采集参数

\`\`\`java // 摄像头采集参数 UCameraParam cameraParam = new UCameraParam();
cameraParam.setCameraId(UCameraParam.CAMERA\_FACING\_ID.FRONT);
cameraParam.setCameraPreviewSizeRatio(UCameraParam.CAMERA\_PREVIEW\_SIZE\_RATIO.RATIO\_16\_9);
cameraParam.setCameraPreviewSizeLevel(UCameraParam.CAMERA\_PREVIEW\_SIZE\_LEVEL.SIZE\_480P);
// 麦克风采集参数 UMicrophoneParam microphoneParam = new UMicrophoneParam(); //
视频编码参数 UVideoEncodeParam videoEncodeParam = new UVideoEncodeParam();
videoEncodeParam.setEncodingSizeLevel(UVideoEncodeParam.VIDEO\_ENCODING\_SIZE\_LEVEL.SIZE\_480P\_16\_9);
// 480x848 videoEncodeParam.setEncodingBitrate(800\*1024); // 800kbps //
音频编码参数 UAudioEncodeParam audioEncodeParam = new UAudioEncodeParam();
\`\`\`

\#\#\# 创建预览窗口

视频采集需要的预览窗口为 \`GLSurfaceView\` 或者其派生的类对象，可以配置在 Layout 文件中，也可以动态创建。

\#\#\# 配置推流参数

通过 \`prepare\` 把推流需要的参数配置到 SDK 中：

\`\`\`java // 如果不使用内置美颜，beautyParam 可以传递 null
mStreamingManager.prepare(preview, cameraParam, microphoneParam,
videoEncodeParam, audioEncodeParam, beautyParam); \`\`\`

\#\#\# 开始推流

\`\`\`java // 通过 streamingUrl 传入 RTMP 推流地址
mStreamingManager.startPublish(streamingUrl); \`\`\`
