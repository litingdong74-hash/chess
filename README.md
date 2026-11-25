# 中国象棋（安卓安装包）

把桌面的 `chess.html` 打包成了一个可以直接装到手机上的 App。

## 安装包

| 项目 | 内容 |
| --- | --- |
| 文件 | `Xiangqi-v1.0.apk` |
| 大小 | 270 KB |
| 应用名称 | 中国象棋 |
| 包名 | `com.dsh.xiangqi` |
| 版本 | 1.0 (versionCode 1) |
| 系统要求 | Android 5.0 及以上（minSdk 21，targetSdk 34） |
| 签名 | 自带自签名证书（v1 + v2 + v3），非应用商店版本 |

SHA-256：`5CFF8E40D92689E2540E69AF97BE1C19F9E2101806EF9693DD210D09C52AF85C`

## 手机上怎么装

1. 把 `Xiangqi-v1.0.apk` 传到手机（微信 / QQ / 数据线 / 网盘都行）。
2. 在手机的「文件管理」里点开这个 apk。**如果在微信里点开，先选“用其他应用打开”**，微信会拦截 apk 直装。
3. 系统提示“禁止安装未知来源应用”时，按提示允许「文件管理」或「浏览器」安装应用。
4. 安装完在桌面找「中国象棋」图标即可。首次打开若提示“未经检测的应用”，选“仍然安装”。

## 游戏功能

- 玩法与原来的 `chess.html` 完全一致：走法、将军、将死、飞将、长将判负等规则都在。
- **双人对战 / 人机·简单 / 人机·中等 / 人机·困难** 四档（AI 搜索深度 2/3/4 层）。
- 新局、悔棋（人机模式自动退两步）、翻转棋盘。
- 手机适配：棋盘按屏幕大小自动缩放，竖屏横屏都能用，点击落子带震动反馈和落子音效（右下角 🔊 可静音）。
- 按下返回键会先问“要退出游戏吗？”，不会一键掉线。

## 目录说明

```
chess/
├─ Xiangqi-v1.0.apk        安装包（发给手机的就是它）
├─ index.html              网页版，双击即可在浏览器里玩（也是 App 内嵌的页面）
├─ preview-phone.png       手机界面预览图
├─ app/                    Android 工程源码
│  ├─ AndroidManifest.xml
│  ├─ assets/index.html    打包进 App 的网页
│  ├─ res/                 图标、主题、字符串
│  └─ src/com/dsh/xiangqi/MainActivity.java   WebView 外壳
└─ _build/                 构建脚本（首次打包会自动下载 JDK 与 Android 工具链）
   ├─ build.ps1            一键出包脚本
   ├─ fetch.py / extract.py  下载、解压工具链
   ├─ gen_icons.py         生成各密度启动图标
   ├─ add_dex.py           把 classes.dex 合进 APK
   └─ xiangqi.keystore     签名密钥（务必保留，升级要沿用同一签名）
```

`_build` 里只有脚本和签名密钥，不到 100 KB。真正的 JDK / Android 工具链在第一次打包时才下载，用完会留在 `_build/jdk`、`_build/bt`、`_build/pf`（约 400 MB），**不需要的话可以直接删掉**，不影响已经生成的安装包和网页版；下次打包会重新下载。

## 想改代码后重新打包

改完 `index.html`（或 `app/` 里的原生代码）后运行：

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File D:\chess\_build\build.ps1
```

脚本会先检查工具链（缺了自动下载解压），然后编译资源、编译 Java、生成 dex、对齐并签名，最终在 `D:\chess\` 生成 `Xiangqi-v1.0.apk`。整个流程在实测中约 1 分钟（含首次下载）。
