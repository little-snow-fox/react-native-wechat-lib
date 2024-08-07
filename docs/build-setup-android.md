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

## 支付回调

If you are going to integrate payment functionality by using this library, then
create a package named also `wxapi` in your application package and a class named
`WXPayEntryActivity`, this is used to bypass the response to JS level:

```java
package your.package.wxapi;

import android.app.Activity;
import android.os.Bundle;
import com.wechatlib;

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

## 分享本地文件
如果你需要分享本地文件，需要在 Android 的工程里进行一些设置，否则会有权限问题

步骤 1：app/src/main/AndroidManifest.xml 中添加 provider 标签，其中 com.yourapp.xxx 要替换为你自己的包名，记得保留后面的 `.fileprovider`

```xml
<application ...>
    ...
    <provider
        android:name="androidx.core.content.FileProvider"
        android:authorities="com.yourapp.xxx.fileprovider"
        android:exported="false"
        android:grantUriPermissions="true">
        <meta-data
            android:name="android.support.FILE_PROVIDER_PATHS"
            android:resource="@xml/filepaths" />
    </provider>
    ...
</application>
```

步骤 2：实现 app/src/main/res/xml/filepaths.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <external-files-path name="myexternalimages" path="Download/" />
    <cache-path name="my_cache_files" path="." />
    <root-path name="root" path="" />
    <files-path name="files-path" path="." />
</paths>
```

在这个 XML 文件中，你可以定义不同的路径类型（如 cache-path、external-path 等），以及对应的路径前缀。这样，在使用 FileProvider.getUriForFile() 时，就可以根据这些定义来获取正确的 URI。

请注意，当组件库被集成到不同的应用中时，你可能需要根据你自己的需求调整 filepaths.xml 中的路径定义。

## 关于 Android11
微信将于近期发布 targetSdkVersion 30的客户端版本，因Android11系统特性，该微信版本在Android 11及以上系统版本的设备上运行时，授权登录、分享、微信支付等功能受到影响，可能无法正常使用。为了适配 Android 系统新版本特性，保证微信功能正常使用，请第三方应用2021年11月1日之前进行更新

在自己 React Native 项目的 android/app/src/main/AndroidManifest.xml 中添加:
```$xml
<queries>
    <!--
      微信 Android 11-更新 openSDK 适配
      参见 https://developers.weixin.qq.com/doc/oplatform/Mobile_App/Access_Guide/Android.html#jump2
    -->
    <package android:name="com.tencent.mm" />
/queries>
```
