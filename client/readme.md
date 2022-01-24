# EasyGame Android SDK
### 1. 簡介
歡迎使用 EasyGames Andoird SDK，當前最新版本為4.8.943。

### 2. 參數
#### 2.1 EASYGAMES_APP_ID
由我方分配給遊戲的應用id。
#### 2.2 EASYGAMES_PUBLISHMENT_AREA
發行地區編號，參數值為2。
#### 2.3 EASYGAMES_PAY_CHANNEL
支付類型，參數值為2。
#### 2.4 EASYGAMES_TRACK_KEY
由我方分配的事件追蹤的公鑰。
#### 2.5 google_public_key
在Goole Play商店後臺上生成的支付公鑰。
#### 2.6 google_client_id
在Google API後臺上生成的谷歌登錄驗證所需的client id。
#### 2.7 com.facebook.sdk.ApplicationId
在Facebook後臺上生成的應用id。
### 3. 環境搭建
#### 3.1 gradle版本及庫引用設置
gradle版本為5.6.4（僅供參考），並且請在當前Project目錄下的build.gralde文件中加上如下配置：
```gradle
buildscript {
    repositories {
    	google()
        mavenCentral()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.4.2'
	
        // 如果使用Firebase雲消息推送功能，請打開以下配置 
    	// classpath 'com.google.gms:google-services:4.2.0'
    }
}

allprojects {
    repositories {
        google()
	mavenCentral()
    }
}
```
如果使用 Firebase 雲消息推送功能，請在當前遊戲工程目錄下的build.gradle文件中加上如下配置：
```gradle
apply plugin: 'com.google.gms.google-services'
```
如果使用 LINE 登錄，請在當前Project目錄下的gradle.properties文件中加上如下配置：
```gradle
android.enableD8.desugaring=true
```
另外，還需要在當前Project目錄下的gradle.properties文件中加上如下配置：
```gradle
EASYGAMES_SDK_VERSION=4.8.943
```
#### 3.2 lib 選擇
針對於在港臺地區發行的遊戲，請在當前Module目錄下的「build.gradle」文件裏打開如下圖所示的配置：<br/>
```gradle
repositories {
    flatDir {
        dirs 'libs'
    }
}

dependencies {
    // base begin
    api "io.github.sonicdjgh:platform:$EASYGAMES_SDK_VERSION@aar"
    api "io.github.sonicdjgh:payment:$EASYGAMES_SDK_VERSION@aar"
    api "io.github.sonicdjgh:support:$EASYGAMES_SDK_VERSION@aar"
    api 'com.android.support.constraint:constraint-layout:1.1.0'
    // base 
    
    // appsflyer begin
    api 'com.appsflyer:af-android-sdk:4+@aar'
    api 'com.android.installreferrer:installreferrer:1.0'
    // appsflyer end

    // google begin
    api 'com.google.android.gms:play-services-auth:16.+'
    api 'com.google.android.gms:play-services-base:16.+'
    api 'com.google.android.gms:play-services-basement:16.+'
    api 'com.google.android.gms:play-services-drive:16.+'
    api 'com.google.android.gms:play-services-games:16.+'
    api 'com.google.android.gms:play-services-gcm:16.+'
    api 'com.google.android.gms:play-services-iid:16.+'
    api 'com.google.android.gms:play-services-tasks:16.+'
    
    // googleplay begin
    // 如果使用 GooglePlay 支付，請打開下面的配置
    // api 'com.android.billingclient:billing:3.0.2'
    // googleplay end
    
    // firebase begin
    // 如果使用 Firebase 雲消息推送，請打開下面的配置
    // api 'com.google.firebase:firebase-core:16.0.8'
    // api 'com.google.firebase:firebase-messaging:18.0.0'
    // firebase end
    // google end
    
    // facebook begin
    api 'com.facebook.android:facebook-core:8.+'
    api 'com.facebook.android:facebook-login:8.+'
    api 'com.facebook.android:facebook-share:8.+'
    // facebook end
    
    // LINE begin
    // 如果使用 LINE 登錄，請打開下面的配置
    // api 'com.linecorp:linesdk:5.0.1'
    // LINE begin
}

```
#### 3.3 關於Unity的SDK接入
a. 首先使用Android Studio自建一個安卓項目工程後並完成SDK的接入工作；<br/><br/>
b. 請註意，遊戲主Activity需要繼承Unity的UnityPlayerActivity；<br/><br/>
c. Google推薦對危險權限的使用有一定要求，需要加入申請權限的邏輯。但由於Unity會自動申請「AndroidManifest.xml」文件中所配置的危險權限，不便於邏輯控製。如果有需要，請在「AndroidManifest.xml」文件中的「application」標簽內加入如下配置：
```Xml
<meta-data
    android:name="unityplayer.SkipPermissionsDialog"
    android:value="true" />
```
d. 如果發現SDK的懸浮窗無法響應手勢動作，請在「AndroidManifest.xml」文件中的「application」標簽內加入如下配置：
```Xml
<meta-data 
    android:name="unityplayer.ForwardNativeEventsToDalvik" 
    android:value="true"/>
```
#### 3.4 其他
minSdkVersion = 17，targetSdkVersion = 30
### 4. AndroidManifest.xml文件配置
#### 4.1 AndroidManifest.xml中的參數配置
```gradle
// 在遊戲Module的「build.gradle」中的「defaultConfig」裏添加如下配置：
manifestPlaceholders = [
                // base begin
                EGLS_APP_ID              : "",// 用於SDK初始化 
                EGLS_PUBLISHMENT_AREA    : "2",// 用於SDK識別發行區
                EGLS_PAY_CHANNEL         : "2",// 用於SDK識別支付方式
		EASYGAMES_TRACK_KEY	 : "",// 用於SDK事件追蹤初始化
                EGLS_PAY_IS_SANDBOX      : "false",// 設為false即可
		
		GOOGLE_WEB_CLIENT_ID     : "",// 用於SDK的Google登錄
		FACEBOOK_APPLICATION_ID  : "",// 用於SDK的Facebook登錄
		LINE_CHANNEL_ID          : "",// 用於SDK的LINE登錄
		
		// APPS_FLYER_DEV_KEY    : "",// 用於AppsFlyer統計功能初始化，如果運營沒有特殊需求，這裏無需添加
                // base end
		
		// other begin
		GOOGLE_PLAY_PUBLIC_KEY   : "",// 用於SDK的Google Play支付，若無需求可不填
		GOOGLE_GAME_APP_ID       : "",// 用於SDK的Google Game成就系統，若無需求可不填
                // other end
        ]
```
#### 4.2 Permission 配置
```Xml
<!-- AppsFlyer begin -->
<!-- 如果現在接入的安卓包是針對除Google Play以外的其他應用商店，那麽此權限一定需要聲明，否則要刪除該權限聲明 -->
<!-- <uses-permission android:name="android.permission.READ_PHONE_STATE" /> -->
<!-- AppsFlyer end -->


<!-- Google Play begin -->
<!-- 如果使用Google Play支付功能，請打開以下配置 -->
<!--
<uses-permission android:name="com.android.vending.BILLING" />
<uses-feature
    android:name="android.hardware.camera"
    android:required="false" />
<uses-feature
    android:name="android.hardware.camera.autofocus"
    android:required="false" />
<uses-feature
    android:name="android.hardware.telephony"
    android:required="false" />
<uses-feature
    android:name="android.hardware.microphone"
    android:required="false" />
-->
<!-- Google Play end -->
```
請註意：以上 Permission 配置中只打開了SDK基礎功能相關的配置，如果使用到其他功能，請打開對應的 Permission 配置！
#### 4.3 Application相關配置
```Xml
<!-- 請註意Application標簽中的「android:networkSecurityConfig」以及「android:requestLegacyExternalStorage」屬性的設置 -->
</application
    android:name="com.easygames.demo.GameApplication"
    android:allowBackup="false"
    android:icon="@drawable/icon"
    android:label="EasyGames SDK Demo"
    android:networkSecurityConfig="@xml/network_security_config"
    android:requestLegacyExternalStorage="true">
	
    <!-- 遊戲Activity -->	
    <activity
        android:name="com.easygames.demo.GameActivity"
        android:configChanges="fontScale|orientation|keyboardHidden|locale|navigation|screenSize|uiMode"
        android:screenOrientation="landscape"
        android:theme="@style/EglsTheme.NoTitleBar.Fullscreen.NoAnimation" >
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />

            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
        <!-- DeepLink begin -->
        <intent-filter>
            <data
                android:host="${applicationId}"
                android:scheme="easygames${EASYGAMES_APP_ID}" />

            <action android:name="android.intent.action.VIEW" />

            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
        </intent-filter>
        <!-- DeepLink end -->
    </activity>
	
    <!-- Base begin -->
    <meta-data
        android:name="EASYGAMES_APP_ID"
        android:value="${EASYGAMES_APP_ID}" />
	
    <meta-data
        android:name="EASYGAMES_PUBLISHMENT_AREA"
        android:value="${EASYGAMES_PUBLISHMENT_AREA}" />
	
    <meta-data
        android:name="EASYGAMES_PAY_CHANNEL"
        android:value="${EASYGAMES_PAY_CHANNEL}" />

    <meta-data
        android:name="EASYGAMES_TRACK_KEY"
        android:value="${EASYGAMES_TRACK_KEY}" />
	
    <meta-data
        android:name="EASYGAMES_PAY_IS_SANDBOX"
        android:value="${EASYGAMES_PAY_IS_SANDBOX}" />

    <!-- 默認值，如有需求可更改 -->
    <meta-data
        android:name="EASYGAMES_DOMAIN"
        android:value="passport.in99.com.tw" />
    <!-- Base end -->
        

    <!-- AppsFlyer begin -->
    <!-- 為了確保所有Install Referrer監聽器可以成功監聽由系統播放的referrer參數，請一定在AndroidManifest.xml中將AppsFlyer的監聽器置於所有同類監聽器第一位，並保證receiver tag在application tag中 -->
    <!-- 如果已經有其他的receiver來監聽「INSTALL_REFERRER」， 那麽請用「MultipleInstallBroadcastReceiver」 -->
    <receiver
        android:name="com.appsflyer.SingleInstallBroadcastReceiver"
        android:exported="true" >
        <intent-filter>
            <action android:name="com.android.vending.INSTALL_REFERRER" />
        </intent-filter>
    </receiver>
    
    <meta-data
        android:name="appsflyer_enable"
        android:value="true" />
    
    <!-- 如果有特殊需求修改devkey時，請打開以下配置 -->	
    <!--	
    <meta-data
        android:name="appsflyer_dev_key"
        android:value="${APPS_FLYER_DEV_KEY}" />
    -->
    <!-- AppsFlyer end -->


    <!-- Google begin -->
    <meta-data
        android:name="google_client_id"
        android:value="${GOOGLE_WEB_CLIENT_ID}" />
	
    <!-- 如果使用Firebase雲消息推送，請打開以下配置 -->
    <!--
    <service
        android:name="com.easygames.support.google.firebase.FirebaseMesgService"
        android:exported="false">
        <intent-filter>
            <action android:name="com.google.firebase.MESSAGING_EVENT" />
        </intent-filter>
    </service>
    -->
	
    <!-- 如果使用Firebase雲消息推送，請打開以下配置 -->
    <!-- Firebase雲消息推送所使用的icon圖案 -->
    <!--
    <meta-data
        android:name="com.google.firebase.messaging.default_notification_icon"
        android:resource="@drawable/egls_push_icon" />
    -->
	
    <!-- 如果使用Firebase雲消息推送，請打開以下配置 -->
    <!-- Firebase雲消息推送所使用的icon底色 -->
    <!--
    <meta-data
        android:name="com.google.firebase.messaging.default_notification_color"
        android:resource="@color/egls_push_color" />
    -->
	
    <!-- 如果使用Google Play Game成就功能，請打開以下配置 -->
    <!--
    <meta-data
        android:name="com.google.android.gms.games.APP_ID"
        android:value="\0${GOOGLE_GAME_APP_ID}" />
    -->

    <!-- 如果使用Google Play支付功能，請打開以下配置 -->
    <!--
    <meta-data
        android:name="google_public_key"
        android:value="${GOOGLE_PLAY_PUBLIC_KEY}" />
    -->
    <!-- Google end -->
    

    <!-- Facebook begin -->
    <meta-data
        android:name="com.facebook.sdk.ApplicationId"
        android:value="\0${FACEBOOK_APPLICATION_ID}" />

    <!--如果遊戲需要開啟Facebook的「USER_FRIEND」權限，請打開以下配置 -->
    <!--
    <meta-data
        android:name="facebook_user_friends_enable"
        android:value="true" />
    -->
						    
    <!-- 如果遊戲需要使用Facebook分享功能，請打開以下配置 -->
    <!--
    <provider
        android:name="com.facebook.FacebookContentProvider"
        android:authorities="com.facebook.app.FacebookContentProvider${FACEBOOK_APPLICATION_ID}"
        android:exported="true" />
    -->
    <!-- Facebook end  -->


    <!-- LINE begin -->
    <!-- 如果使用 LINE 登入，請打開以下配置 -->
    <!--
    <meta-data
        android:name="line_channel_id"
        android:value="${LINE_CHANNEL_ID}" />
    -->
    <!-- LINE end -->
</application>
```

### 5. 基礎方法實現（必接）
```Java 
@Override
protected void onCreate() {
    super.onCreate();
    GamePlatform.onCreate(this);
}

@Override
protected void onResume() {
    super.onResume();
    GamePlatform.onResume(this);
}
    
@Override
protected void onPause() {
    super.onPause();
    GamePlatform.onPause(this);
}
	
@Override
protected void onDestroy() {
    super.onDestroy();
    GamePlatform.onDestroy(this);
}
```

### 6. SDK初始化（必接）
```Java
// 請在你的Application類中，按照如下進行接口的調用：
@Override
public void onCreate() {
    super.onCreate();
    GameTracker.initApplication(this);
    GamePlatform.initApplication(this);
}
```
```Java
// 請在你的Activity類中，按照如下進行接口的調用：
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    EglsPlatform.setSDKActionHandler(new SDKActionHandler() {

        @Override
        public void onHandleInit(int state, String message) {
            if (state == Constants.SDK_STATE_SUCCESS) {// 初始化成功後的處理
                // 如果需要使用LINE登錄，請調用如下接口
		// GamePlatform.Config.setEnableLineSignIn(true);
		// 如果需要使用SDK懸浮窗導航欄中的登出，請調用如下接口
		// GamePlatform.Config.setEnableFloateMenuLogout(true);
            } else {// 初始化失敗後的處理
                
            }
        }

        @Override
        public void onHandleAgreement(boolean isAgree) {// SDK是否同意用戶協議的結果處理
            if (isAgree) {
                // 玩家選擇同意協議後的處理
            } else {
                // 玩家選擇不同意協議後的處理
            }
        }

        @Override
        public void onHandleLogin(int state, String token, String uid, String accountType, String nickName, String message) {// SDK登錄的結果處理
            switch (state) {
                case Constants.SDK_STATE_SUCCESS:// 登錄成功後的處理
                    // accountType = "0"時，表示遊客賬號登錄
                    // accountType = "1"時，表示EGLS賬號登錄
                    // accountType = "2"時，表示Google賬號登錄
                    // accountType = "3"時，表示Facebook賬號登錄
                    break;
                case Constants.SDK_STATE_CANCEL:// 登錄取消後的處理
                    break;
                case Constants.SDK_STATE_ERROR:// 登錄失敗後的處理
                    break;
            }
        }
	
	@Override
        public void onHandleLogout() {
	    // 只有當點擊懸浮窗中登出按鈕後才會觸發
        }

        @Override
        public void onHandleChannelBind(int state, String accountType, String nickName, String message) {// SDK綁定的結果處理
            switch (state) {
                case Constants.SDK_STATE_SUCCESS:// 綁定成功後的處理
                    break;
                case Constants.SDK_STATE_CANCEL:// 綁定取消後的處理
                    break;
                case Constants.SDK_STATE_ERROR:// 綁定失敗後的處理
                    break;
            }
        }

        @Override
        public void onHandleTokenFailure() {// SDK登錄token失效的結果處理（需要遊戲實現返回到登錄頁面的邏輯）

        }

        @Override
        public void onHandlePurchase(int state, TradeInfo tradeInfo, String message) {// SDK支付的結果處理
            switch (state) {
                case Constants.SDK_STATE_SUCCESS:// 支付完成後的處理（僅表示客戶端支付操作完成，最終要以服務器的通知為準）
                    break;
                case Constants.SDK_STATE_CANCEL:// 支付取消後的處理
                    break;
                case Constants.SDK_STATE_ERROR:// 支付失敗後的處理
                    break;
            }
        }

        @Override
        public void onHandleShare(int state, int type, String message) {// SDK分享的結果處理
            switch (state) {
                case Constants.SDK_STATE_SUCCESS:// 分享成功後的處理
                    // 當type為Constants.TYPE_SHARE_FACEBOOK時，表示Facebook分享
                    // 當type為Constants.TYPE_SHARE_LINE時，表示LINE分享
                    break;
                case Constants.SDK_STATE_CANCEL:// 分享失成功的處理
                    break;
                case Constants.SDK_STATE_ERROR:// 分享失敗後的處理
                    break;
            }
        }

        @Override
        public void onHandleExit(boolean isExit) {// SDK退出遊戲的結果處理（該方法只針對遊戲調用SDK的"EglsPlatform.Support.exit()"接口的響應）
            if (isExit) {
                // 玩家選擇退出後的處理
            } else {
                // 玩家選擇繼續遊戲後的處理
            }
        }
    });
}
```

### 7. Account模塊接口
「Account」模塊中包含了與賬號相關的功能接口。
#### 7.1 SDK UI Interface （主要適用於遊戲）
在「Account」模塊裏所包含的接口名稱中，帶有「game」詞綴的接口，在調用時，會根據業務功能自身需求，來展示所需要的UI。
#### 7.1.1 egls登錄
```Java
GamePlatform.Account.gameLogin(this, Constants.MODE_LOGIN_AUTO);
```
#### 7.1.2 egls切換賬號
```Java
GamePlatform.Account.gameSwitch(this);
```
#### 7.1.3 egls用戶中心
```Java
GamePlatform.Account.gameUserCenter(this);
```
<details>
<summary>7.2 SDK Lightly Interface （主要適用於應用）</summary><br />
	
在「Account」模塊裏所包含的接口名稱中，帶有「Lightly」詞綴的接口，在調用時，不會顯示SDK自身集成的相關UI。
#### 7.2.1 手機登錄 
```Java
// 即傳入手機號、密碼後進行登錄
// 響應登錄回調，賬號類型為：Constants.TYPE_USER_ACCOUNT_EGLS
GamePlatform.Account.mobileLoginLightly(Activity activity, String mobile, String password)
```
#### 7.2.2 郵箱登錄
```Java
// 即傳入電子郵箱、密碼後進行登錄
// 響應登錄回調，賬號類型為：Constants.TYPE_USER_ACCOUNT_EGLS
GamePlatform.Account.mailLoginLightly(Activity activity, String mail, String password)
```
#### 7.2.3 渠道登錄
```Java
// 即根據傳入的賬號類型來調用對應的渠道登錄，這裏支持谷歌、Facebook登錄
// 響應登錄回調，返回登錄的賬號類型
// 另外，當accountType為空時，將采取默認登錄，如果沒有最近一次的登錄記錄，則進行遊客登錄；否則選擇最近一次的登錄賬號進行登錄
GamePlatform.Account.channelLoginLightly(Activity activity, String accountType)
```
#### 7.2.4 手機註冊
```Java
// 手機註冊第一步為「手機註冊驗證」，即傳入手機號後，發送驗證碼到手機上
// 響應接口裏傳入的回調，根據state狀態來識別是否發送成功，message可用於消息提示
EglsPlatform.Account.mobileRegisterVerifyLightly(String mobile, OnSimpleActionCallback callback)

// 手機註冊第二步為「手機註冊請求」，即傳入手機號、驗證碼及密碼後，請求註冊
// 響應登錄回調，賬號類型為：Constants.TYPE_USER_ACCOUNT_EGLS
GamePlatform.Account.mobileRegisterRequestLightly(String mobile, String verificationCode, String password)
```
#### 7.2.5 郵箱註冊
```Java
// 郵箱註冊第一步為「郵箱註冊驗證」，即傳入電子郵箱後，發送驗證碼到電子郵箱上
// 響應接口裏傳入的回調，根據state狀態來識別是否發送成功，message可用於消息提示
GamePlatform.Account.mailRegisterVerifyLightly(String mail, OnSimpleActionCallback callback)

// 郵箱註冊第二步為「郵箱註冊請求」，即傳入電子郵箱、驗證碼及密碼後，請求註冊
// 響應登錄回調，賬號類型為：Constants.TYPE_USER_ACCOUNT_EGLS
EglsPlatform.Account.mailRegisterRequestLightly(String mail, String verificationCode, String password)
```
#### 7.2.6 渠道註銷
```Java
// 即根據傳入的賬號類型來調用對應的渠道註銷，當再次請求該渠道登錄時，用戶可以重新選擇賬號
// 需要註意的是，有些渠道SDK是不提供主動註銷的邏輯接口的（比如Facebook的app登錄，如果此時手機上裝有Facebook應用，那麽需要先在應用裏切換賬號）
// 另外，當accountType為空時，將采取默認註銷，即註銷當前所有的渠道登錄
GamePlatform.Account.channelLogoutLightly(String accountType) 
```
#### 7.2.7 手機綁定
```Java
// 手機綁定第一步為「手機綁定驗證」，即傳入手機號後，發送驗證碼到手機上
// 響應接口裏傳入的回調，根據state狀態來識別是否發送成功，message可用於消息提示
GamePlatform.Account.mobileBindVerifyLightly(String mobile, OnSimpleActionCallback callback)

// 手機綁定第二步為「手機綁定請求」，即傳入手機號、驗證碼及密碼後，請求綁定
// 響應綁定回調
// 需要註意的是，若為遊客賬號請求的綁定，在綁定成功後，遊客賬號變為手機賬號（uid、token不變）；否則即添加了一個手機登錄方式，當前登錄的賬號類型不變
// 目前，傳入的密碼對於非遊客賬號進行的綁定，是無效的
GamePlatform.Account.mobileBindRequestLightly(String mobile, String verificationCode, String password) 
```
#### 7.2.8 郵箱綁定
```Java
// 郵箱綁定第一步為「郵箱綁定驗證」，即傳入電子郵箱後，發送驗證碼到電子郵箱上
// 響應接口裏傳入的回調，根據state狀態來識別是否發送成功，message可用於消息提示
GamePlatform.Account.mailBindVerifyLightly(String mail, OnSimpleActionCallback callback) 

// 郵箱綁定第二步為「郵箱綁定請求」，即傳入電子郵箱、驗證碼及密碼後，請求綁定
// 響應綁定回調
// 需要註意的是，若為遊客賬號請求的綁定，在綁定成功後，遊客賬號變為郵箱賬號（uid、token不變）；否則即添加了一個郵箱登錄方式，當前登錄的賬號類型不變
// 目前，傳入的密碼對於非遊客賬號進行的綁定，是無效的
GamePlatform.Account.mailBindRequestLightLy(String mail, String verificationCode, String password)
```
#### 7.2.9 渠道綁定
```Java
// 即根據傳入的賬號類型來調用對應的渠道綁定，這裏支持谷歌、Facebook登錄
// 響應綁定回調
// 需要註意的是，若為遊客賬號請求的綁定，在綁定成功後，遊客賬號變為渠道賬號（uid、token不變）；否則即添加了一個渠道登錄方式，當前登錄的賬號類型不變
GamePlatform.Account.channelBindLightly(Activity activity, String accountType)
```
#### 7.2.10 密碼修改
```Java
// 即修改當前通過手機或郵箱登錄的賬號的登錄密碼
// 響應接口裏傳入的回調，根據state狀態來識別是否修改成功，message可用於消息提示
GamePlatform.Account.pwdModifyLightly(String password, OnSimpleActionCallback callback)
```
#### 7.2.11 密碼重置
```Java
// 密碼重置第一步為「密碼重置鑒權」，即傳入手機號或電子郵箱後，發送驗證碼到手機或電子郵箱上
// 響應接口裏傳入的回調，根據state狀態來識別是否發送成功，message可用於消息提示
GamePlatform.Account.pwdResetCaptchaLightly(String userAccount, OnSimpleActionCallback callback) 

// 密碼重置第二步為「密碼重置請求」，即傳入手機號或電子郵箱、鑒權碼後，請求密碼重置
// 響應接口裏傳入的回調，根據state狀態來識別是否重置成功，message可用於消息提示
GamePlatform.Account.pwdResetRequestLightly(String userAccount, String captcha, OnSimpleActionCallback callback)
```
</details>


### 7.3 Other Interface
#### 7.3.1 賬號進入
```Java
//在完成登錄後，當玩家角色進入到服務器或是應用用戶進入到主頁面時，需要調用該方法
GamePlatform.Account.onAccountEnter(this);
```

### 8 Payment模塊接口
「Payment」模塊中包含了與支付相關的功能接口。
#### 8.1 SDK UI Interface （主要適用於遊戲）
在「Payment模塊接口」模塊裏所包含的接口名稱中，帶有「game」詞綴的接口，在調用時，會根據業務功能自身需求，來展示所需要的UI。
#### 8.1.1 egls支付
```Java
String amount = "1.0";// 總金額
String productId = "PDT001";// 檔位id
String productName = "鉆石";// 檔位名稱
String cpOrderInfo = "2SDF34DF12GH0S23234GAER5";// CP訂單信息，由接入方生成
GamePlatform.Payment.gamePurchase(amount, productId, productName, cpOrderInfo, Constants.FLAG_PURCHASE_DEFAULT);
```
#### 8.1.2 egls訂閱（僅支持Google訂閱）
```Java
String amount = "1.0";// 總金額
String productId = "PDT002";// 檔位id
String productName = "月卡";// 檔位名稱
String cpOrderInfo = "2SDF34DF12GH0S23234GAER6";// CP訂單信息，由接入方生成
GamePlatform.Payment.gameSubscribe(amount, productId, productName, cpOrderInfo);
```
<details>
<summary>8.2 SDK Lightly Interface （主要適用於應用）</summary><br />
	
在「Payment模塊接口」模塊裏所包含的接口名稱中，帶有「Lightly」詞綴的接口，在調用時，不會顯示SDK自身集成的相關UI。
#### 8.2.1 渠道支付
```Java
String amount = "1.0";// 總金額
String productId = "PDT001";// 檔位id
String productName = "鉆石";// 檔位名稱
String cpOrderInfo = "2SDF34DF12GH0S23234GAER5";// CP訂單信息，由接入方生成
GamePlatform.Payment.channelPurchaseLightly(this, amount, productId, productName, cpOrderInfo, Constants.FLAG_PURCHASE_DEFAULT);
```
#### 8.2.2 渠道訂閱（僅支持Google訂閱）
```Java
String amount = "1.0";// 總金額
String productId = "PDT002";// 檔位id
String productName = "月卡";// 檔位名稱
String cpOrderInfo = "2SDF34DF12GH0S23234GAER6";// CP訂單信息，由接入方生成
GamePlatform.Payment.channelPurchaseLightly(this， amount, productId, productName, cpOrderInfo);
```
</details>

### 9. Social模塊接口
「Social」模塊中包含了與社交相關的功能接口。
#### 9.1 渠道分享
```Java
int type = Constants.TYPE_SHARE_FACEBOOK;
String shareTitle = "";// 分享標題
String shareText = "";// 分享文本
String shareImageFilePath = "";// 分享圖片（絕對路徑）
String shareLink = "";// 分享鏈接
boolean isTimelineCb = false;
GamePlatform.Social.channelShare(this, type, shareTitle, shareText, shareImageFilePath, shareLink, isTimelineCb);
```

### 10. Support模塊接口
「Support」模塊中包含了輔助相關的功能接口。
#### 10.1 遊戲退出
```Java
GamePlatform.Support.exit();
```
#### 10.2 Facebook遊戲邀請
```Java
String title = "Let's go!";
String text = "Yeah!";
GamePlatform.Support.getFacebookHelper().requestGameInvitation(this, title, text, new FacebookHelper.FacebookGameInvitationCallback() {

    @Override
    public void onSuccess(List<FacebookInvitedFriend> facebookInvitedFriends) {
	for (FacebookInvitedFriend friend : facebookInvitedFriends) {
            // friend.getNickName() 邀請好友的昵稱
	    // friend.getUid() 邀請好友的uid
     	    // friend.getPicUrl() 邀請好友的頭像地址
	}
    }

    @Override
    public void onCancel() {
    	
    }

    @Override
    public void onError(String message) {
	
    }
});
```
#### 10.3 Facebook用戶好友信息獲取
所謂「Facebook用戶好友」，就是指使用相同app的Facebook好友，並不只是Facebook好友。目前，如果遊戲裏沒有相關功能的需求，則不建議使用該接口（該接口的使用，需要通過Facebook的登錄審核）。
```Java
GamePlatform.Support.getFacebookHelper().getUserFriends(this, new FacebookHelper.FacebookGetUserFriendsCallback() {

    @Override
    public void onResponse(List<FacebookUserFriend> facebookUserFriends) {
	for (FacebookUserFriend friend : facebookUserFriends) {
	    // friend.getNickName() 用戶好友的昵稱
	    // friend.getUid() 用戶好友的uid
     	    // friend.getPicUrl() 用戶好友的頭像地址
	}
    }
});
```

### 11. Firebase雲消息推送（選接）
當有需要使用Firebase的雲消息推送時，首先請在遊戲項目的「/res/drawable」目錄下，添加一張名為「egls_push_icon」的圖片。然後，除了按照對接文檔中「3.1」、「3.4」和「4.4」的說明進行配置以外，還需要從Google後臺下載一個名為「google-services.json」的文件（該文件由我方運營提供），並將該文件放在當前遊戲Module工程目錄下，如下圖所示：<br/>
![image](https://github.com/sonicdjgh/egls-android-game-sdk-release-studio/blob/master/res/S4001.png)<br/>


### 12. AppsFlyer數據統計（根據運營需求對接）
AppsFlyer主要用於Global業務的數據統計，啟用該功能的做法，首先要按照上面所提到的，在AndroidManifest.xml文件中打開對應的配置。對於AppsFlyer統計功能的相關接口調用，其相關初始化部分的邏輯已經嵌入進SDK當中，因此開發者無需關心較為復雜的初始化步驟，只需根據需求，調用對應的接口即可。<br /><br />
#### 12.1 閃屏動畫首次啟動事件追蹤（必接）
```Java
GameTracker.trackEventCustom(GameTracker.EVENT_ONE_SPLASH_IMAGE, null);
```
#### 12.2 新手任務開始事件追蹤（必接）
```Java
GameTracker.trackEventCustom(GameTracker.EVENT_TUTORIAL_START, null);
```
#### 12.3 新手任務完成事件追蹤（必接）
```Java
GameTracker.trackEventCustom(GameTracker.EVENT_TUTORIAL_COMPLETE, null);
```
#### 12.4 創建新角色事件追蹤（必接）
```Java
GameTracker.trackEventCustom(GameTracker.EVENT_NEW_CHARACTER, null);
```
#### 12.5 遊戲資源首次更新開始事件追蹤（必接）
```Java
GameTracker.trackEventCustom(GameTracker.EVENT_ONE_UPDATE_START, null);
```
#### 12.6 遊戲資源首次更新完成事件追蹤（必接）
```Java
GameTracker.trackEventCustom(GameTracker.EVENT_ONE_UPDATE_COMPLETE, null);
```
#### 12.7 遊戲資源首次加載開始事件追蹤（必接）
```Java
GameTracker.trackEventCustom(GameTracker.EVENT_ONE_LOAD_START, null);
```
#### 12.8 遊戲資源首次加載完成事件追蹤（必接）
```Java
GameTracker.trackEventCustom(GameTracker.EVENT_ONE_LOAD_COMPLETE, null);
```
#### 12.9 自定義事件追蹤()（根據需求接入）
```Java
// 有時候運營會針對具體的數據分析增加特定的事件統計，那麽請調用該接口，傳入特定的事件名稱
// trackData的格式為json字符串，形如：{key:value,key:value,key:value...}
GameTracker.trackEventCustom(trackEvent, trackData);
```

### 13. 其他註意事項
1. Google推薦的審核中，會對遊戲首次運行時所使用的必要「危險權限」的申請和使用進行檢查。SDK會主動申請「android.permission.WRITE_EXTERNAL_STORAGE」權限，但如果遊戲還另需申請其他的「危險權限」，可以在調用「EglsPlatform.initActivity()」接口前，使用「addNecessaryPermission()」接口。例如：
```Java
GamePlatform.Config.addNecessaryPermission(Manifest.permission.READ_PHONE_STATE);
GamePlatform.Config.addNecessaryPermission(Manifest.permission.RECORD_AUDIO);
```
2. 同樣也是為了適應Google推薦的審核要求，SDK在遊戲第一次安裝並啟動後，會先彈出一個關於危險權限使用的說明。SDK默認的說明只有關於SD卡權限的使用說明，如果遊戲在初始化時有使用到其他的危險權限，那麽可以在調用「GamePlatform.initActivity()」接口前，使用如下方法來修改提示文本：
```Java
// 需要註意的是，該接口是直接替換原默認文本的，所以還需要加上SD卡權限的使用說明。
String permissionContent = "xxx";
GamePlatform.Config.setPermissionContent(permissionContent);
```
