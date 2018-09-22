## 设置
APK位置： `/system/priv-app/Settings/Settings.apk`

apktool命令： `apktool d -r *.apk`

### 优化恢复默认应用时浏览器显示包名 com.android.browser 的问题
代码位置： `com/android/settings/applications/PreferredListSettings$1.smali`
```
.method public onClick
# 搜索代码：com.android.browser
# 删除该行代码，同时删除下面invoke-virtual调用语句中包含该代码的变量，如v4
# 将 setDefaultBrowserPackageNameAsUser(Ljava/lang/String;I)Z 修改为 getDefaultBrowserPackageNameAsUser(I)Ljava/lang/String;
# 注：此处可能是 setDefaultBrowserPackageName(Ljava/lang/String;I)Z ，须修改为 getDefaultBrowserPackageName(I)Ljava/lang/String;
```

### 移除『我的设备』，恢复为『关于手机』
代码路径： `com/android/settings/device`
```
# 搜索代码 IS_GLOBAL_BUILD:Z ，该语句会在多个方法出现，选择例如下面的布尔值类型函数，并对该语句 return true (Lcom/winter/mysu;->TRUE:Z)
.method public static Ib(Landroid/content/Context;)Z
    .locals 1

    .prologue
    .line 231
    sget-boolean v0, Lmiui/os/Build;->IS_GLOBAL_BUILD:Z

    const/4 v0, 0x1

    if-eqz v0, :cond_0

    .line 232
    const/4 v0, 0x0

    return v0

    .line 234
    :cond_0
    invoke-static {}, Lcom/android/settings/device/a;->In()Z

    move-result v0

    return v0
.end method
```

### 移除『关于手机』的系统更新菜单
代码位置： `com/android/settings/device/MiuiDeviceInfoSettings.smali`
```
.method public onCreate(Landroid/os/Bundle;)V
# 搜索代码 miui_update 定位相关项，将诸如以下代码：
.line 152
const-string/jumbo v0, "miui_update"

invoke-virtual {p0, v0}, Lcom/android/settings/device/MiuiDeviceInfoSettings;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

move-result-object v0

check-cast v0, Lcom/android/settings/widget/CustomValuePreference;

iput-object v0, p0, Lcom/android/settings/device/MiuiDeviceInfoSettings;->aog:Lcom/android/settings/widget/CustomValuePreference;

.line 153
iget-boolean v0, p0, Lcom/android/settings/device/MiuiDeviceInfoSettings;->aoa:Z

if-eqz v0, :cond_0

.line 154
invoke-virtual {p0}, Lcom/android/settings/device/MiuiDeviceInfoSettings;->getActivity()Landroid/app/Activity;

move-result-object v0

invoke-static {v0}, Lcom/android/settings/device/a;->abV(Landroid/content/Context;)Ljava/lang/String;

move-result-object v0

.line 155
invoke-static {v0}, Landroid/text/TextUtils;->isEmpty(Ljava/lang/CharSequence;)Z

move-result v0

if-nez v0, :cond_0

.line 156
iget-object v0, p0, Lcom/android/settings/device/MiuiDeviceInfoSettings;->aog:Lcom/android/settings/widget/CustomValuePreference;

const/4 v3, 0x2

invoke-virtual {v0, v3}, Lcom/android/settings/widget/CustomValuePreference;->ajN(I)V

# 修改为：
.line 152
const-string/jumbo v0, "miui_update"

invoke-virtual {p0, v0}, Lcom/android/settings/device/MiuiDeviceInfoSettings;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

move-result-object v0

check-cast v0, Lcom/android/settings/widget/CustomValuePreference;

.line 153
invoke-virtual {p0}, Lcom/android/settings/device/MiuiDeviceInfoSettings;->getPreferenceScreen()Landroid/preference/PreferenceScreen;

move-result-object v3

# v0 对应上面的 move-result-object v0
invoke-virtual {v3, v0}, Landroid/preference/PreferenceScreen;->removePreference(Landroid/preference/Preference;)Z
```

### 恢复『更多应用』原来的展示样式
关键代码： `com.miui.appmanager.AppManagerMainActivity`

代码位置： `com/android/settings/applications/ApplicationsContainer.smali`
```
.method public onActivityCreated
# 搜索 Lmiui/os/Build;->IS_TABLET:Z 将其改成 Lcom/winter/mysu;->TRUE:Z
```
代码位置： `com/android/settings/applications/ManageApplicationsActivity.smali`
```
.method protected onCreate
# 搜索 Lmiui/os/Build;->IS_TABLET:Z 将其改成 Lcom/winter/mysu;->TRUE:Z
```
代码位置： `com/android/settings/search/tree/ApplicationsSettingsTree.smali`
```
.method public getIntent
# 搜索 Lmiui/os/Build;->IS_TABLET:Z 将其改成 Lcom/winter/mysu;->TRUE:Z
```
代码位置： `com/android/settings/MiuiSettings.smali`
```
.method public onBuildStartFragmentIntent
# 将以下代码：
const-string/jumbo v1, "com.miui.securitycenter"

const-string/jumbo v2, "com.miui.appmanager.AppManagerMainActivity"

invoke-virtual {v0, v1, v2}, Landroid/content/Intent;->setClassName(Ljava/lang/String;Ljava/lang/String;)Landroid/content/Intent;

# 修改为：
const-class v1, Lcom/android/settings/applications/ManageApplicationsActivity;

invoke-virtual {v0, p0, v1}, Landroid/content/Intent;->setClass(Landroid/content/Context;Ljava/lang/Class;)Landroid/content/Intent;
```
