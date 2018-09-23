## framework.jar
JAR位置： `/system/framework/framework.jar`

### 优化文字选择菜单搜索按钮的点击行为
代码位置： `android/widget/Editor$ActionPinnedPopupWindow.smali`
```
.method public onClick(Landroid/view/View;)V
# 关键代码：android.intent.action.WEB_SEARCH
# 将其上方的 Lmiui/os/Build;->IS_INTERNATIONAL_BUILD:Z 修改为 Lcom/winter/mysu;->TRUE:Z
# 如果你删除了MIUI搜索APP，必须执行此修改，否则点击会FC
```
