## 垃圾清理
APK位置： `/system/priv-app/CleanMaster/CleanMaster.apk`

apktool命令： `apktool d -r *.apk`

### 移除广告
代码位置： `com/miui/optimizecenter/information/DataModel.smali`
```
.method public static post(Ljava/util/Map;)Ljava/lang/String;
# 对诸如以下的代码执行修改：
const v6, 0x7f08002a    # preference_key_information_setting_wlan 对应的 id（在 res/values/public.xml 中查看，下同）

invoke-virtual {v0, v6}, Lcom/miui/optimizecenter/Application;->getString(I)Ljava/lang/String;

move-result-object v6

const/4 v7, 0x0

invoke-interface {v5, v6, v7}, Landroid/content/SharedPreferences;->getBoolean(Ljava/lang/String;Z)Z

move-result v4    # 修改为 sget-boolean v4, Lcom/winter/mysu;->TRUE:Z

const v6, 0x7f08002b    # preference_key_information_setting_close 对应的 id

invoke-virtual {v0, v6}, Lcom/miui/optimizecenter/Application;->getString(I)Ljava/lang/String;

move-result-object v6

const/4 v7, 0x1

invoke-interface {v5, v6, v7}, Landroid/content/SharedPreferences;->getBoolean(Ljava/lang/String;Z)Z

move-result v1    # 修改为 sget-boolean v1, Lcom/winter/mysu;->FALSE:Z

# 在路径 com/miui/optimizecenter 下搜索与之类似的实例代码，并执行同样的修改
```

### 移除信息流设置项
代码位置： `com/miui/optimizecenter/settings/SettingsActivity.smali`
```
.method public onCreate
# 在 res/values/public.xml 中找到 preference_category_information_setting 对应的 id
# 在当前类定位该 id 所在的位置，可以发现诸如以下的代码：
invoke-virtual {p0}, Lcom/miui/optimizecenter/settings/SettingsActivity;->getPreferenceScreen()Landroid/preference/PreferenceScreen;

move-result-object v4

const v6, 0x7f080029

invoke-virtual {p0, v6}, Lcom/miui/optimizecenter/settings/SettingsActivity;->getString(I)Ljava/lang/String;

move-result-object v6

invoke-virtual {p0, v6}, Lcom/miui/optimizecenter/settings/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

move-result-object v2

check-cast v2, Landroid/preference/PreferenceCategory;

# 在其后面增加一行代码，其中 v4, v2 对应上面的相关变量
invoke-virtual {v4, v2}, Landroid/preference/PreferenceCategory;->removePreference(Landroid/preference/Preference;)Z
```
