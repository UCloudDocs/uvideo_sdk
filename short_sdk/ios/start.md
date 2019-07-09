{{indexmenu_n>2}}
亲爱的开发者，欢迎使用UCloud短视频SDK。如下内容将引导您在自己的IOS工程中集成短视频录制，编辑及导出功能。首先，我们假定您已了解
Objective-C 的基础语法。

\#\# 添加依赖 需要进入UCloud控制台创建所需且匹配业务的SDK，后将MovieousShortVideo引入工程中。

\#\# 引入相关头文件 在您需要集成短视频SDK的页面源文件中添加如下语句 \`\`\`objectivec \#import
\<MovieousShortVideo/MovieousShortVideo.h\> \`\`\`

下面，可以根据自己的需求，选择性地集成视频录制、视频编辑或视频导出模块 \#\# 视频录制 普通列表项目添加麦克风和摄像头使用权限
打开项目配置页面，Info 选项添加 \`Privacy - Camera Usage Description\`
和 \`Privacy - Microphone Usage Description\` 描述
![](/images/short_sdk/ios/authorization.png)

生成音频和视频的配置对象并根据需求进行&#8;相关参数的更改 \`\`\`objectivec

``` 
  MSVRecorderAudioConfiguration *audioConfiguration = [MSVRecorderAudioConfiguration defaultConfiguration];
  MSVRecorderVideoConfiguration *videoConfiguration = [MSVRecorderVideoConfiguration defaultConfiguration];
  // 根据需求更改需要更改的参数，不更改则使用默认参数
  videoConfiguration.cameraResolution = AVCaptureSessionPreset1280x720;
  videoConfiguration.cameraPosition = AVCaptureDevicePositionFront;
```

\`\`\`

生成录制核心控制对象&#8; \`\`\`objectivec

``` 
  _recorder = [[MSVRecorder alloc] initWithAudioConfiguration:audioConfiguration videoConfiguration:videoConfiguration error:&error];
  if (error) {
      SHOW_ERROR_ALERT;
      return;
  }
  // 设置最大录制长度
  _recorder.maxDuration = 10;
  // 设置录制事件回调
  _recorder.delegate = self;
  // 将预览视图插入视图栈中，以便预览摄像头采集结果
  _recorder.preview.frame = self.view.frame;
  [self.view insertSubview:_recorder.preview atIndex:0];
```

\`\`\`

开始音视频采集 \`\`\`objectivec

``` 
  [_recorder startCapturingWithCompletionHandler:^(BOOL audioGranted, NSError *audioError, BOOL videoGranted, NSError *videoError) {
      if (videoError) {
          SHOW_ALERT(@"error", videoError.localizedDescription, @"ok");
          return;
      }
      if (!videoGranted) {
          SHOW_ALERT(@"warning", @"video not authorized", @"ok");
          return;
      }
      dispatch_async(dispatch_get_main_queue(), ^{
          [self syncBeautyParams];
      });
      if (audioError) {
          SHOW_ALERT(@"error", audioError.localizedDescription, @"ok");
          return;
      }
      if (!audioGranted) {
          SHOW_ALERT(@"warning", @"audio not authorized", @"ok");
          return;
      }
  }];
```

\`\`\`

开始录制片段 \`\`\`objectivec

``` 
  if (![_recorder startRecordingWithClipConfiguration:config error:&error]) {
      SHOW_ERROR_ALERT;
      return;
  }
```

\`\`\`

完成当前片段录制 \`\`\`objectivec

``` 
  [_recorder finishRecordingWithCompletionHandler:^(MSVMainTrackClip *clip, NSError *error) {
      // 从生成的 clip 对象中可以获取到当前录制片段生成的 mp4 文件及相关描述信息
      if (error) {
          SHOW_ERROR_ALERT;
      }
  }];
```

\`\`\`
完成当前片段录制之后还可以回到\`开始录制片段\`的步骤继续进行下一个片段的录制，所有录制好的片段将会顺序拼接为一个视频草稿&#8;保存在
\`\_recorder.draft\` 对象中，&#8;可传入编辑器进行编辑或传入导出器直接导出为&#8;一个&#8;合成后的文件

\#\# 视频编辑 创建编辑器&#8;核心控制类 \`\`\`objectivec

``` 
  // 此处的 draft 可以使用在录制阶段生成的也可以使用已有音视频资源（例如系统相册或从远端下载等）创建
  _editor = [MSVEditor createSharedInstanceWithDraft:_draft error:&error];
  if (error) {
      SHOW_ERROR_ALERT;
      return;
  }
  _editor.delegate = self;
  // 将预览视图插入视图栈中，以便预览编辑结果
  _previewContainer.preview = _editor.preview;
  _editor.loop = YES;
```

\`\`\`

&#8;预览草稿 \`\`\`objectivec

``` 
  [_editor play];
```

\`\`\`

\- 进行视频编辑 \`MovieousShortVideo\` 使用 \`MSVDraft\`
对象&#8;存储&#8;视频的编辑信息，你可以根据需求对
\`\_editor.draft\`
&#8;的参数进行任意调整，调整之后预览&#8;视图会实时刷新为调整参数之后的内容，下面演示一下对视频进行剪辑操作
\`\`\`objectivec

``` 
  _editor.draft.timeRange = (MovieousTimeRange){
      // 需要剪辑的部分的开始时间
      startTime,
      // 需要剪辑的部分的时长
      duration,
  };
```

\`\`\`

\#\# 视频导出 创建导出核心控制对象 \`\`\`objectivec

``` 
  NSError *error;
  // 创建导出控制对象
  _exporter = [[MSVVideoExporter alloc] initWithDraft:_draft error:&error];
  if (error) {
      SHOW_ERROR_ALERT;
      return;
  }
  // 将导出结果保存到相册中
  _exporter.saveToPhotosAlbum = YES;
  __weak typeof(self) wSelf = self;
  // 进度回调
  _exporter.progressHandler = ^(float progress) {
      NSLog(@"progress: %f", progress);
  };
  // 完成回调
  _exporter.completionHandler = ^(NSURL *URL) {
      SHOW_ALERT_FOR(@"完成", @"成功导出", @"知道啦", wSelf);
  };
  // 失败回调
  _exporter.failureHandler = ^(NSError *error) {
      SHOW_ERROR_ALERT_FOR(wSelf);
  };
```

\`\`\`

开始导出 \`\`\`objectivec

``` 
  [_exporter startExport];
```

\`\`\`
