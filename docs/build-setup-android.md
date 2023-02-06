# Build Setup for Android

## 注意
请根据本文档严格配置，否则会导致微信无法回调你的应用，例如授权登录后无法跳回 APP，或者小程序无法拉起 APP
如果配置过程有不明白，可以查看 [样本工程](https://github.com/little-snow-fox/react-native-wechat-lib/tree/master/example)
在样本工程里搜索 '**react-native-wechat-lib support**' 便可以找到所有需要添加配置的地方

## 注册模块
添加到 `MainApplication.java` 或 `MainActivity.java`:
```java
import com.wechatlib.WeChatLibPackage; // Add this line

  @Override
  protected List<ReactPackage> getPackages() {
    @SuppressWarnings("UnnecessaryLocalVariable")
    List<ReactPackage> packages = new PackageList(this).getPackages();
    // Packages that cannot be autolinked yet can be added manually here, for example:
    // packages.add(new MyReactNativePackage());
    packages.add(new WeChatLibPackage()); // Add this line
    return packages;
  }
```

## 登录和分享回调
如果您打算集成登录或共享功能，则需要这样做
在应用程序包和类中创建名为 "wxapi" 的包, 命名为 WXEntryActivit
```java
package your.package.wxapi;

import android.app.Activity;
import android.os.Bundle;
import com.wechatlib.WeChatLibModule;

public class WXEntryActivity extends Activity {
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    WeChatLibModule.handleIntent(getIntent());
    finish();
  }
}
```

Then add the following node to `AndroidManifest.xml`:

```xml
<manifest>
  <application>
    <activity
      android:name=".wxapi.WXEntryActivity"
      android:label="@string/app_name"
      android:exported="true"
    />
  </application>
</manifest>
```

**Integrating the WeChat Payment**

If you are going to integrate payment functionality by using this library, then
create a package named also `wxapi` in your application package and a class named
`WXPayEntryActivity`, this is used to bypass the response to JS level:

```java
package your.package.wxapi;

import android.app.Activity;
import android.os.Bundle;
import com.theweflex.react.WeChatModule;

public class WXPayEntryActivity extends Activity {
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    WeChatModule.handleIntent(getIntent());
    finish();
  }
}
```

Then add the following node to `AndroidManifest.xml`:

```xml
<manifest>
  <application>
    <activity
      android:name=".wxapi.WXPayEntryActivity"
      android:label="@string/app_name"
      android:exported="true"
    />
  </application>
</manifest>
```

如果遇到安卓App拉起小程序后，小程序无法返回App的情况，需要在AndroidManifest.xml的WXEntryActivity中添加下面这段配置:
```
android:taskAffinity="your packagename"
android:launchMode="singleTask"
```
保证跳转后回到你的app的task。
实际上，我的代码如下：
```xml
<manifest>
  <application>
    <activity
      android:name=".wxapi.WXEntryActivity"
      android:label="@string/app_name"
      android:exported="true"
      android:taskAffinity="org.xxx.xxx.rnapp"
      android:launchMode="singleTask"
    />
  </application>
</manifest>
```
