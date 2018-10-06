## 状态栏（系统用户界面）
APK位置： `/system/priv-app/MiuiSystemUI/MiuiSystemUI.apk`

apktool命令： `apktool d -r *.apk`

## 优化搜索按钮的点击行为
代码位置： `com/android/systemui/statusbar/HeaderView.smali`
```
.method private buildShortcutClickIntent()Landroid/content/Intent;
# 将代码 Lmiui/os/Build;->IS_INTERNATIONAL_BUILD:Z 修改为 sget-boolean v1, Lcom/winter/mysu;->TRUE:Z
# 此处修改是避免卸载 MIUI搜索APP 导致的点击异常

# 修改后点击会调用支持WEB搜索的应用，默认是 MIUI 浏览器（若存在），你可以修改此行为，删除诸如以下代码即可：
new-instance v0, Landroid/content/Intent;

.end local v0    # "intent":Landroid/content/Intent;
const-string/jumbo v1, "com.android.browser.browser_search"

invoke-direct {v0, v1}, Landroid/content/Intent;-><init>(Ljava/lang/String;)V

.line 268
.local v0, "intent":Landroid/content/Intent;
const-string/jumbo v1, "com.android.browser"

invoke-virtual {v0, v1}, Landroid/content/Intent;->setPackage(Ljava/lang/String;)Landroid/content/Intent;

.line 269
invoke-virtual {p0}, Lcom/android/systemui/statusbar/HeaderView;->getContext()Landroid/content/Context;

move-result-object v1

invoke-static {v1, v0}, Lcom/android/systemui/Util;->isIntentActivityExist(Landroid/content/Context;Landroid/content/Intent;)Z

move-result v1

if-nez v1, :cond_0
```

### 使用国际版通知样式
代码位置： `com/android/systemui/Util.smali`
```
.method public static isInternationalBuild()Z
# return true
```

### 移除状态栏数字电量显示的百分号
代码位置： `com/android/systemui/BatteryMeterView.smali`
```
.method private updatePercentText()V
# 在语句 invoke-virtual {v0, v1}, Landroid/widget/TextView;->setText(Ljava/lang/CharSequence;)V 前增加诸如以下代码：
const/4 v2, 0x0

invoke-virtual {v1}, Ljava/lang/String;->length()I

move-result v3

add-int/lit8 v3, v3, -0x1

invoke-virtual {v1, v2, v3}, Ljava/lang/String;->substring(II)Ljava/lang/String;

move-result-object v1

# 上面代码的作用是去除数值最后面的百分号
```
