## 负一屏（信息助手）
APK位置： `/system/priv-app/PersonalAssistant/PersonalAssistant.apk`

apktool命令： `apktool d -r *.apk`

### 移除运动计步关联的小米VIP，使其点击无效
代码位置： `com/miui/home/launcher/assistant/ui/view/SportCardView.smali`
```
.method public onClick(Landroid/view/View;)V
# return null
```

### 优化删除小米应用商店组件后安装菜鸟裹裹调用失败的问题
代码位置： `com/miui/personalassistant/util/IntentUtil.smali`
```
# 将该类中的 "mimarket://details" 修改为 "market://details"
```

### 屏蔽更新
代码位置： `com/miui/personalassistant/util/DownloadHelper.smali`
```
.method private checkVersionUpgrade()Z
# return false
```
