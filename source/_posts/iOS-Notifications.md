---
title: iOS通知机制
date: 2016-12-01 15:21:16
categories: 
- iOS
tags:
- iOS
- 总结
- 推送
- 通知
- Notification

---
  通知和推送是两种不同的概念。推送只是远程通知的触发方式，而通知是操作层面的一种UI展示方式，所以推送不等同于通知。iOS中通知机制包括两类：一类是本地通知；另一类是推送通知，也叫远程通知。两种通知在iOS中的表现一致，可以通过横幅或者弹出提醒两种形式告诉用户，并且点击通知可以会打开应用程序，但是实现原理却完全不同。
  iOS10中将通知功能整合成了一个框架UserNotification。
  
## iOS10前
相当于闹钟，无需联网
### [本地通知](http://www.cnblogs.com/kenshincui/p/4168532.html)
这篇文章说得已经很详尽了。

1. 请求授权(注意：如果不请求授权在设置中是没有对应的通知设置项的)
    
	    UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes: (UIUserNotificationTypeBadge | UIUserNotificationTypeAlert | UIUserNotificationTypeSound) categories:nil];
	    [[UIApplication sharedApplication] registerUserNotificationSettings:settings];

2. 添加通知
	
			// 添加本地通知
		+ (void)addLocalNotificationWithKey:(NSString *)key Type:(NSString *)type Content:(NSString*)content Time:(NSDate *)time{
		    // 获取所有本地通知数组
		    [clsOtherFun cancelLocalNotificationWithKey:key];
		    
		    // 初始化本地通知对象
		    UILocalNotification *notification = [[UILocalNotification alloc] init];
		    if (notification) {
		        // 设置通知的提醒时间
		        notification.timeZone = [NSTimeZone defaultTimeZone]; // 使用本地时区
		        notification.fireDate = time;
		        
		        // 设置重复间隔
		        notification.repeatInterval = kCFCalendarUnitDay;
		        
		        // 设置提醒的文字内容
		        notification.alertBody   = content;
		        notification.alertAction = NSLocalizedString(@"起床了", nil);
		        notification.soundName = UILocalNotificationDefaultSoundName;
		        
		        // 设置应用程序右上角的提醒个数
		        // notification.applicationIconBadgeNumber++;
		        
		        // 设定通知的userInfo，用来标识该通知
		        NSMutableDictionary *aUserInfo = [[NSMutableDictionary alloc] init];
		        aUserInfo[LocalNotificationIDKey] = key;
		        aUserInfo[LocalNotificationTypeKey] = type;
		        notification.userInfo = aUserInfo;
		        
		        // 将通知添加到系统中
		        [[UIApplication sharedApplication] scheduleLocalNotification:notification];
		    }
		}

3. 取消通知
	
			// 取消某个本地推送通知
		+ (void)cancelLocalNotificationWithKey:(NSString *)key {
		    // 获取所有本地通知数组
		    NSArray *localNotifications = [UIApplication sharedApplication].scheduledLocalNotifications;
		    
		    for (UILocalNotification *notification in localNotifications) {
		        NSDictionary *userInfo = notification.userInfo;
		        if (userInfo) {
		            // 根据设置通知参数时指定的key来获取通知参数
		            NSString *info = userInfo[LocalNotificationIDKey];
		            
		            // 如果找到需要取消的通知，则取消
		            if ([info isEqualToString:key]) {
		                [[UIApplication sharedApplication] cancelLocalNotification:notification];
		                break;
		            }
		        }
		    }
		}

	
4. 接受通知
	* App在活跃状态
		
			-(void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification{
				LRLog(@"\n应用内Local notification:%@ 提醒內容:%@ 提醒时间:%@",notification.userInfo[LocalNotificationIDKey],notification.alertBody,notification.fireDate);
		   	}

	* App在后台 (当用户点击系统通知时)
		
			- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
				……
				//接收通知参数®
				UILocalNotification *notification=[launchOptions valueForKey:UIApplicationLaunchOptionsLocalNotificationKey];
				NSDictionary *userInfo= notification.userInfo;
				……
			}

### [远程通知](http://www.cnblogs.com/kenshincui/p/4168532.html)

## iOS10

UserNotification相比之前的通知功能更加强大.

![UserNotification框架](~/source/images/UN01.png)

**基本流程：申请权限和注册 -> 创建和发起 -> 展示和处理**

### 权限申请
#### iOS系统注保护用户隐私，许多操作都需获取用户权限，通知就是其一。示例如下：

		//进行用户权限的申请
		[[UNUserNotificationCenter currentNotificationCenter] requestAuthorizationWithOptions:UNAuthorizationOptionBadge|UNAuthorizationOptionSound|UNAuthorizationOptionAlert|UNAuthorizationOptionCarPlay completionHandler:^(BOOL granted, NSError * _Nullable error) {
		      //在block中会传入布尔值granted，表示用户是否同意
		      if (granted) {
		            //如果用户权限申请成功，设置通知中心的代理
		            [UNUserNotificationCenter currentNotificationCenter].delegate = self;
		      }
		}];
	
#### UNAuthorizationOptions权限参数，其枚举定义如下：

		typedef NS_OPTIONS(NSUInteger, UNAuthorizationOptions) {
		    //允许更新app上的通知数字
		    UNAuthorizationOptionBadge   = (1 << 0),
		    //允许通知声音
		    UNAuthorizationOptionSound   = (1 << 1),
		    //允许通知弹出警告
		    UNAuthorizationOptionAlert   = (1 << 2),
		    //允许车载设备接收通知
		    UNAuthorizationOptionCarPlay = (1 << 3),
		};

#### 远程通知需要注册和获取token
		
* 注册远程通知
		
		// 在 AppDelegate 的 - (BOOL)application: didFinishLaunchingWithOptions: 中获取用户 token：
	    [[UIApplication sharedApplication] registerForRemoteNotifications];
* 获取token

			- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken{
	    
	    NSString *strToken=[NSString stringWithFormat:@"%@",deviceToken];
	    strToken=[strToken stringByReplacingOccurrencesOfString:@"<" withString:@""];
	    strToken=[strToken stringByReplacingOccurrencesOfString:@" " withString:@""];
	    strToken=[strToken stringByReplacingOccurrencesOfString:@">" withString:@""];
	    //    LRLog(@"\nMy Token is:%@",strToken);
	    [clsOtherFun setDeviceToken:strToken];
	}
	
### 创建与发起通知
	
	// 1. 创建通知内容
    UNMutableNotificationContent *content = [[UNMutableNotificationContent alloc] init];
    //    content.badge = @2;
    content.title = identifier;
    content.body = msg;
    content.sound = [UNNotificationSound defaultSound];
    // 设定通知的userInfo，用来标识该通知
    NSMutableDictionary *aUserInfo = [[NSMutableDictionary alloc] init];
    aUserInfo[LocalNotificationIDKey] = identifier;
    content.userInfo = aUserInfo;
    
    // 2. 创建发送触发
    UNTimeIntervalNotificationTrigger *trigger = [UNTimeIntervalNotificationTrigger triggerWithTimeInterval:second repeats:NO];
    
    // 3. 创建一个发送请求
    UNNotificationRequest *request = [UNNotificationRequest requestWithIdentifier:identifier content:content trigger:trigger];
    
    // 4. 将请求添加到发送中心
    [[UNUserNotificationCenter currentNotificationCenter] addNotificationRequest:request withCompletionHandler:^(NSError * _Nullable error) {
        if (error != nil) {
            LRLog(@"Time Interval Notification scheduled Error : %@",error.description);
        }
    }];

#### 触发器
 **触发方式有四种，Time Interval，Calender和Location，Push。其中Push是远程通知专用触发器。**
##### UNTimeIntervalNotificationTrigger
UNTimeIntervalNotificationTrigger是计时触发器，开发者可以设置其在添加通知请求后一定时间发送。在日常生活中当某项功能操作完成后，提醒用户下一步应做事项。

	UNTimeIntervalNotificationTrigger *trigger = [UNTimeIntervalNotificationTrigger triggerWithTimeInterval:second repeats:NO];
##### UNCalendarNotificationTrigger
UNCalendarNotificationTrigger是日历触发器，开发者可以设置其在某个时间点触发。

	NSCalendar *greCalendar = [[NSCalendar alloc] initWithCalendarIdentifier:NSCalendarIdentifierGregorian];
	NSDateComponents *dateComponents = [greCalendar components:NSCalendarUnitHour|NSCalendarUnitMinute  fromDate:time];
	dateComponents.timeZone = [NSTimeZone defaultTimeZone];//指定几时几秒
	    
	UNCalendarNotificationTrigger *trigger = [UNCalendarNotificationTrigger triggerWithDateMatchingComponents:dateComponents repeats:YES];
##### UNLocationNotificationTrigger
UNLocationNotificationTrigger是地域触发器，开发者可以设置当用户进出某一区域时触发。
	
	CLRegion *region = [[CLCircularRegion alloc] initWithCenter:coordinate radius:radius identifier:identifier];
    region.notifyOnExit = YES;
    region.notifyOnEntry = YES;
    UNLocationNotificationTrigger *trigger = [UNLocationNotificationTrigger triggerWithRegion:region repeats:YES];
    
#### 为通知内容添加附件
 附件主要指的是媒体附件，例如图片，音频和视频，为通知内容添加附件需要使用UNNotificationAttachment类。示例代码如下：
 	
 	//创建媒体附件
    UNNotificationAttachment * attach = [UNNotificationAttachment attachmentWithIdentifier:@"imageAttach" URL:[NSURL fileURLWithPath:[[NSBundle mainBundle] pathForResource:@"1" ofType:@"mp3"]] options:nil error:nil];
    //设置附件数组
    content.attachments = @[attach];
    
#### 通知模板的设置

	//创建action
	UNTextInputNotificationAction * action = [UNTextInputNotificationAction actionWithIdentifier:@"action" title:@"回复" options:UNNotificationActionOptionAuthenticationRequired textInputButtonTitle:@"发送" textInputPlaceholder:@"请输入回复内容"];
	//创建通知模板
    UNNotificationCategory * category = [UNNotificationCategory categoryWithIdentifier:@"myNotificationCategoryText" actions:@[action,action2,action3,action4] intentIdentifiers:@[] options:UNNotificationCategoryOptionCustomDismissAction];
    //设置通知内容对应的模板 需要注意 这里的值要与对应模板id一致
    content.categoryIdentifier = @"myNotificationCategoryText";
    [[UNUserNotificationCenter currentNotificationCenter] setNotificationCategories:[NSSet setWithObjects:category, nil]];


## 参考链接
[iOS开发系列--通知与消息机制](http://www.cnblogs.com/kenshincui/p/4168532.html)

[iOS推送之本地推送](http://www.jianshu.com/p/77ee3b98c132)

[iOS推送之远程推送](http://www.jianshu.com/p/4b947569a548)