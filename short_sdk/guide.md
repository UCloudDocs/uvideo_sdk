# 使用指南

{{indexmenu_n>2}}
本章目的在于指导初次使用的用户如何快速理解控制台操作，实现SDK的接入的第一步。

## Step1. 新建APP

进入短视频SDK产品界面，点击【新建】。

![](/images/short_sdk/1543388329458.jpg)

APPName:填写待创建的APP名称

APPID:该应用的唯一标识

客户联系人:留下可联系电话，保证信息及时同步。

## Step2. 详情页

新建页面输入内容，点击确定后，进入详情页

![](/images/short_sdk/详情页.png)

基础模块为必选功能，高级订阅中的功能项可根据自己的业务需求勾选合适功能。

## Step3. 资源列表页

在选择试用或者商用后，会展示对应信息 ![](/video/uvideo_sdk/short_sdk/list.jpg)

**PS： 最后鉴权时间为转为商用时间。 **

**试用期间可任意时间进入详情页转为商用。**

## Step4. License下载和使用说明

![](/images/short_sdk/list.jpg) 点击下载License，控制台会生成 lincese.txt
鉴权文件。

IOS License使用说明：

1、访问 <https://github.com/umediakits/UShortVideo-Cocoa-Release> 下载最新版本的
SDK

2、在应用启动时调用 \[MovieousShortVideo registerWithLicense:${license}\]; 方法注册
SDK，其中 ${license} 填写 lincese.txt 中的字符串

3、参照 <https://docs.ucloud.cn/video/uvideo_sdk/short_sdk/ios/start> 进行
SDK 其他相关集成工作

Android License使用说明：

1、访问 <https://github.com/umediakits/UShortVideo-Android-Release> 下载最新版本的
SDK

2、在应用启动时通过 UShortVideoEnv 中的 init 接口，把 lincese.txt 文件中的字符串传递给 SDK 进行鉴权。

3、参照 <https://docs.ucloud.cn/video/uvideo_sdk/short_sdk/android/start>
进行 SDK 其他相关集成工作
