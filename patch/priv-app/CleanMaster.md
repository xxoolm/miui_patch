## 垃圾清理
APK位置： `/system/priv-app/CleanMaster/CleanMaster.apk`

apktool命令： `apktool d -r *.apk`

### 删除广告
代码位置： `com/miui/optimizecenter/config/ServerUrl.smali`
```
# 将该类中的 https://adv.sec.miui.com/info/layout 和 https://adv.sec.intl.miui.com/info/layout 全部修改为 http://127.0.0.1/
```

### 移除信息流设置项
代码位置： `com/miui/optimizecenter/settings/SettingsActivity.smali`
```
# 在 res/values/public.xml 中找到 preference_category_information_setting 对应的 id
# 在当前类定位该 id 所在的位置，可以发现诸如以下的代码：
const v6, 0x7f09002b  # preference_category_information_setting 对于的 id

invoke-virtual {p0, v6}, Lcom/miui/optimizecenter/settings/SettingsActivity;->getString(I)Ljava/lang/String;

move-result-object v6

invoke-virtual {p0, v6}, Lcom/miui/optimizecenter/settings/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

move-result-object v3

check-cast v3, Landroid/preference/PreferenceCategory;

# 在其后面增加以下语句：
invoke-virtual {p0}, Lcom/miui/optimizecenter/settings/SettingsActivity;->getPreferenceScreen()Landroid/preference/PreferenceScreen;

move-result-object v0

# v3 对应上面的 move-result-object v3
invoke-virtual {v0, v3}, Landroid/preference/PreferenceCategory;->removePreference(Landroid/preference/Preference;)Z
```
