{{indexmenu_n>3}}

直播SDK分别使用 \`MLAudioConfiguration\` 和 \`MLVideoConfiguration\`
对象进行音频和视频的参数配置，这两个配置对象都支持使用
\`-defaultConfiguration\`
方法获取一个默认的配置对象，在获取到默认的配置对象之后我们可以根据业务需求对相关参数进行更改供后续使用。
\`\`\`objectivec // 创建默认配置对象 MLAudioConfiguration \*audioConfiguration =
\[MLAudioConfiguration defaultConfiguration\]; MLVideoConfiguration
\*videoConfiguration = \[MLVideoConfiguration defaultConfiguration\]; //
对需要配置的参数进行配置 audioConfiguration.audioBitRate = MLAudioBitRate128Kbps;
videoConfiguration.cameraResolution = AVCaptureSessionPresetHigh; \`\`
\` \#\#\# 创建 \`MLController\`

使用在上一步创建好的 \`MLAudioConfiguration\` 和 \`MLVideoConfiguration\`
对象以及推流服务器的 URL 创建 \`MLController\`
对象，创建成功之后您可以使用该对象进行直播行为的控制，状态获取和参数调整。

\`\`\`objectivec MLController controller = \[\[MLController alloc\]
initWithURL:\[NSURL URLWithString:\_URL\]
audioConfiguration:\_audioConfiguration
videoConfiguration:\_videoConfiguration\]; \`\`\`

\- 注意：创建 \`MLController\` 对象需要保证您已经将推流所使用的域名备案到
\[Movieous\](<https://movieous.cn/>) 否则初始化方法将返回 \`nil\`

\#\#\# 开始采集

开始进行音视频的采集，当开始采集接口被调用之后系统将会向用户申请摄像头和麦克风的权限（如果在相应配置中 \`enable\` 被配置为
\`YES\`）请确保您已经在项目的 \`Info.plist\` 文件中添加了 \`Privacy - Camera Usage
Description\` 以及 \`Privacy - Microphone Usage Description\`
的描述，否则调用该接口将会导致程序崩溃。

\`\`\`objectivec \[\_movieousLiveController
startCapturingWithCompletion:^(AVAuthorizationStatus
cameraAuthorizationStatus, AVAuthorizationStatus
microphoneAuthorizationStatus, NSError \*error) {

``` 
  // 做出相应响应
```

}\]; \`\`\`

\#\#\# 添加预览视图到当前视图的子视图

\`MLController\` 提供 \`@property(nonatomic, strong, readonly) UIView
\*preview\` 的属性用于预览摄像头采集的数据，您可以将它加入到您当前视图的子视图并调整它的大小以开始预览摄像头的输入。
\`\`\`objectivec \[self.view addSubview:controller.preview\];
controller.preview.frame = self.view.bounds; \`\`\` -
注意：请在开始采集之后再获取预览视图，否则获取到的预览视图对象将为
\`nil\`

\#\#\# 开始推流

下面可以开始进行推流了

\`\`\`objectivec \[controller startBroadcastingWithCompletion:^(NSError
\*error) {

``` 
  // 做出相应响应
```

}\]; \`\`\`

\#\#\# 设置 \`MLController\` 的 delegate

\`MLController\` 通过 delegate 的方式进行直播相关状态的通知，您可以通过实现
\`MLControllerDelegate\` 中定义的方法来获得包括直播状态等相关的通知。

\`\`\`objectivec {

``` 
  ...
  controller.delegate = self
  ...
```

}

// 获取直播相关状态改变的回调 - (void)controller:(MLController \*)controller
stateDidChange:(MLState)state error:(NSError \*)error {

``` 
  // 针对相应直播状态改变做出相应响应
```

} \`\`\`

\#\#\# 结束推流

当推流完成之后可以调用如下方法结束推流（结束之后依然可以使用该对象再次调用
\`-startBroadcastingWithCompletion:\` 方法再次开始推流）

\`\`\`objectivec \[controller stopBroadcasting\]; \`\`\`
