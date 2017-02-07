---
title: iOS项目总结
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

我在此项目负责了哪些工作，分别在哪些地方做得出色/和别人不一样/成长快，这个项目中，我最困难的问题是什么，我采取了什么措施，最后结果如何。这个项目中，我最自豪的技术细节是什么，为什么，实施前和实施后的数据对比如何，同事和领导对此的反应如何。

## 医疗类
- [糖福薈](https://itunes.apple.com/cn/app/tang-fu-hui/id1194434437?mt=8)
	
	糖福薈是一个对血糖血压等健康数据统计和监控的App，分用户和亲友两端。亲友端可远程监控患者健康状态。	我在此项目中负责血糖血压等记录添加和展示UI的绘制、通用选项卡和提醒模块，梳理了Notification的相关知识，更深入理解UIKit。

- DirCare（未上线）

	DirCare是一个医生预约App，我在此项目中负责HomePage（Filter选择）、SearchResult（医生列表和地图展示）、MedicalTeam、Settings的UI绘制，因此对于UITableView、UICollectionView、基本animation、MapKit有了更深入的了解。
## 电商类
- [港藥網](https://itunes.apple.com/cn/app/gang-yao-wang-xiang-gang-yao/id1042925913?mt=8)
- [EATBAR 吃吧](https://itunes.apple.com/cn/app/eatber-chi-baapp-xiang-gang/id1021841744?mt=8)
- Eateraction（在马来西亚上线）



## 工具类
- [易咪錶](https://itunes.apple.com/cn/app/yi-mi-biao-quan-ao-ren-ren/id1145701266?mt=8)

<!--- [Macau EasyCheck](https://itunes.apple.com/cn/app/macau-easycheck/id911653250?mt=8)-->

## 房產類
- [易上樓](https://itunes.apple.com/cn/app/easyhome-macau-yi-shang-lou/id1107872819?mt=8)

## 物流运输
- eCall（个人外包项目 已交付上线）
	- Swift3

### 难点与重点

### 知识点总结
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

#### App架构
1. MVC
2. MVVM

#### ViewController的切换
1. Tabbar
	* 系统Tabbar 与 自定义Tabbar
2. Navigation
	* 导航栏 
3. 其他转场效果

#### 图形与表格显示(重点难点)
#### [本地与远程推送](https://bluerunrun.github.io/2016/12/01/iOS-Notifications/)
#### [网络服务（AFNetWorking）](https://github.com/AFNetworking/AFNetworking)
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
3. 毛玻璃效果

		<!--创建显示图片-->
		UIImageView * imageView = [[UIImageView alloc] init];
		/**  毛玻璃特效类型
		 *   UIBlurEffectStyleExtraLight,
		 *   UIBlurEffectStyleLight,
		 *   UIBlurEffectStyleDark
		 */  
		UIBlurEffect * blurEffect = [UIBlurEffect effectWithStyle:UIBlurEffectStyleLight];
		<!--毛玻璃视图-->
		UIVisualEffectView * effectView = [[UIVisualEffectView alloc] initWithEffect:blurEffect];
		<!--添加到要有毛玻璃特效的控件中-->
		effectView.frame = imageView.bounds;
		[imageView addSubview:effectView];
		<!--设置模糊透明度-->
		effectView.alpha = .5f;

#### LoadingView与HUD 
1. MBProgressHUD
2. LoadingView

#### [数据持久化](http://www.cocoachina.com/ios/20161115/18084.html)
1. 文件 
2. 数据库 (FMDB)
3. UserDefault
4. 字典与数组模型化 (MJExtension)

#### UIScrollVIew
1. ‘xib’与‘代码生成’可滚动的内容的区别
2. 配合Tabbar的View滚动
3. 图片查看器
4. 键盘遮挡 (TPKeyboardAvoiding)

#### UITableView与UICollectionView
1. 下拉刷新与加载更多(MJRefresh)
2. cell的动态高度
3. 多层嵌套

#### 应用内数据传递
1. Block
2. Notification
3. Delegate
4. 暴露的对象变量

#### 图片的加载与缓存(SDWebImage)
1. 理解iOS存储机制

#### 常见的日期处理(NSDate与NSCalendar)
1.  日期格式化
2.  日期比较

#### 相机与相册
1. 二维码的生产与扫描
2. 相机授权与调用
3. 图片的选择(QBImagePickerController)

#### IOS开发之手势
1. 常见手势
2. [手势共存](http://www.cnblogs.com/iphone520/archive/2011/10/27/2226548.html)

#### UIPickerView与UIDatePicker
1. ListPickView

#### UIWebView与UIActivityIndicatorView
#### 微信分享