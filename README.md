# rn-umeng-analytics
##安装
```
npm install rn-umeng-analytics
react-native link rn-umeng-analytics
```

## 简介
其实这个库代码都是友盟官方提供的，此库只是方便安装而已。
官方SDK下载地址：http://dev.umeng.com/analytics/h5/sdk-download

官方集成文档地址：http://dev.umeng.com/analytics/h5/react-native%E9%9B%86%E6%88%90%E6%96%87%E6%A1%A3

## 使用
```
import UMNative from 'rn-umeng-analytics';

UMNative.onPageBegin("pageA");
UMNative.onPageEnd("pageA");
```

## 配置
	使用`react-native link`后你还需要手动做一些配置（真麻烦...我也这么觉得。）
### iOS
 * 添加依赖
	请在你的工程目录结构中，添加友盟统计框架，在选项
	`TARGETS--> Build Phases-->Link Binary With Libraries-->Add Other`，选择模块下的ios目录中UMMobClick.framework文件并选择确认；
	添加系统依赖框架(Framework)和编译器选项
	`TARGETS-->Build Phases-->Link Binary With Libraries--> + -->CoreTelephony.framework libz.tbd libsqlite3.0.tbd`
<!--	添加 Fram径-->
	`TARGETS--> Build Settings --> Framework search Paths --> + "$(SRCROOT)/../node_modules/rn-umeng-analytics/ios/umsdk_IOS_analyics_no-idfa_v4.1.5/" non-recursive`
	添加 Header 扫描路径
	`TARGETS--> Build Settings --> Header Search Paths --> + $(SRCROOT)/../node_modules/rn-umeng-analytics/ios recursive`

 * 配置 AppDelegate.m
	AppDelegate.m 的配置主要包括填写`Appkey`，设置发送策略和填写渠道id三部分，代码示例如下：

```
#import "UMMobClick/MobClick.h"
...
- (BOOL))application:(UIApplication) *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

  ...

  UMConfigInstance.appKey = @"[Appkey]";
//  UMConfigInstance.eSType=E_UM_GAME;//友盟游戏统计，如不设置默认为应用统计
  
  [MobClick startWithConfigure:UMConfigInstance];
  [MobClick setLogEnabled:YES]; 
  //设置是否打印sdk的log信息, 默认NO(不打印log).设置为YES,umeng SDK 会输出log信息可供调试参考. 除非特殊需要，否则发布产品时需改回NO.

}
```

### android
* 配置AndroidManifest.xml文件
```
<manifest>
    <application>
	    <!-- 添加appKey -->
		<meta-data
			android:name="UMENG_APPKEY"
			android:value="你的key" />
		<meta-data
			android:name="UMENG_CHANNEL"
			android:value="应用标识" />
	</application>

	<!-- 添加 umeng 统计的权限 -->
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses-permission>
	<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
	<uses-permission android:name="android.permission.INTERNET"></uses-permission>
	<uses-permission android:name="android.permission.READ_PHONE_STATE"></uses-permission>

</manifest>
```

* 配置MainActivity.java
	在Activity的几个生命周期中添加统计支持。
```
import com.umeng.analytics.MobclickAgent;

@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);

	// SDK在统计Fragment时，需要关闭Activity自带的页面统计，
	// 然后在每个页面中重新集成页面统计的代码(包括调用了 onResume 和 onPause 的Activity)。
	MobclickAgent.openActivityDurationTrack(false);
	MobclickAgent.setSessionContinueMillis(1000);
	MobclickAgent.setScenarioType(this, MobclickAgent.EScenarioType.E_UM_NORMAL);
}

@Override
protected void onResume() {
	super.onResume();
	MobclickAgent.onResume(this);
	MobclickAgent.onKillProcess(this.getApplicationContext());
}

@Override
protected void onPause() {
	super.onPause();
	MobclickAgent.onPause(this);
}
```

-----

## JS 统计方法
JavaScript方法介绍
```
UMNative.onEvent(eventId) 自定义事件
UMNative.onCCEvent(evenArray, evenValue, eventLabel) 结构化自定义事件
UMNative.onEventWithLabel(eventId, eventLabel) 自定义事件
UMNative.onEventWithParameters(eventId, eventData) 自定义事件
UMNative.onEventWithCounter(eventId, eventData, eventNum) 自定义事件
UMNative.onPageBegin(pageName) 页面开始的时候调用此方法
UMNative.onPageEnd(pageName) 页面结束的时候调用此方法

// 后边的方法没有，因为用的不是包含IDFA的SDK
UMNative.profileSignInWithPUID(puid) 统计帐号登录接口
UMNative.profileSignInWithPUIDWithProvider(puid, provider) 统计帐号登录接口
UMNative.profileSignOff() 帐号统计退出接口
UMNative.setUserLevelId(level) 当玩家建立角色或者升级时,需调用此接口
UMNative.startLevel(level) 在游戏开启新的关卡的时候调用
UMNative.finishLevel(level) 关卡结束时候调用
UMNative.failLevel(level) 关卡失败时候调用
UMNative.exchange(orderId, currencyAmount, currencyType, virtualAmount, channel) 真实消费统计
UMNative.pay(cash, source, coin) 真实消费统计
UMNative.payWithItem(cash, source, item, amount, price) 真实消费统计
UMNative.buy(item, amount, price) 虚拟消费统计
UMNative.use(item, amount, price) 物品消耗统计
UMNative.bonusWithItem(item, amount, price, source) 额外奖励
UMNative.(coin, source) 额外奖励
```

## 测试

友盟提供了实时的测试工具给我们进行测试，并且不污染实际的数据。使用时，我们需要[添加测试设备](http://mobile.umeng.com/test_devices)。


**注意：** 包中使用的是无IDFA版本的SDK