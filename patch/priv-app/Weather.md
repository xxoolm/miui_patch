## 天气
APK位置： `/system/priv-app/Weather/Weather.apk`

apktool命令： `apktool d -r *.apk`

### 移除主界面的广告
代码路径： `com/miui/weather2/tools`
```
# 搜索代码 Lmiui/os/Build;->IS_TABLET:Z 找到函数名包含 (Landroid/content/Context;)Z 的方法
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
# 搜索函数名包含 (Landroid/widget/ListView;Lcom/miui/weather2/structures/CityData;Z)V 的方法
# 在该方法中可以找到诸如以下的代码片段：
new-instance v1, Ljava/util/ArrayList;

invoke-direct {v1}, Ljava/util/ArrayList;-><init>()V

if-eqz p2, :cond_0

invoke-virtual {p2}, Lcom/miui/weather2/structures/CityData;->getWeatherData()Lcom/miui/weather2/structures/WeatherData;

move-result-object v0

:cond_0
# Lcom/miui/weather2/tools/bj;->T(Landroid/content/Context;)Z 为上面我们处理过的方法
invoke-static {p0}, Lcom/miui/weather2/tools/bj;->T(Landroid/content/Context;)Z

move-result v2

# 将代码：
invoke-static {p0}, Lcom/miui/weather2/tools/bj;->T(Landroid/content/Context;)Z

move-result v2
# 修改为 sget-boolean v2, Lcom/winter/mysu;->TRUE:Z
```

### 移除信息流设置项
代码位置： `com/miui/weather2/ActivitySet.smali`
```
# 搜索代码 key_other_settings_category 定位相应方法，可以找到诸如以下的代码片段：
const-string v0, "key_other_settings_category"

invoke-virtual {p0, v0}, Lcom/miui/weather2/ActivitySet;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

move-result-object v0

check-cast v0, Landroid/preference/PreferenceCategory;

iput-object v0, p0, Lcom/miui/weather2/ActivitySet;->q:Landroid/preference/PreferenceCategory;

# 在方法结束 return-void 前增加 诸如以下代码：
invoke-virtual {p0}, Lcom/miui/weather2/ActivitySet;->getPreferenceScreen()Landroid/preference/PreferenceScreen;

move-result-object v0

# 这里的 ->q:Landroid/preference/PreferenceCategory; 就对应上面我们找到的
iget-object v1, p0, Lcom/miui/weather2/ActivitySet;->q:Landroid/preference/PreferenceCategory;

invoke-virtual {v0, v1}, Landroid/preference/PreferenceScreen;->removePreference(Landroid/preference/Preference;)Z
```
