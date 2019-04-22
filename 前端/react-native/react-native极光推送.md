## react-native 集成极光推送，iOS/andriod
[项目](https://github.com/miqiaoli/dbysDrivers)
1. 极光 appkey  ebd4f2236b5698a95bd55257
```
    "jcore-react-native": "^1.3.2",
    "jpush-react-native": "^2.5.3",
    "react": "16.6.0-alpha.8af6728",
    "react-native": "0.57.4",
    Xcode: version 10.2(10E125)
    Android Studio 3.3.2
```

> 时间 2019-04-10

* ```npm install jpush-react-native jcore-react-native --save```
* ``` react-native link jpush-react-native react-native link jcore-react-native ``` <br>然后会要求输入AppKey 极光应用获取一个AppKey
ebd4f2236b5698a95bd55257
* 解决IOS XcodeLibraries文件夹下有没有
   RCTJpushModule.xcodproj
   和
   RCTJcoreModule.xcodproj文件
   rnpm link jpush-react-native
   rnpm link jcore-react-native
   没有安装 rnpm 先 npm install rnpm -g 

### Android

*  android/settings.gradle
``` 
include ':jpush-react-native'
project(':jpush-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/jpush-react-native/android')
include ':jcore-react-native'
project(':jcore-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/jcore-react-native/android')
```
* android/app/build.gradle
```
android {
    ...
    defaultConfig {
        ...
        manifestPlaceholders = [
                JPUSH_APPKEY: "ebd4f2236b5698a95bd55257",
                APP_CHANNEL : "default"
        ]
        ...
    }
}
dependencies {
    implementation project(':jcore-react-native')
    implementation project(':jpush-react-native')
    ...
    implementation fileTree(dir: "libs", include: ["*.jar"])
    implementation "com.android.support:appcompat-v7:${rootProject.ext.supportLibVersion}"
    implementation "com.facebook.react:react-native:+"  // From node_modules
}
```
* android/app/src/main/java/com/.../MainActivity.java
```
import android.os.Bundle;
import cn.jpush.android.api.JPushInterface;
public class MainActivity extends ReactActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // SplashScreen.show(this);  // here
        super.onCreate(savedInstanceState);
        JPushInterface.init(this); //新加
    }
    // 新加
    @Override
    protected void onPause() {
        super.onPause();
        JPushInterface.onPause(this);
    }

    @Override
    protected void onResume() {
        super.onResume();
        JPushInterface.onResume(this);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
    }
    // 新加
}

```
* android/app/src/main/java/com/.../MainApplication.java
```
import cn.jpush.reactnativejpush.JPushPackage;

public class MainApplication extends Application implements ReactApplication {
    ...
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
             
        new MainReactPackage(),
        new JPushPackage(!BuildConfig.DEBUG, !BuildConfig.DEBUG) // 新加
            
      );
    }
}
```
* android/app/src/main/AndroidManifest.xml
```
<manifest ...>
    ...
    <meta-data
        android:name="JPUSH_APPKEY"
        android:value="${JPUSH_APPKEY}" />
    <meta-data
        android:name="JPUSH_CHANNEL"
        android:value="${APP_CHANNEL}" />
</manifest>
```

### iOS
* Xcode 打开，检查 /Libraies是否有RCTJpushModule.xcodproj
   和
   RCTJcoreModule.xcodproj，没有上面有方法
* target/项目名/Build Phases/Link Binary with Libraries中加入如下库
```js
libz.tbd
CoreTelephony.framework
Security.framework
CFNetwork.framework
CoreFoundation.framework
SystemConfiguration.framework
Foundation.framework
UIKit.framework
UserNotifications.framework
libresolv.tbd
```
* ``` UserNotifications.framework 设置status为Optiona l```
* 项目/capacities/push notifications on 打开
* target/项目名/Build Settings/Header Search Paths是否加入,没有就加入
```js
$(SRCROOT)/../node_modules/jcore-react-native/ios/RCTJCoreModule
$(SRCROOT)/../node_modules/jpush-react-native/ios/RCTJPushModule
* ios/Podfile
```js
// 查看是否写入，没有就添加
target 'DbysDriver' do
  
 ...
  pod 'JPushRN', :path => '../node_modules/jpush-react-native'

  pod 'JCoreRN', :path => '../node_modules/jcore-react-native'
 ...
end

```
* ios/项目名/AppDelegate.m
```js
// 查看是否写入，没有就写入，导入如下头文件
#import <RCTJPushModule.h>
#ifdef NSFoundationVersionNumber_iOS_9_x_Max
#import <UserNotifications/UserNotifications.h>
#endif

// didFinishLaunchingWithOptions 方法里面添加如下代码
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{ 
  // 新加的，没有就写入
  JPUSHRegisterEntity * entity = [[JPUSHRegisterEntity alloc] init];
     entity.types = UNAuthorizationOptionAlert|UNAuthorizationOptionBadge|UNAuthorizationOptionSound;
     [JPUSHService registerForRemoteNotificationConfig:entity delegate:self];

  [JPUSHService setupWithOption:launchOptions appKey:@"你的极光appkey"
                        channel:nil apsForProduction:nil];

  NSURL *jsCodeLocation;
  ...    
}

// 这些代码，一般不需要手动添加，会自动写入
...
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
{
  [JPUSHService registerDeviceToken:deviceToken];
}

- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
  // 取得 APNs 标准信息内容
{
  [[NSNotificationCenter defaultCenter] postNotificationName:kJPFDidReceiveRemoteNotification object:userInfo];
}

- (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification
{
  [[NSNotificationCenter defaultCenter] postNotificationName:kJPFDidReceiveRemoteNotification object: notification.userInfo];
}

- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)   (UIBackgroundFetchResult))completionHandler
{
  [[NSNotificationCenter defaultCenter] postNotificationName:kJPFDidReceiveRemoteNotification object:userInfo];
}

// iOS 10 Support
- (void)jpushNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(NSInteger))completionHandler
{
  NSDictionary * userInfo = notification.request.content.userInfo;
  if ([notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
    [JPUSHService handleRemoteNotification:userInfo];
    [[NSNotificationCenter defaultCenter] postNotificationName:kJPFDidReceiveRemoteNotification object:userInfo];
  }

  completionHandler(UNNotificationPresentationOptionAlert); // 需要执行这个方法，选择是否提醒用户，有Badge、Sound、Alert三种类型可以选择设置
}

// iOS 10 Support
- (void)jpushNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)())completionHandler
{
  NSDictionary * userInfo = response.notification.request.content.userInfo;
  if ([response.notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
    [JPUSHService handleRemoteNotification:userInfo];
    [[NSNotificationCenter defaultCenter] postNotificationName:kJPFOpenNotification object:userInfo];
  }

  completionHandler();
}
...
```

```js
```
[demo](https://github.com/jpush/jpush-react-native/tree/master/example)