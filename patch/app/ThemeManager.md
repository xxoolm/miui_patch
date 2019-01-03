## 主题
APK位置： `/system/app/ThemeManager/ThemeManager.apk`

apktool命令： `apktool d -r *.apk`

### 删除首页的banner推荐
代码路径： `com/android/thememanager`
```
.method public getAdMarker()I
# return false

# 在上面方法对应的类中有一个布尔方法，通过 ->getImgUrls()Ljava/util/List; 定位
# return false
# 函数原型： .method private static isSupported
```
代码路径： `com/android/thememanager/view`
```
# 在 res/values/public.xml 中找到 ad_banner_view 对应的 id
# 在当前路径搜索该id所在的方法，并在当前类搜索调用该方法的函数，return null
```
代码路径： `com/android/thememanager/v9`
```
# 在 res/values/public.xml 中找到 element_ad_banner 对应的id
# 在当前路径搜索该id定位相关方法，可以找到诸如以下的代码，删除相关代码段
# 删除前：
:pswitch_1
new-instance v0, Lcom/android/thememanager/v9/d/f;

iget-object v2, p0, Lcom/android/thememanager/v9/a/d;->at:Lcom/android/thememanager/activity/m;

const v3, 0x7f03000d  # element_ad_banner 对应的id

invoke-virtual {v1, v3, p1, v6}, Landroid/view/LayoutInflater;->inflate(ILandroid/view/ViewGroup;Z)Landroid/view/View;

move-result-object v3

iget-object v4, p0, Lcom/android/thememanager/v9/a/d;->az:Lcom/android/thememanager/v9/b/a;

invoke-direct {v0, v2, v3, v4}, Lcom/android/thememanager/v9/d/f;-><init>(Landroid/app/Fragment;Landroid/view/View;Lcom/android/thememanager/v9/b/a;)V

goto :goto_0

# 删除后：
:pswitch_1
goto :goto_0
```

### 删除在线主题详情页的广告
代码路径： `com/android/thememanager/v9`
```
# 在当前路径搜索 onAdLoadSucceeded 定位相关方法
.method public onAdLoadSucceeded
# 修改该方法的函数体与 .method public onAdLoadFailed 方法一致
```
