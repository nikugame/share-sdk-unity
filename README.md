# NKShareSDK（Unity版）接入文档(ver1.0.5_170307)
NKShareForUnity集成了Android和iOS端的资源，可直接接入Unity

------
##1、SDK说明
当前SDK是Unity版本，目前包含Android和iOS资源，导入Assets下接入即可。
##2、接入准备
NKShareSDK集成了QQ、微信、微博的分享，接入需申请这几个平台的参数，分别配置到Android和iOS端。
###（1）Android端配置
#### <1>平台参数
修改Android的assets目录下的share.properties文件里的各项参数对应的值即可（勿修改参数名）。
#### <2>AndroidManifest
AndroidManifest中请注意修改或增加以下几项配置：
#####①包名 package和version：
```
package="com.nkgame.nksdkdemoforunity"
android:versionCode="1"
android:versionName="1.0"
```
##### ②权限
```
<uses-permission android:name=""/>
```
##### ③Application属性
修改应用图标，对应位置res下的drawable文件夹下的ic_launcher.png
修改应用名称，对应res/values/strings内的app_name
```
android:icon="@drawable/ic_launcher"
android:label="@string/app_name"
```
##### ③Activity
**启动Activity**
修改启动的Activity为游戏的Activity,其他属性按游戏需求修改：
```
<activity android:configChanges="locale|fontScale|keyboard|keyboardHidden|mcc|mnc|navigation|orientation|screenLayout|screenSize|smallestScreenSize|touchscreen|uiMode" android:label="@string/app_name" android:launchMode="singleTask" android:name="com.unity3d.player.UnityPlayerActivity" android:screenOrientation="fullSensor">
<intent-filter>
<action android:name="android.intent.action.MAIN"/>
<category android:name="android.intent.category.LAUNCHER"/>
<category android:name="android.intent.category.LEANBACK_LAUNCHER"/>
</intent-filter>
<meta-data android:name="unityplayer.UnityActivity" android:value="true"/>
</activity>
```
**分享Activity**
```
<!--请务必在调起微博分享的Activity添加如下的intent-filter，此Activity更换为自己的Activity-->
<activity android:name=".ShareActivity">
<intent-filter>
<action android:name="com.sina.weibo.sdk.action.ACTION_SDK_REQ_ACTIVITY" />

<category android:name="android.intent.category.DEFAULT" />
</intent-filter>
</activity>
<!--QQ渠道，注意此处的scheme属性，需要替换为自己的申请的appid-->
```
**QQ渠道**
<!--QQ渠道，注意此处的scheme属性，需要替换为自己的申请的appid-->
<activity
android:name="com.tencent.tauth.AuthActivity"
android:launchMode="singleTask"
android:noHistory="true">
<intent-filter>
<action android:name="android.intent.action.VIEW" />

<category android:name="android.intent.category.DEFAULT" />
<category android:name="android.intent.category.BROWSABLE" />
<data android:scheme="tencent1105814454" />
</intent-filter>
</activity>
<activity
            android:name="com.tencent.connect.common.AssistActivity"
            android:configChanges="orientation|keyboardHidden"
            android:screenOrientation="behind"
            android:theme="@android:style/Theme.Translucent.NoTitleBar" />

**微信相关**
修改src下的WXEntryActivity的包名为游戏的包名：
因微信的特殊性，同时接入NKBaseSDK和NKShareSDK时需要在同一个WXEntryActivity实现两者的回调，即同时接入时需要合并功能到同一文件。
```
<activity
android:name="com.wanmei.zt3D.wxapi.WXEntryActivity"
android:exported="true"
android:label="@string/app_name"
android:launchMode="singleTop"
android:theme="@android:style/Theme.Translucent" />
```
### （2）iOS端配置

#### <1>.导入
SystemConfiguration.framework,
libz.tbd,Foundation.framework,
libstdc++.6.0.9.tbd,
Security.framework,
libiconv.tbd,
CoreGraphics.framework,
QuartzCore.framework,
ImageIO.framework,
CoreTelephony.framework,
CoreText.framework,
UIKit.framework,
libsqlite3.0.tbd,

#### <2>.加入配置build settings —> other linker flag 增加 -fobjc-arc -ObjC

#### <3>.info.plist中加入以下配置信息：(不要和已有配置重复)，然后在对应的URL Schemes中修改对应的appid.(Info -> URL Types )
````
<key>LSApplicationQueriesSchemes</key>
<array>
<string>weibosdk2.5</string>
<string>weibosdk</string>
<string>sinaweibohd</string>
<string>sinaweibo</string>
<string>weixin</string>
<string>mqq</string>
<string>mqqapi</string>
<string>mqqwpa</string>
<string>mqqbrowser</string>
<string>mttbrowser</string>
<string>mqqOpensdkSSoLogin</string>
<string>mqqopensdkapiV2</string>
<string>mqqopensdkapiV3</string>
<string>mqqopensdkapiV4</string>
<string>wtloginmqq2</string>
<string>mqzone</string>
<string>mqzoneopensdk</string>
<string>mqzoneopensdkapi</string>
<string>mqzoneopensdkapi19</string>
<string>mqzoneopensdkapiV2</string>
<string>mqqapiwallet</string>
<string>mqqopensdkfriend</string>
<string>mqqopensdkdataline</string>
<string>mqqgamebindinggroup</string>
<string>mqqopensdkgrouptribeshare</string>
<string>tencentapi.qq.reqContent</string>
<string>tencentapi.qzone.reqContent</string>
</array>
<key>CFBundleURLTypes</key>
<array>
<dict>
<key>CFBundleTypeRole</key>
<string>Editor</string>
<key>CFBundleURLName</key>
<string>weixin</string>
<key>CFBundleURLSchemes</key>
<array>
<string>wx67b6059559daff51</string>
</array>
</dict>
<dict>
<key>CFBundleTypeRole</key>
<string>Editor</string>
<key>CFBundleURLName</key>
<string>tencent</string>
<key>CFBundleURLSchemes</key>
<array>
<string>tencent222222</string>
</array>
</dict>
<dict>
<key>CFBundleTypeRole</key>
<string>Editor</string>
<key>CFBundleURLName</key>
<string>com.weibo</string>
<key>CFBundleURLSchemes</key>
<array>
<string>3001014611</string>
</array>
</dict>
</array>

```

#### <4>.加入调用下列函数中加入[WeiboSDK handleOpenURL:url delegate:[NikuShareSDK instance]];

- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation;
- (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url;

#### <5>.修改代码中的appID
在文件NKShareOCCS.mm中找到c_init方法，修改其中的微博，qq,及微信的appid
```
void c_init(int channel){
if(channel==SHARECHANNEL_WEIBO){
[[NikuShareSDK instance]registWeboAppId:@"3001014611"];
}else if(channel==SHARECHANNEL_QQ){
[[NikuShareSDK instance] registQQAppId:@"222222"];
}else if(channel==SHARECHANNEL_WEIXIN){
[[NikuShareSDK instance ]registWeixinAppId:@"wx67b6059559daff51" withGameName:@"暗黑信仰"];
}
}
```

## 3、SDK接口描述
所有的接口均集成在NKShareSDK中，方法均为静态方法，可直接通过类名调用。
###（1） 初始化实例
```
public static void init(AndroidJavaObject activity)

使用示例：
```
NKShareSDK.init(currentActivity);
```

### （2） 初始化渠道（必接）
```
public static void init(String channel)
```
```
//    渠道名称
public const String NKChannel_QQ = "QQ";//QQ好友
public const String NKChannel_QQzone = "QQZone";//QQ空间
public const String NKChannel_Wechat = "Wechat";//微信好友
public const String NKChannel_WechatTimeline = "WechatTimeline";//微信朋友圈
public const String NKChannel_WechatFavorite = "WechatFavorite";//微信收藏
public const String NKChannel_Weibo = "Weibo";//微博
```
或  public static void init(AndroidJavaObject activity, NKShareListener shareListener)(带监听分享结果）
```
说明：

| 参数        | 类型   |  说明  |适用平台
| --------   | -----:  | :----:  |:----:  |
| channel     | string |   渠道    | Android/iOS
| shareListener     | NKShareListener |   监听器    | Android
使用示例：
```
NKShareSDK.init(NKChannel_QQ);
或  NKShareSDK.init(NKChannel_QQ,new NKShareListener);
```
###NKShareListener(Android 监听器）
NKListener对接Android NKShareDK中的NKShareListener，可以实现初始化、分享结果等消息的相互传递，对于信息的处理，请直接在NKShareListener相应的方法内实现（切勿更改类名、方法名称）。
说明：
**目前部分渠道在分享后不会返回分享结果，因此可能造成这些渠道无法监听到分享结果，请评估后选择接入。**
#### <1>onInit
调用初始化接口后收到此回调 参数说明： errorcode 为0表示成功，-1表示未知错误
#### <2>onResult
调用分享接口后收到此回调 参数说明： errorcode 为0表示成功，-1表示未知错误,1表示用户取消
### （3）设置分享类型（必接）
```
NKShareSDK.setShareType（int shareType）;
```
说明：shareType分享类型，可在NKShareSDK中选择NKSHARE_TYPE_,包含文本（text）、图片（image）、音频（music）、视频（video）、网页（webpage），如：
```
public static void setShareType(int shareType)
```
```
//    分享数据类型
public const int NKSHARE_TYPE_TEXT = 1;//文本
public const int NKSHARE_TYPE_IMAGE = 2;//图片
public const int NKSHARE_TYPE_MUSIC = 3;//音频
public const int NKSHARE_TYPE_VIDEO = 4;//视频
public const int NKSHARE_TYPE_WEBPAGE = 5;//网页
```
### （4）设置标题或名称（必接）
```
public static void setTitle(String title)
```
如：    
```
NKShareSDK.setTitle("Test");
```
### （5）设置链接地址（必接）
```
public static void setTargetUrl(String targetUrl)
```
说明：用于分享链接
###（6）设置分享图片
```
public static void setImageUrl(String imageUrl)
```
说明：path可传递路径、网络链接等，目前仅支持本地链接及应用内图片。
###（7）设置分享具体内容（必接）
```
public static void setText(String text)
```
###（8）设置分享渠道并分享（必接）
```
public static void showShare(String channel)
```
说明：此处的channel必须与init时的channel一致
###（9）生成二维码并分享
说明：使用此方法不用再接入showShare方法了。
<1>使用游戏包内资源的图片（仅用于Android）
```
public static void showShareWithQRCode(String channel, String url, String uuid, String gameid, String roleid, String sid, String pid, int widthPix, int heightPix, int logoId, AndroidJavaObject activity)
```
说明：
| 参数        | 类型   |  说明  |
| --------   | -----:  | :----:  |
| channel     | String |   渠道    |
| url     | String |   二维码中的链接地址    |
| uuid     | String |   二维码中链接中分享用户id    |
| gid     | String |   二维码中链接中分享游戏id    |
| sid     | String |   二维码中链接中分享服务器id    |
| rid     | String |   二维码中链接中分享游戏角色id    |
| pid     | String |   二维码中链接中分享 id    |
| widthPix     | int |   分享二维码的宽，单位像素    |
| heightPix     | int |   分享二维码的高，单位像素    |
| logoId     | int |   二维码中logo的图片在R.class中的id    |
| activity     | AndroidJavaObject |   当前的Activity    |
<2>使用资源路径的图片
```
public static void showShareWithQRCode(String channel, String url, String uuid, String gameid, String roleid,String sid, String pid, int widthPix, int heightPix, String imagePath, AndroidJavaObject activity)
```
说明：
| 参数        | 类型   |  说明  |
| --------   | -----:  | :----:  |
| channel     | String |   渠道    |
| url     | String |   二维码中的链接地址    |
| uuid     | String |   分享用户id    |
| gid     | String |   二维码中链接中分享游戏id    |
| sid     | String |   二维码中链接中分享服务器id    |
| rid     | String |   二维码中链接中分享游戏角色id    |
| pid     | String |   二维码中链接中分享 id    |
| widthPix     | int |   分享二维码的宽，单位像素    |
| heightPix     | int |   分享二维码的高，单位像素    |
| imagePath     | String |   二维码中logo的图片所在路径    |
| activity     | AndroidJavaObject |   当前的Activity    |

###（10）辅助方法（主要单用）
*<1>生成二维码是否成功*
public static bool createQRImage(String uuid, String gid, String sid, String rid, String pid, int widthPix, int heightPix, String imagePath, AndroidJavaObject activity)
说明：
| 参数        | 类型   |  说明  |
| --------   | -----:  | :----:  |
| uuid     | String |   分享用户id    |
| gid     | String |   二维码中链接中分享游戏id    |
| sid     | String |   二维码中链接中分享服务器id    |
| rid     | String |   二维码中链接中分享游戏角色id    |
| pid     | String |   二维码中链接中分享 id    |
| widthPix     | int |   分享二维码的宽，单位像素    |
| heightPix     | int |   分享二维码的高，单位像素    |
| imagePath     | String |   二维码中logo的图片所在路径    |
| activity     | AndroidJavaObject |   当前的Activity    |
*<2>获取二维码完整路径*
此路径指向固定路径（根目录下的cache目录），且二维码的名称固定为qr_nikugame.png。用此方法必须先确定createQRImage是否为true，以确保生成二维码图片并存储
public static String getQRCodePath(AndroidJavaObject activity)

## 4、生命周期（仅限Android）
```
public static void onNewIntent(AndroidJavaObject intent)(必接)

public static void onCreate(AndroidJavaObject savedInstanceState)（必接）

public static void onStart()

public static void onResume()

public static void onPause()

public static void onStop()

public static void onDestroy()

public static void onRestart()

public static void onActivityResult(int requestCode, int resultCode, AndroidJavaObject data)
```
## 5、版本更新说明
ver1.0.0.1121
首个ShareSDK版本，增加了QQ、微信、微博渠道的分享，及分享结果的接口
ver1.0.0.1128
新增生成二维码并分享接口，新增微信分享图片处理方法，新增生命周期
ver1.0.0.1129
调整类名规则，防止与其他SDK产生混淆
ver1.0.0.1205
调整二维码的生成时传递的主要信息形式，采用加密以保证数据安全
ver1.0.0.1214
更新so文件，修复回调接口错误的问题（由init(AndroidJavaObject activity,，NKShareListener listener)改为init(String channel,，NKShareListener listener)）
ver1.0.1_1227
新增单独的生成二维码方法和获取二维码路径的方法，增强灵活性
ver1.0.2_170117
调整分享二维码内url的设置，由写死改为可自由设置
ver1.0.5_170307
更新性能