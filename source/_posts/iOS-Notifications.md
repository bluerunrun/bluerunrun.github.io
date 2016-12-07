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
  即使App不在活跃状态也可以向用户推送信息.


  iOS中通知机制又叫消息机制，其包括两类：一类是本地通知；另一类是推送通知，也叫远程通知。两种通知在iOS中的表现一致，可以通过横幅或者弹出提醒两种形式告诉用户，并且点击通知可以会打开应用程序，但是实现原理却完全不同。
  
## 本地推送
相当于闹钟，无需联网
### [iOS10前的](http://www.cnblogs.com/kenshincui/p/4168532.html)
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

### [iOS10](https://onevcat.com/2016/08/notification/)

## 远程推送

## 参考链接
[iOS开发系列--通知与消息机制](http://www.cnblogs.com/kenshincui/p/4168532.html)

[iOS推送之本地推送](http://www.jianshu.com/p/77ee3b98c132)

[iOS推送之远程推送](http://www.jianshu.com/p/4b947569a548)