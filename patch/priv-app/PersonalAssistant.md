## 负一屏（信息助手）
APK位置： `/system/priv-app/PersonalAssistant/PersonalAssistant.apk`

apktool命令： `apktool d -r *.apk`

### 移除运动计步关联的小米VIP，使其点击无效
代码位置： `com/miui/home/launcher/assistant/ui/view/SportCardView.smali`
```
.method public onClick(Landroid/view/View;)V
# return null
```

### 修复删除小米应用商店组件后安装菜鸟裹裹导致的FC
代码位置： `com/miui/personalassistant/ui/fragment/ExpressSettingFragment.smali`
```
.method public onClick(Landroid/view/View;)V
# 方法1：直接切断其调用链
# 搜索 com.cainiao.wireless 可以找到诸如以下的代码：
:sswitch_2
const-string/jumbo v0, "com.cainiao.wireless"

invoke-static {v0}, Lcom/miui/personalassistant/util/IntentUtil;->intentToAppStore(Ljava/lang/String;)Landroid/content/Intent;

move-result-object v0

invoke-virtual {p0, v0}, Lcom/miui/personalassistant/ui/fragment/ExpressSettingFragment;->startActivity(Landroid/content/Intent;)V

goto :goto_0
# 删除 invoke-virtual {p0, v0}, Lcom/miui/personalassistant/ui/fragment/ExpressSettingFragment;->startActivity(Landroid/content/Intent;)V 这行代码

# 方法2：修改其调用行为，调用其它应用商店
# 将上述代码修改为：
:sswitch_2
const-string/jumbo v1, "com.cainiao.wireless"

invoke-static {v1}, Lcom/miui/personalassistant/util/IntentUtil;->intentToAppStore(Ljava/lang/String;)Landroid/content/Intent;

move-result-object v0

invoke-virtual {p0}, Lcom/miui/personalassistant/ui/fragment/ExpressSettingFragment;->getActivity()Landroid/app/Activity;

move-result-object v1

invoke-static {v1, v0}, Lcom/miui/personalassistant/util/IntentUtil;->canResolveIntent(Landroid/content/Context;Landroid/content/Intent;)Z

move-result v1

if-eqz v1, :goto_0

invoke-virtual {p0, v0}, Lcom/miui/personalassistant/ui/fragment/ExpressSettingFragment;->startActivity(Landroid/content/Intent;)V

goto :goto_0
```
代码位置： `com/miui/personalassistant/util/IntentUtil.smali`
```
# 将该类中的 "mimarket://details" 修改为 "market://details"
```
