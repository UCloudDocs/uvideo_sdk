{{indexmenu_n>4}}
\`\`\`objectivec @class MLController;

@protocol MLControllerDelegate\<NSObject\>

@optional

/\*\* \* @brief 当 \`MLController\` 状态发生改变时被调用 \* @param controller
调用该方法的 MLController 对象 \* @param state 推流控制器的新状态 \* @param
error 当推流控制器状态转变为 \`MLStreamStateError\` 时，error 才不为 nil \*/ -
(void)controller:(MLController \*)controller
stateDidChange:(MLState)state error:(NSError \*)error;

/\*\* \* @brief 当新的视频数据产生后将调用该方法 \* @param controller 调用该方法的
\`MLController\` 对象 \* @param videoData 视频数据 \* @return 处理完成的视频数据 \*/ -
(CVPixelBufferRef)controller:(MLController \*)controller
didGetVideoData:(CVPixelBufferRef)videoData;

/\*\* \* @brief 当新的音频数据产生后将调用该方法 \* @param controller 调用该方法的
\`MLController\` 对象 \* @param videoData 音频数据 \* @return 处理完成的音频数据 \*/ -
(AudioBufferList \*)controller:(MLController \*)controller
didGetAudioData:(AudioBufferList \*)audioData;

@end

@interface MLController : NSObject

/\*\* \* @brief SDK 的日志等级,默认为 MLStreamLogLevelWarning \* @warning 请不要在
release 版本中使用 MLLogLevelVerbose，这会导致性能下降。 \*/ +
(void)setLogLevel:(MLLogLevel)logLevel;

/\*\* \* @brief 摄像头预览视图 \*/ @property(nonatomic, strong, readonly)
UIView \*preview;

/\*\* \* @brief 推流控制器 \*/ @property(nonatomic, assign, readonly) MLState
state;

/\*\* \* @brief 推流服务器的地址 \*/ @property(nonatomic, strong, readonly)
NSURL \*URL;

/\*\* \* @brief 视频内容和视图的比例不一致时的填充方式 \* 默认为 MovieousScalingModeAspectFit
\*/ @property(nonatomic, assign) MovieousScalingMode scalingMode;

/\*\* \* @brief 代理对象 \* 默认为 nil \*/ @property(nonatomic, weak) id
\<MLControllerDelegate\> delegate;

/\*\* \* @brief 当 dynamicFrameRate 开启时，推流的帧率会根据网络状况进行动态调整。 \* 默认为 NO \*/
@property(nonatomic, assign) BOOL dynamicFrameRate;

/\*\* \* @discussion 当 autoReconnect 打开时，如果推流异常终止，MLController 会进入
\`MLStreamStateReconnecting\` 状态，然后回尝试三次重新连接，从 0\~10
秒随机递增，如果依然无法连接成功，\`MLStreamStateError\`
会转为 \`MLStreamStateError\` 状态。 \* 默认为 YES \*/ @property(nonatomic,
assign) BOOL autoReconnect;

/\*\* \* @brief 打开网络监控时，\`MLController\` 会在网络发生切换时自动尝试连接。 \* 默认为 YES \*/
@property (nonatomic, assign) BOOL monitorNetwork;

/\*\* \* @brief 当 \`monitorNetwork\` 打开时，如果网络发生切换
\`connectionChangeActionHandler\` 将会被调用，如果你想继续重连，返回 YES，如果想取消重连则返回 NO。
\*/ @property (nonatomic, copy) ConnectionChangeActionHandler
connectionChangeActionHandler;

/\*\* \* @brief 使用推流服务器地址来初始化 \`MLController\` 对象 \* @param URL 推流服务器的地址
\* @return 返回初始化完成的 \`MLController\` 对象 \* @warning 如果传入的地址未授权，该方法将返回
nil \*/ - (instancetype)initWithURL:(NSURL \*)URL;

/\*\* \* @brief 使用推流服务器地址及音频和视频配置来初始化 \`MLController\` 对象 \* @param URL
推流服务器的地址 \* @param audioConfiguration 音频配置，传入 nil 表示使用默认配置 \* @param
videoConfiguration 视频配置，传入 nil 表示使用默认配置 \* @return 返回初始化完成的
\`MLController\` 对象 \* @warning 如果传入的地址未授权，该方法将返回 nil \*/ -
(instancetype)initWithURL:(NSURL \*)URL
audioConfiguration:(MLAudioConfiguration \*)audioConfiguration
videoConfiguration:(MLVideoConfiguration \*)videoConfiguration
NS\_DESIGNATED\_INITIALIZER;

/\*\* \* @brief 开始采集，调用此方法将申请音视频的权限。 \* @param completion 闭包会在操作完成后被调用
\*/ - (void)startCapturingWithCompletion:(void (^)(AVAuthorizationStatus
cameraAuthorizationStatus, AVAuthorizationStatus
microphoneAuthorizationStatus, NSError \*error))completion;

/\*\* \* @brief 暂停音频采集 \*/ - (void)pauseAudioCapturing;

/\*\* \* @brief 恢复音频采集 \*/ - (void)resumeAudioCapturing;

/\*\* \* @brief 暂停视频采集 \*/ - (void)pauseVideoCapturing;

/\*\* \* @brief 恢复视频采集 \*/ - (void)resumeVideoCapturing;

/\*\* \* @brief 开始推流 \* @param completion 闭包会在操作完成后被调用 \*/ -
(void)startBroadcastingWithCompletion:(void (^)(NSError
\*error))completion;

/\*\* \* @brief 停止推流 \*/ - (void)stopBroadcasting;

@end

@interface MLController(Video)

/\*\* \* @brief 前置摄像头预览是否镜像 \* 默认为 MLVideoConfiguration 的配置 \*/
@property (nonatomic, assign) BOOL mirrorFrontPreview;

/\*\* \* @brief 后置摄像头预览是否镜像 \* 默认为 MLVideoConfiguration 的配置 \*/
@property (nonatomic, assign) BOOL mirrorBackPreview;

/\*\* \* @brief 前置摄像头推流是否镜像 \* 默认为 MLVideoConfiguration 的配置 \*/
@property (nonatomic, assign) BOOL mirrorFrontStream;

/\*\* \* @brief 后置摄像头推流是否镜像 \* 默认为 MLVideoConfiguration 的配置 \*/
@property (nonatomic, assign) BOOL mirrorBackStream;

/\*\* \* @brief 是否开启触摸对焦 \* 默认为 MLVideoConfiguration 的配置 \*/ @property
(nonatomic, assign) BOOL touchToFocus;

/\*\* \* @brief 是否开启手电筒 \* 默认为 MLVideoConfiguration 的配置 \*/ @property
(nonatomic, assign, readonly) BOOL torchOn; -
(BOOL)setTorchOn:(BOOL)torchOn error:(NSError \*\*)error;

/\*\* \* @brief 是否开启持续自动对焦 \* 默认为 MLVideoConfiguration 的配置 \*/ @property
(nonatomic, assign, readonly) BOOL continuousAutofocus; -
(BOOL)setContinuousAutofocus:(BOOL)continuousAutofocus error:(NSError
\*\*)error;

/\*\* \* @brief 帧率 \* 默认为 MLVideoConfiguration 的配置 \*/ @property
(nonatomic, assign, readonly) NSUInteger frameRate; -
(BOOL)setFrameRate:(NSUInteger)frameRate error:(NSError \*\*)error;

/\*\* \* @brief 采集分辨率 \* 默认为 MLVideoConfiguration 的配置 \*/ @property
(nonatomic, strong, readonly) AVCaptureSessionPreset cameraResolution; -
(BOOL)setCameraResolution:(AVCaptureSessionPreset)cameraResolution
error:(NSError \*\*)error;

/\*\* \* @brief 摄像头位置 \* 默认为 MLVideoConfiguration 的配置 \*/ @property
(nonatomic, assign, readonly) AVCaptureDevicePosition cameraPosition; -
(BOOL)setCameraPosition:(AVCaptureDevicePosition)cameraPosition
error:(NSError \*\*)error;

/\*\* \* @brief 摄像头旋转方向 \* 默认为 MLVideoConfiguration 的配置 \*/ @property
(nonatomic, assign, readonly) AVCaptureVideoOrientation
cameraOrientation; -
(BOOL)setCameraOrientation:(AVCaptureVideoOrientation)cameraOrientation
error:(NSError \*\*)error;

/\*\* \* @brief 控制摄像头变焦系数 \* 默认为 MLVideoConfiguration 的配置 \*/ @property
(nonatomic, assign, readonly) CGFloat cameraZoomFactor; -
(BOOL)setCameraZoomFactor:(CGFloat)cameraZoomFactor error:(NSError
\*\*)error;

/\*\* \* @brief 切换摄像头 \*/ - (BOOL)switchCameraWithError:(NSError
\*\*)error;

@end

@interface MLController(Audio)

/\*\* \* @brief 是否开启耳返 \* 默认为 NO \*/ @property (nonatomic, assign) BOOL
headphoneMonitor;

@end \`\`\`

\#\#\# 音频参数类（MLAudioConfiguration） \`\`\`objectivec @interface
MLAudioConfiguration : NSObject \< NSCopying \>

/\*\* \* @brief 是否开启音频流 \* 默认为 YES \*/ @property (nonatomic, assign)
BOOL enable;

/\*\* \* @brief 音频输入源 \* 默认为 MLAudioSourceMicrophone \*/ @property
(nonatomic, assign) MLAudioSource source;

/\*\* \* @brief 是否允许和其他音频混合 \* 默认为 YES \*/ @property (nonatomic, assign)
BOOL allowMixWithOthers;

/\*\* \* @brief 是否开启回声消除 \* 默认为 NO \*/ @property (nonatomic, assign)
BOOL acousticEchoCancellationEnable;

/\*\* \* @brief 音频编码比特率 \* 默认为 MLAudioBitRate96Kbps \*/ @property
(nonatomic, assign) MLAudioBitRate audioBitRate;

/\*\* \* @brief 创建一个默认的配置对象 \* @return 创建完成的默认配置对象 \*/ +
(instancetype)defaultConfiguration;

@end \`\`\`

\#\#\# 视频参数累（MLVideoConfiguration） \`\`\`objectivec @interface
MLVideoConfiguration : NSObject \< NSCopying \>

/\*\* \* @brief 是否开启视频 \* 默认为 YES \*/ @property (nonatomic, assign) BOOL
enable;

/\*\* \* @brief 视频输入源 \*/ @property (nonatomic, assign) MLVideoSource
source;

/\*\* \* @brief 视频帧率 \* 默认为 30 \*/ @property (nonatomic, assign)
NSUInteger frameRate;

/\*\* \* @brief 前置摄像头预览是否镜像 \* 默认为 MLVideoConfiguration 的配置 \*/
@property (nonatomic, assign) BOOL mirrorFrontPreview;

/\*\* \* @brief 后置摄像头预览是否镜像 \* 默认为 MLVideoConfiguration 的配置 \*/
@property (nonatomic, assign) BOOL mirrorBackPreview;

/\*\* \* @brief 前置摄像头推流是否镜像 \* 默认为 MLVideoConfiguration 的配置 \*/
@property (nonatomic, assign) BOOL mirrorFrontStream;

/\*\* \* @brief 后置摄像头推流是否镜像 \* 默认为 MLVideoConfiguration 的配置 \*/
@property (nonatomic, assign) BOOL mirrorBackStream;

/\*\* \* @brief 摄像头采集分辨率 \* 默认为 AVCaptureSessionPresetHigh \*/ @property
(nonatomic, copy) AVCaptureSessionPreset cameraResolution;

/\*\* \* @brief 摄像头位置 \* 默认为 AVCaptureVideoOrientationPortrait \*/
@property (nonatomic, assign) AVCaptureDevicePosition cameraPosition;

/\*\* \* @brief 摄像头转向 \* 默认为 AVCaptureDevicePositionFront \*/ @property
(nonatomic, assign) AVCaptureVideoOrientation cameraOrientation;

/\*\* \* @brief H.264 的 profile 和 level，参考
<https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC> 了解更多信息 \* 默认为
AVVideoProfileLevelH264HighAutoLevel \*/ @property (nonatomic, copy)
NSString \*profileLevel;

/\*\* \* @brief 视频编码尺寸 \* @discussion
需要注意的是如果编码的视频尺寸和采集视频尺寸不一定一样，如果采集尺寸和编码尺寸比例不相同，编码器将使用
AVVideoScalingModeResizeAspectFill 的方式来填充 \* 默认为 (640, 480) \*/
@property (nonatomic, assign) CGSize size;

/\*\* \* @brief 视频编码的最大 GOP, 参考
<https://en.wikipedia.org/wiki/Group_of_pictures> 以获取更多信息 \*
默认为视频采集帧率的三倍 \*/ @property (nonatomic, assign)
NSUInteger gop;

/\*\* \* @brief 编码视频的平均码率 \* 默认为 1 mbps \*/ @property (nonatomic,
assign) NSUInteger averageBitRate;

/\*\* \* @brief 创建默认配置对象 \* @return 创建完成的默认配置对象 \*/ +
(instancetype)defaultConfiguration;

@end \`\`\`

\#\#\# 类型定义（MLTypeDefines） \`\`\`objectivec /** \* @brief 视频源的类型 \*/
typedef NS\_ENUM(NSInteger, MLVideoSource) { /**

``` 
   * @brief 摄像头采集
   */
  MLVideoSourceCamera,
```

};

/** \* @brief 音频源类型 \*/ typedef NS\_ENUM(NSInteger, MLAudioSource) { /**

``` 
   * @brief 麦克风采集
   */
  MLAudioSourceMicrophone,
```

};

/** \* @brief 编码的音频码率 \*/ typedef NS\_ENUM(NSInteger, MLAudioBitRate) {
/**

``` 
   * @brief 64 Kbps 码率
   */
  MLAudioBitRate64Kbps = 64000,
  /**
   * @brief 96 Kbps 码率
   */
  MLAudioBitRate96Kbps = 96000,
  /**
   * @brief 128 Kbps 码率
   */
  MLAudioBitRate128Kbps = 128000,
```

};

/** \* @brief 日志级别 \*/ typedef NS\_ENUM(NSUInteger, MLLogLevel){ /**

``` 
   * @brief 不打印日志
   */
  MLLogLevelOff       = 0,
  /**
   * @brief 只打印 error 日志
   */
  MLLogLevelError,
  /**
   * @brief 打印 error and warning 日志
   */
  MLLogLevelWarning,
  /**
   * @brief 打印 error, warning and info 日志
   */
  MLLogLevelInfo,
  /**
   * @brief 打印 error, warning, info and debug 日志
   */
  MLLogLevelDebug,
  /**
   * @brief 打印 error, warning, info, debug and verbose 日志
   */
  MLLogLevelVerbose,
```

};

typedef NS\_ENUM(NSInteger, MLState) {

``` 
  /**
   * @brief 初始状态
   */
  MLStateInitial,
  /**
   * @brief 正在链接服务器
   */
  MLStateConnecting,
  /**
   * @brief 服务器已连接
   */
  MLStateConnected,
  /**
   * @brief 正在断开服务器
   */
  MLStateDisconnecting,
  /**
   * @brief 服务器已断开
   */
  MLStateDisconnected,
  /**
   * @brief 正在进行服务器重连接
   */
  MLStateReconnecting,
  /**
   * @brief 推流发生错误
   */
  MLStateError,
  /**
   * @brief 正在采集，而未开始推流
   */
  MLStateCapturing,
```

};

/** \* @brief Network transition type \*/ typedef NS\_ENUM(NSUInteger,
MLNetworkStateTransition) { /**

``` 
   * @brief Unknown transition
   */
  MLNetworkStateTransitionUnknown = 0,
  /**
   * @brief Transition from no network to WiFi
   */
  MLNetworkStateTransitionUnconnectedToWiFi,
  /**
   * @brief Transition from no network to WWAN(4G, 3G, etc)
   */
  MLNetworkStateTransitionUnconnectedToWWAN,
  /**
   * @brief Transition from WiFi to no network
   */
  MLNetworkStateTransitionWiFiToUnconnected,
  /**
   * @brief Transition from WWAN(4G, 3G, etc) to no network
   */
  MLNetworkStateTransitionWWANToUnconnected,
  /**
   * @brief Transition from WiFi to WWAN(4G, 3G, etc)
   */
  MLNetworkStateTransitionWiFiToWWAN,
  /**
   * @brief Transition from WWAN(4G, 3G, etc) to WiFi
   */
  MLNetworkStateTransitionWWANToWiFi,
```

};

/\*\* \* @brief 接收网络状态切换状态变更通知的闭包 \*/ typedef
BOOL(^ConnectionChangeActionHandler)(MLNetworkStateTransition
transition);

\`\`\`
