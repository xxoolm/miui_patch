## 传送门
APK位置： `/system/priv-app/ContentExtension/ContentExtension.apk`

apktool命令： `apktool d -r *.apk`

### 优化搜索按钮的点击行为
备注： 如果你删除了MIUI搜索APP，请执行此修改，否则点击搜索按钮会FC

代码位置： `com/miui/contentextension/utils/Utilities.smali`
```
.method public static final searchPlainText(Landroid/content/Context;Landroid/content/ComponentName;Ljava/lang/String;)V
# 直接修改该方法的函数体，调用支持WEB搜索的应用进行处理，如：
.method public static final searchPlainText(Landroid/content/Context;Landroid/content/ComponentName;Ljava/lang/String;)V
    .locals 2

    new-instance v0, Landroid/content/Intent;

    const-string v1, "android.intent.action.WEB_SEARCH"

    invoke-direct {v0, v1}, Landroid/content/Intent;-><init>(Ljava/lang/String;)V

    const-string v1, "query"

    invoke-virtual {v0, v1, p2}, Landroid/content/Intent;->putExtra(Ljava/lang/String;Ljava/lang/String;)Landroid/content/Intent;

    invoke-virtual {p0}, Landroid/content/Context;->getPackageManager()Landroid/content/pm/PackageManager;

    move-result-object v1

    invoke-virtual {v0, v1}, Landroid/content/Intent;->resolveActivity(Landroid/content/pm/PackageManager;)Landroid/content/ComponentName;

    move-result-object v1

    if-eqz v1, :cond_0

    invoke-virtual {p0, v0}, Landroid/content/Context;->startActivity(Landroid/content/Intent;)V

    :goto_0
    return-void

    :cond_0
    const-string v0, "App unavailable, please check browser settings."

    const/4 v1, 0x1

    invoke-static {p0, v0, v1}, Landroid/widget/Toast;->makeText(Landroid/content/Context;Ljava/lang/CharSequence;I)Landroid/widget/Toast;

    move-result-object v0

    invoke-virtual {v0}, Landroid/widget/Toast;->show()V

    goto :goto_0
.end method
```
