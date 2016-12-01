---
title: 项目知识点总结
date: 2016-11-30 12:09:56
update: 2016-11-30 17:57:00
categories: 
- iOS
tags:
- iOS
- 总结
- 项目

---
对项目所涉及的知识点进行总结。
### TXH
***
#### App设置
1. 启动图片的设置
2. PrefixHeader.pch
	* 此文件可以设置共用的Macro
	* [设置方法](http://www.jianshu.com/p/a19bb67d705e)
	* 当工程有混编时，需添加如下代码 
	
			<!--若没有此限制 则c文件也会导入了oc头-->
			#ifdef __OBJC__
			#import "UIImage+color.h"
			#import "UIButton+FillColor.h"
			#import "NSString+lr_stringDate.h"
			#endif
3. [关于Xcode的Other Linker Flags](http://blog.csdn.net/meegomeego/article/details/19343423)
> 那么，Other Linker Flags到底是用来干什么的呢？还有-ObjC和-all_load到底发挥了什么作用呢？

4. 关于证书
	* [iOS开发证书、秘钥](http://www.cnblogs.com/kenshincui/p/4168532.html#certificate)
	* [Apple证书](http://blog.csdn.net/holydancer/article/details/9219333)

#### 版本检测
#### App架构
1. Tabbar
	* 系统Tabbar 与 自定义Tabbar
2. Navigation
	* 导航栏 
3. 其他转场效果

#### 本地与远程推送
#### 网络服务
1. 离线上传
2. 在线上传
3. 网络检测

#### 定时器
#### UITextField与UITextView
1. 常见文本的处理
2. 富文本
3. textview自定义Placeholder

#### 动画
1. 放大缩小
2. 呼吸效果

#### LoadingView与HUD 
#### 数据持久化
1. 数据库
2. UserDefault
3. 实体类
4. 字典与数组模型化

#### UITableView与UICollectionView
1. 下拉刷新与加载更多
2. cell的动态高度

#### 应用内数据传递
1. Block
2. Notification
3. Delegate
4. 暴露的对象变量

#### 图片的加载与缓存
1. 理解iOS存储机制

#### 常见的日期处理(NSDate与NSCalendar)
1.  日期格式化
2.  日期比较

#### 相机与相册
1. 二维码的生产与扫描
2. 相机授权与调用
3. 图片的选择

#### UIPickerView与UIDatePicker
#### 图形与表格显示
#### UIWebView与UIActivityIndicatorView
#### 微信分享