## 下载管理
APK位置： `/system/priv-app/DownloadProviderUi/DownloadProviderUi.apk`

apktool命令： `apktool d -r *.apk`

### 移除下载热榜
代码位置： `com/android/providers/downloads/ui/config/AppConfig.smali`
```
.method public static isShowRank()Z
# return false
```

### 移除资源推荐
代码位置： `com/android/providers/downloads/ui/utils/PreferenceUtil.smali`
```
.method public isShowInfoFlow()Z
.method public isShowRecommend()Z
.method public isShowVideoEnter()Z
# return false
```

### 移除信息流设置项
代码位置： `com/android/providers/downloads/ui/fragment/DownloadSettingFragment.smali`
```
.method private initInfoFlowPref()V
# 搜索代码 isCmTestBuilder()Z 并对其 return true，调用 removePreference 方法
```

修改后：
```
.method private initInfoFlowPref()V
    .locals 2

    .prologue
    .line 266
    const-string/jumbo v0, "pref_info_flow"

    invoke-virtual {p0, v0}, Lcom/android/providers/downloads/ui/fragment/DownloadSettingFragment;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    iput-object v0, p0, Lcom/android/providers/downloads/ui/fragment/DownloadSettingFragment;->mInfoFlowPref:Landroid/preference/Preference;

    .line 273
    invoke-virtual {p0}, Lcom/android/providers/downloads/ui/fragment/DownloadSettingFragment;->getPreferenceScreen()Landroid/preference/PreferenceScreen;

    move-result-object v0

    iget-object v1, p0, Lcom/android/providers/downloads/ui/fragment/DownloadSettingFragment;->mInfoFlowPref:Landroid/preference/Preference;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceScreen;->removePreference(Landroid/preference/Preference;)Z

    .line 265
    return-void
.end method
```

### 移除WLAN测速入口
代码位置： `com/android/providers/downloads/ui/DownloadList.smali`
```
.method public onOptionsItemSelected
.method public onPrepareOptionsMenu
# 反编译 res，在 values/public.xml 找到 speed_entrance 对应的id，并在这两个方法中删除与之有关的代码段
# 以 .method public onOptionsItemSelected 为例
# 删除前：
.method public onOptionsItemSelected(Landroid/view/MenuItem;)Z
    .locals 5
    .param p1, "item"    # Landroid/view/MenuItem;

    .prologue
    .line 414
    invoke-super {p0, p1}, Lcom/android/providers/downloads/ui/activity/BaseActivity;->onOptionsItemSelected(Landroid/view/MenuItem;)Z

    .line 415
    const/4 v0, 0x1

    .line 416
    .local v0, "handle":Z
    invoke-interface {p1}, Landroid/view/MenuItem;->getItemId()I

    move-result v2

    sparse-switch v2, :sswitch_data_0

    .line 448
    const/4 v0, 0x0

    .line 451
    :cond_0
    :goto_0
    return v0

    .line 418
    :sswitch_0
    invoke-static {p0}, Lcom/android/providers/downloads/ui/utils/XLUtil;->isNetworkAvailable(Landroid/content/Context;)Z

    move-result v2

    if-eqz v2, :cond_1

    .line 419
    invoke-virtual {p0}, Lcom/android/providers/downloads/ui/DownloadList;->resumeAllTask()V

    goto :goto_0

    .line 421
    :cond_1
    iget-object v2, p0, Lcom/android/providers/downloads/ui/DownloadList;->mContext:Landroid/content/Context;

    const v3, 0x7f09006a

    const/4 v4, 0x0

    invoke-static {v2, v3, v4}, Landroid/widget/Toast;->makeText(Landroid/content/Context;II)Landroid/widget/Toast;

    move-result-object v2

    invoke-virtual {v2}, Landroid/widget/Toast;->show()V

    goto :goto_0

    .line 425
    :sswitch_1
    invoke-virtual {p0}, Lcom/android/providers/downloads/ui/DownloadList;->pauseAllTask()V

    goto :goto_0

    .line 428
    :sswitch_2
    invoke-virtual {p0}, Lcom/android/providers/downloads/ui/DownloadList;->startNewTaskActivity()V

    goto :goto_0

    .line 431
    :sswitch_3
    iget-boolean v2, p0, Lcom/android/providers/downloads/ui/DownloadList;->shouldDisabled:Z

    if-nez v2, :cond_0

    .line 432
    new-instance v1, Landroid/content/Intent;

    const-class v2, Lcom/android/providers/downloads/ui/activity/DownloadSettingActivity;

    invoke-direct {v1, p0, v2}, Landroid/content/Intent;-><init>(Landroid/content/Context;Ljava/lang/Class;)V

    .line 433
    .local v1, "intent":Landroid/content/Intent;
    invoke-virtual {p0, v1}, Lcom/android/providers/downloads/ui/DownloadList;->startActivity(Landroid/content/Intent;)V

    goto :goto_0

    .line 445
    .end local v1    # "intent":Landroid/content/Intent;
    :sswitch_4
    invoke-direct {p0}, Lcom/android/providers/downloads/ui/DownloadList;->startNetTestSpeedPage()V

    goto :goto_0

    .line 416
    :sswitch_data_0
    .sparse-switch
        0x7f090041 -> :sswitch_0
        0x7f090042 -> :sswitch_1
        0x7f090043 -> :sswitch_3
        0x7f0900b9 -> :sswitch_2
        0x7f0901d9 -> :sswitch_4  # 0x7f0901d9 为 speed_entrance 对应的id
    .end sparse-switch
.end method

# 删除后：
.method public onOptionsItemSelected(Landroid/view/MenuItem;)Z
    .locals 5
    .param p1, "item"    # Landroid/view/MenuItem;

    .prologue
    .line 414
    invoke-super {p0, p1}, Lcom/android/providers/downloads/ui/activity/BaseActivity;->onOptionsItemSelected(Landroid/view/MenuItem;)Z

    .line 415
    const/4 v0, 0x1

    .line 416
    .local v0, "handle":Z
    invoke-interface {p1}, Landroid/view/MenuItem;->getItemId()I

    move-result v2

    sparse-switch v2, :sswitch_data_0

    .line 448
    const/4 v0, 0x0

    .line 451
    :cond_0
    :goto_0
    return v0

    .line 418
    :sswitch_0
    invoke-static {p0}, Lcom/android/providers/downloads/ui/utils/XLUtil;->isNetworkAvailable(Landroid/content/Context;)Z

    move-result v2

    if-eqz v2, :cond_1

    .line 419
    invoke-virtual {p0}, Lcom/android/providers/downloads/ui/DownloadList;->resumeAllTask()V

    goto :goto_0

    .line 421
    :cond_1
    iget-object v2, p0, Lcom/android/providers/downloads/ui/DownloadList;->mContext:Landroid/content/Context;

    const v3, 0x7f09006a

    const/4 v4, 0x0

    invoke-static {v2, v3, v4}, Landroid/widget/Toast;->makeText(Landroid/content/Context;II)Landroid/widget/Toast;

    move-result-object v2

    invoke-virtual {v2}, Landroid/widget/Toast;->show()V

    goto :goto_0

    .line 425
    :sswitch_1
    invoke-virtual {p0}, Lcom/android/providers/downloads/ui/DownloadList;->pauseAllTask()V

    goto :goto_0

    .line 428
    :sswitch_2
    invoke-virtual {p0}, Lcom/android/providers/downloads/ui/DownloadList;->startNewTaskActivity()V

    goto :goto_0

    .line 431
    :sswitch_3
    iget-boolean v2, p0, Lcom/android/providers/downloads/ui/DownloadList;->shouldDisabled:Z

    if-nez v2, :cond_0

    .line 432
    new-instance v1, Landroid/content/Intent;

    const-class v2, Lcom/android/providers/downloads/ui/activity/DownloadSettingActivity;

    invoke-direct {v1, p0, v2}, Landroid/content/Intent;-><init>(Landroid/content/Context;Ljava/lang/Class;)V

    .line 433
    .local v1, "intent":Landroid/content/Intent;
    invoke-virtual {p0, v1}, Lcom/android/providers/downloads/ui/DownloadList;->startActivity(Landroid/content/Intent;)V

    goto :goto_0

    .line 416
    :sswitch_data_0
    .sparse-switch
        0x7f090041 -> :sswitch_0
        0x7f090042 -> :sswitch_1
        0x7f090043 -> :sswitch_3
        0x7f0900b9 -> :sswitch_2
    .end sparse-switch
.end method
```
