## 浏览器
APK位置： `/system/priv-app/Browser/Browser.apk`

apktool命令： `apktool d -r *.apk`

说明：浏览器初始化资源位于 `assets` 目录，API相关代码：`api.browser.miui.com` 、`hot.browser.miui.com`

ps: 将 `assets` 目录预设的相关 `.json` 文件的内容置空，即修改为 `{}`

### 定位国际版相关代码
代码路径： `smali/miui/browser`
```
# 搜索 IS_INTERNATIONAL_BUILD 进行定位，可以发现诸如以下代码：
const-string v1, "IS_INTERNATIONAL_BUILD"

const-string v2, "Z"

invoke-static {v0, v1, v2}, Lmiui/support/reflect/Field;->of(Ljava/lang/Class;Ljava/lang/String;Ljava/lang/String;)Lmiui/support/reflect/Field;

move-result-object v1

const/4 v2, 0x0

invoke-virtual {v1, v2}, Lmiui/support/reflect/Field;->getBoolean(Ljava/lang/Object;)Z

move-result v1

sput-boolean v1, Lmiui/browser/f/a;->e:Z

# 记住上面的 Lmiui/browser/f/a;->e:Z
```

### 移除底栏的『资讯』、『视频』
代码路径： `com/android/browser`
```
# 搜索 pref_current_miui_home_position 、pref_hide_yellowpage_ 定位相关方法
# 将方法中的国际版代码（就是上面找到的 Lmiui/browser/f/a;->e:Z , 下同）修改为 Lcom/winter/mysu;->TRUE:Z
```
代码位置： `com/android/browser/toolbar/BottomBar.smali`
```
.method protected a(Landroid/util/AttributeSet;)V
    # 省略的代码

    const v0, 0x7f1000ba  # action_back 对应的 id （在 res/values/public.xml 中查找，下同）

    invoke-virtual {p0, v0}, Lcom/android/browser/toolbar/BottomBar;->findViewById(I)Landroid/view/View;

    move-result-object v0

    check-cast v0, Landroid/widget/ImageButton;

    # 增加代码开始
    const/4 v1, 0x0  # 0x0 表示设置为可见，下同

    invoke-virtual {v0, v1}, Landroid/widget/ImageButton;->setVisibility(I)V
    # 增加代码结束

    iput-object v0, p0, Lcom/android/browser/toolbar/BottomBar;->p:Landroid/widget/ImageButton;

    iget-object v0, p0, Lcom/android/browser/toolbar/BottomBar;->p:Landroid/widget/ImageButton;

    invoke-virtual {v0, p0}, Landroid/widget/ImageButton;->setOnClickListener(Landroid/view/View$OnClickListener;)V

    iget-object v0, p0, Lcom/android/browser/toolbar/BottomBar;->p:Landroid/widget/ImageButton;

    invoke-virtual {v0, v2}, Landroid/widget/ImageButton;->setEnabled(Z)V

    iget-object v0, p0, Lcom/android/browser/toolbar/BottomBar;->p:Landroid/widget/ImageButton;

    const v1, 0x7f02002e

    invoke-virtual {v0, v1}, Landroid/widget/ImageButton;->setImageResource(I)V

    const v0, 0x7f1000bb  # action_forward 对应的 id

    invoke-virtual {p0, v0}, Lcom/android/browser/toolbar/BottomBar;->findViewById(I)Landroid/view/View;

    move-result-object v0

    check-cast v0, Landroid/widget/ImageButton;

    # 增加代码开始
    const/4 v1, 0x0

    invoke-virtual {v0, v1}, Landroid/widget/ImageButton;->setVisibility(I)V
    # 增加代码结束

    iput-object v0, p0, Lcom/android/browser/toolbar/BottomBar;->q:Landroid/widget/ImageButton;

    iget-object v0, p0, Lcom/android/browser/toolbar/BottomBar;->q:Landroid/widget/ImageButton;

    invoke-virtual {v0, p0}, Landroid/widget/ImageButton;->setOnClickListener(Landroid/view/View$OnClickListener;)V

    iget-object v0, p0, Lcom/android/browser/toolbar/BottomBar;->q:Landroid/widget/ImageButton;

    invoke-virtual {v0, v2}, Landroid/widget/ImageButton;->setEnabled(Z)V

    const v0, 0x7f100004  # action_info 对应的 id

    invoke-virtual {p0, v0}, Lcom/android/browser/toolbar/BottomBar;->findViewById(I)Landroid/view/View;

    move-result-object v0

    check-cast v0, Lcom/android/browser/toolbar/ToolBarItem;

    iput-object v0, p0, Lcom/android/browser/toolbar/BottomBar;->x:Lcom/android/browser/toolbar/ToolBarItem;

    # 增加代码开始
    const/16 v1, 0x8  # 0x8 表示设置为隐藏且不占用空间，下同

    invoke-virtual {v0, v1}, Lcom/android/browser/toolbar/ToolBarItem;->setVisibility(I)V
    # 增加代码结束

    iget-object v0, p0, Lcom/android/browser/toolbar/BottomBar;->x:Lcom/android/browser/toolbar/ToolBarItem;

    invoke-virtual {v0, p0}, Lcom/android/browser/toolbar/ToolBarItem;->setOnClickListener(Landroid/view/View$OnClickListener;)V

    const v0, 0x7f10001e  # action_video 对应的 id

    invoke-virtual {p0, v0}, Lcom/android/browser/toolbar/BottomBar;->findViewById(I)Landroid/view/View;

    move-result-object v0

    check-cast v0, Lcom/android/browser/toolbar/ToolBarItem;

    iput-object v0, p0, Lcom/android/browser/toolbar/BottomBar;->y:Lcom/android/browser/toolbar/ToolBarItem;

    # 增加代码开始
    const/16 v1, 0x8

    invoke-virtual {v0, v1}, Lcom/android/browser/toolbar/ToolBarItem;->setVisibility(I)V
    # 增加代码结束

    iget-object v0, p0, Lcom/android/browser/toolbar/BottomBar;->y:Lcom/android/browser/toolbar/ToolBarItem;

    invoke-virtual {v0, p0}, Lcom/android/browser/toolbar/ToolBarItem;->setOnClickListener(Landroid/view/View$OnClickListener;)V

    # 省略的代码
.end method
```
紧接着在当前类的另一个方法中删除跟上面对应的 `ImageButton` 的所有 `setVisibility(I)V` 代码片段
```
p:Landroid/widget/ImageButton;  # action_back
q:Landroid/widget/ImageButton;  # action_forward
x:Landroid/widget/ImageButton;  # action_info
y:Landroid/widget/ImageButton;  # action_video
```
例如：
```
iget-object v0, p0, Lcom/android/browser/toolbar/BottomBar;->p:Landroid/widget/ImageButton;

invoke-virtual {v0, v2}, Landroid/widget/ImageButton;->setVisibility(I)V
```

### 阻止个人中心url重定向，锁定跳转到『我的小米』
代码路径： `com/android/browser`
```
# 搜索 pref_account_url 找到对应的无返回值的方法，return null
```

### 阻止『小说书架』url重定向，锁定为初始状态
代码路径： `com/android/browser`
```
# 搜索 pref_bookshelf_url 找到对应的无返回值的方法，return null
```

### 修改『我的视频』为国际版样式
代码路径： `com/android/browser/menu`
```
# 搜索 Lmiui/browser/video/MiuiVideoManagerActivity 定位对应方法
# 将方法中的国际版代码修改为 Lcom/winter/mysu;->TRUE:Z
```

### 移除搜索建议的广告推荐
代码位置： `com/android/browser/suggestion/MiuiSuggestionProvider.smali`
```
# 搜索 api.browser.miui.com/suggest/suglist 定位对应方法
# 对其上层调用 return null
```

### 移除下载APP时浏览器底部弹出的『猜你喜欢』
代码路径： `com/android/browser`
```
# 搜索 browser.extra.show_go_supermarket_link 找到诸如以下代码片段：
const-string v0, "browser.extra.show_go_supermarket_link"

invoke-virtual {p2, v0}, Landroid/content/Intent;->getStringExtra(Ljava/lang/String;)Ljava/lang/String;

move-result-object v0

iget-object v1, p0, Lcom/android/browser/aa$25;->a:Lcom/android/browser/aa;

invoke-static {v1}, Lcom/android/browser/aa;->h(Lcom/android/browser/aa;)Lmiui/browser/c/j;

move-result-object v1

new-instance v2, Lcom/android/browser/aa$25$2;

invoke-direct {v2, p0, v0}, Lcom/android/browser/aa$25$2;-><init>(Lcom/android/browser/aa$25;Ljava/lang/String;)V

# 定位到类 com/android/browser/aa$25$2 的 .method public run()V 方法，return null
```

### 拦截相关国内API
代码路径： `smali/miui/browser`
```
# 搜索 api.browser.miui.com 进行定位相应方法，可以发现诸如以下的代码：
const-string v1, "/bsr/update/"  # 相关API路径

invoke-virtual {v0, v1}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;

move-result-object v0

invoke-virtual {v0}, Ljava/lang/StringBuilder;->toString()Ljava/lang/String;

move-result-object v0

sput-object v0, Lmiui/browser/d/a$h;->c:Ljava/lang/String;
# 在 com/android/browser 路径下搜索 Lmiui/browser/d/a$h;->c:Ljava/lang/String;
# 逐级分析其调用链，可以找到诸如以下的代码：
.method public m(Landroid/content/Context;)Ljava/lang/String;
    # 省略的代码
    const-string v3, "get data from server: "
    # 省略的代码
.end method

# 在 com/android/browser 路径下 "get data from server: " 定位相关方法，将其函数体改成诸如以下的代码
.method public m(Landroid/content/Context;)Ljava/lang/String;
    .locals 1

    const-string v0, ""

    return-object v0
.end method

# 可通过抓包软件分析找到相关API路径，直接修改为一个不存在的路径，此时服务器会返回404（仍须消耗些许流量）
# 你也可以通过hosts进行屏蔽，这不是miui_patch研究的方式，故不再展开。
```

### 屏蔽浏览器更新
定位 API `/bsr/ota/` 执行相应修改（切断调用链），或者设置一个不存在的API，令服务器返回404

### 关闭消息通知及空格位推送
代码路径： `com/android/browser`
```
# 搜索以下关键词定位相关布尔型方法
enable_notifications
quicklink_push
# return false
```

### 移除『宫格位推送』设置项
代码位置： `com/android/browser/preferences/AdvancedPreferencesFragment.smali`
```
.method public onCreate(Landroid/os/Bundle;)V
# 将诸如以下代码：
const-string v0, "quicklink_push"

invoke-virtual {p0, v0}, Lcom/android/browser/preferences/AdvancedPreferencesFragment;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

move-result-object v0

check-cast v0, Landroid/preference/CheckBoxPreference;

invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

# 修改为：
const-string v1, "quicklink_push"

invoke-virtual {p0, v1}, Lcom/android/browser/preferences/AdvancedPreferencesFragment;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

move-result-object v1

invoke-virtual {v0, v1}, Landroid/preference/PreferenceGroup;->removePreference(Landroid/preference/Preference;)Z
```

### 移除『消息通知管理』设置树（可选）
代码位置： `com/android/browser/preferences/MainPreferenceFragment.smali`
```
.method public onCreate(Landroid/os/Bundle;)V
# 在方法结束 return-void 增加以下代码：
const-string v1, "accessibility_group"

invoke-virtual {p0, v1}, Lcom/android/browser/preferences/MainPreferenceFragment;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

move-result-object v1

check-cast v1, Landroid/preference/PreferenceGroup;

const-string v2, "pref_header_key_notifications"

invoke-virtual {p0, v2}, Lcom/android/browser/preferences/MainPreferenceFragment;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

move-result-object v2

invoke-virtual {v1, v2}, Landroid/preference/PreferenceGroup;->removePreference(Landroid/preference/Preference;)Z
```

### 移除『软件版本』的更新组件
代码位置： `com/android/browser/preferences/VersionPreferenceFragment.smali`
```
.method public onCreate(Landroid/os/Bundle;)V
# 在方法结束 return-void 前增加以下代码：
const-string v0, "version_check"

invoke-virtual {p0, v0}, Lcom/android/browser/preferences/VersionPreferenceFragment;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

move-result-object v0

check-cast v0, Landroid/preference/PreferenceGroup;

const-string v1, "pref_key_auto_update_under_wifi"

invoke-virtual {p0, v1}, Lcom/android/browser/preferences/VersionPreferenceFragment;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

move-result-object v1

invoke-virtual {v0, v1}, Landroid/preference/PreferenceGroup;->removePreference(Landroid/preference/Preference;)Z

const-string v1, "pref_key_check_new_version"

invoke-virtual {p0, v1}, Lcom/android/browser/preferences/VersionPreferenceFragment;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

move-result-object v1

invoke-virtual {v0, v1}, Landroid/preference/PreferenceGroup;->removePreference(Landroid/preference/Preference;)Z
```
