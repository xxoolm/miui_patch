## 短信
APK位置： `/system/priv-app/Mms/Mms.apk`

apktool命令： `apktool d -r *.apk`

### 移除广告
代码位置： `com/android/mms/util/SmartMessageUtils.smali`
```
.method public static disableAD()Z
# return true
```

APK位置： `/system/app/SmsExtra/SmsExtra.apk`

apktool命令： `apktool d -r *.apk`

### 移除底部菜单
代码位置： `com/miui/smsextra/ui/BottomMenu.smali`
```
.method public static allowMenuMode
# return false
```
**注**： 如 `SmsExtra.apk` 修改后不工作（具体表现为智能短信识别不可用），反编译时请使用 `APKDB` 进行『仅反编译内部Dex文件（smali）』操作，重新编译时使用 `7za.exe` 将 `classes.dex` 更新到apk，命令如下：
```
7za u -tzip SmsExtra.apk classes.dex
```

或者使用 `baksmali/smali`

反编译内部Dex命令：
```
java -jar baksmali.jar d SmsExtra.apk -o SmsExtra
```
编译Dex命令：
```
java -jar smali.jar a -o classes.dex SmsExtra
```
打包进APK：
```
7za u -tzip SmsExtra.apk classes.dex
```
