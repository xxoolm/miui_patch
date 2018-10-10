说明：适用于小米系列机型MIUI V9.5.x稳定版（miui_patch不对红米系列机型单独适配）

```
.
└── system
    ├── app
    │   ├── AnalyticsCore  # 必删，广告组件
    │   │   └── AnalyticsCore.apk
    │   ├── BasicDreams  # 建议删，没卵用
    │   │   └── BasicDreams.apk
    │   ├── BookmarkProvider  # 浏览器关联删除，书签
    │   │   └── BookmarkProvider.apk
    │   ├── GameCenter  # 建议删，小米游戏中心
    │   │   └── GameCenter.apk
    │   ├── mab  # 必删，广告组件
    │   │   ├── lib
    │   │   │   └── arm
    │   │   └── mab.apk
    │   ├── MiuiSuperMarket # 建议删，小米应用商店
    │   │   └── MiuiSuperMarket.apk
    │   ├── PhotoTable # 建议删，没卵用
    │   │   └── PhotoTable.apk
    │   ├── SogouInput # 可删，搜狗输入法小米版
    │   │   └── SogouInput.apk
    │   ├── SystemAdSolution # 必删，广告组件
    │   │   └── SystemAdSolution.apk
    │   ├── Updater # 建议删，系统更新
    │   │   ├── lib
    │   │   │   └── arm64
    │   │   └── Updater.apk
    │   ├── VipAccount # 建议删，小米VIP
    │   │   └── VipAccount.apk
    │   ├── VirtualSim # 可删，米SIM
    │   │   └── VirtualSim.apk
    │   ├── VoiceAssist # 可删，语音助手（小爱同学）
    │   │   └── VoiceAssist.apk
    │   └── XMPass # 建议删，小米卡包
    │       ├── lib
    │       │   └── arm64
    │       └── XMPass.apk
    ├── framework # 以下项若存在则删除
    │   ├── miui.jar
    │   ├── miuisystem.jar
    │   └── miui-update.jar
    └── priv-app
        ├── Browser # 建议删，浏览器
        │   └── Browser.apk
        ├── MiGameCenterSDKService # 可删，小米游戏SDK服务
        │   └── MiGameCenterSDKService.apk
        ├── MiuiVideo # 建议删，小米视频，可替换为国际版
        │   └── MiuiVideo.apk
        ├── MiWebView # 浏览器关联删除，不知干嘛用的
        │   └── MiWebView.apk
        ├── Music # 建议删，小米音乐
        │   └── Music.apk
        └── QuickSearchBox # 建议删，MIUI搜索APP，可替换为『搜索Lite』
            └── QuickSearchBox.apk
```
