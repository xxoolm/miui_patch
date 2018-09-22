## 天气
APK位置： `/system/priv-app/Weather/Weather.apk`

apktool命令： `apktool d -r *.apk`

### 移除主界面的广告
代码位置： `com/miui/weather2/tools/ToolUtils.smali`
```
.method public static canRequestCommercial(Landroid/content/Context;)Z
.method public static canRequestCommercialInfo(Landroid/content/Context;)Z
# return false
```

### 移除15天趋势预报的广告
代码位置： `com/miui/weather2/structures/DailyForecastAdData.smali`
```
.method public isAdInfosValid()Z
.method public isAdParamValid()Z
.method public isAdTitleValid()Z
.method public isLandingPageUrlValid()Z
.method public isUseSystemBrowserValid()Z
# return false
```

### 保留语音播报功能
代码位置： `com/miui/weather2/ActivityWeatherMain.smali`
```
# 搜索函数名包含 Lcom/miui/weather2/tools/ToolUtils;->canRequestCommercialInfo(Landroid/content/Context;)Z 所在的方法

# 将诸如以下的代码：
invoke-static {p0}, Lcom/miui/weather2/tools/ToolUtils;->canRequestCommercialInfo(Landroid/content/Context;)Z

move-result v0
# 修改为 sget-boolean v0, Lcom/winter/mysu;->TRUE:Z
```

### 移除信息流设置项
代码位置： `com/miui/weather2/ActivitySet.smali`
```
.method private initViews()V
# 在方法结束 return-void 前增加以下代码：
invoke-virtual {p0}, Lcom/miui/weather2/ActivitySet;->getPreferenceScreen()Landroid/preference/PreferenceScreen;

move-result-object v2

iget-object v3, p0, Lcom/miui/weather2/ActivitySet;->mOtherCategory:Landroid/preference/PreferenceCategory;

invoke-virtual {v2, v3}, Landroid/preference/PreferenceScreen;->removePreference(Landroid/preference/Preference;)Z
```
