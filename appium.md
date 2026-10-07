```
D:\AndroidSDK\build-tools\29.0.3
D:\AndroidSDK\tools
D:\AndroidSDK\platform-tools
```



# 获取设备命

```
adb devices
```



# 查看包名和界面名

打开已安装的软件,然后在cmd输入

```
adb shell "dumpsys window | grep mCurrentFocus"
或
adb shell dumpsys activity
```



# 连接应用

```
/wd/hub


{
  "platformName": "Android",
  "appium:platformVersion": "7.1.2",
  "appium:deviceName": "64b39af8",
  "appium:appPackage": "com.android.settings",
  "appium:appActivity": "com.android.settings.SubSettings",
  "appium:automationName": "uiautomator2",
  "appium:noReset": "true"
}
```

