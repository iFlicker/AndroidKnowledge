# Android Version Change Log

> 状态：已从 `/Users/lizhiyang/Desktop/AndroidDocs/文档/Android-版本索引.md` 合并主干内容，并按 Android Developers 官方版本页补充早期版本与 Android 17 Beta 信息。最后核对日期：2026-06-14。

## 写作目标

本文件作为 Android 版本演进的独立入口，按版本维度串联平台变化。它不替代 `docs/5.Android` 下的专题文章，而是用于快速确认某个版本在权限、后台、存储、UI、安全、运行时、API 和迁移策略上的关键差异。

阅读时建议固定看 4 个问题：

1. 权限 / 隐私有没有收紧。
2. 后台执行、组件启动、系统行为有没有变化。
3. UI / 大屏 / 导航 / 交互有没有新约束。
4. 新 API 对应到项目里该落在哪一层。

## 版本速查表

| Android 版本 | API Level | 面试 / 适配关键词 |
| --- | --- | --- |
| Android 17 Beta | 37 | 内存限制、MessageQueue 实现变化、ECH、OTP 保护、本地网络权限、证书透明度 |
| Android 16 | 36 | 16 KB 页大小兼容、预测返回默认化、edge-to-edge 收紧、进度通知、广播优先级范围变化 |
| Android 15 | 35 | Private Space、16 KB 页大小、edge-to-edge、app archiving、前台服务限制继续增强 |
| Android 14 | 34 | 前台服务类型、部分照片视频访问、Credential Manager、后台启动限制 |
| Android 13 | 33 | 通知权限、媒体权限拆分、系统照片选择器、Nearby Wi-Fi |
| Android 12L | 32 | 大屏、Taskbar、Split Screen、Activity Embedding |
| Android 12 | 31 | SplashScreen、Material You、蓝牙权限、隐私面板、`android:exported` |
| Android 11 | 30 | 单次授权、权限自动重置、包可见性、强制分区存储 |
| Android 10 | 29 | Scoped Storage、深色主题、手势导航、隐私和位置限制 |
| Android 9 | 28 | Display Cutout、非 SDK 限制、通知回复、多摄 |
| Android 8.0 / 8.1 | 26 / 27 | 后台执行限制、通知渠道、PIP、Autofill、Treble |
| Android 7.0 / 7.1 | 24 / 25 | 多窗口、FileUriExposed、Direct Reply、快捷方式 |
| Android 6.0 | 23 | 运行时权限、Doze、App Links、指纹 |
| Android 5.0 / 5.1 | 21 / 22 | Material Design、ART 默认、JobScheduler、Camera2 |
| Android 4.4 | 19 / 20 | Project Svelte、低内存优化、沉浸式、HCE、打印框架、ART 预览 |
| Android 4.1 / 4.2 / 4.3 | 16 / 17 / 18 | Project Butter、多用户、Daydream、受限资料、BLE、OpenGL ES 3.0 |
| Android 4.0 | 14 / 15 | Holo、手机平板 UI 统一、硬件加速、Face Unlock、Android Beam |
| Android 3.x | 11 / 12 / 13 | 平板 UI、Fragment、ActionBar、Loader、RenderScript |
| Android 2.3 | 9 / 10 | NFC、SIP、陀螺仪、多摄、游戏输入、NDK 增强 |
| Android 2.2 | 8 | JIT、Cloud to Device Messaging、App to SD、性能优化 |
| Android 2.0 / 2.1 | 5 / 6 / 7 | 多账号、Exchange、Bluetooth 2.1、Live Wallpaper、HTML5 |
| Android 1.6 | 4 | 多屏幕密度、Quick Search Box、CDMA、文本转语音 |
| Android 1.5 | 3 | 软键盘、AppWidget、Live Folders、视频录制 |
| Android 1.1 | 2 | 早期 API 修正、地图/通话/消息细节增强 |
| Android 1.0 | 1 | 首个商业版本、四大组件雏形、Market、浏览器、相机 |

## 版本索引

### Android 17 Beta（API 37）

- 版本定位：截至 2026-06-14，Android 17 仍是 Beta 阶段，官方首页显示 Beta 4 已可用于开发、测试和反馈；本节只作为预适配清单，不应当写成稳定版结论。
- 机制变化：
  - 系统引入按设备 RAM 设定的应用内存限制，并会更严格执行运行时内存上限；极端泄漏或异常内存占用可能直接导致进程退出。
  - 面向 target 37 的应用启用新的 lock-free `MessageQueue` 实现，反射私有字段或依赖内部锁行为的代码风险很高。
  - `static final` 字段不再允许通过反射或 JNI 修改。
  - 隐私与网络继续收紧：本地网络访问要求合适权限，ECH（Encrypted Client Hello）进入平台网络能力，OTP 短信访问被延迟/限制。
  - 安全方向包括默认证书透明度计划、跨 profile loopback 流量限制、URI grant 收紧、per-app keystore 数量限制。
- API 变化：
  - 新增 `JobDebugInfo`、ProfilingManager 触发器等调试/性能诊断能力。
  - 新增联系人选择器、CJKV IME 可访问性反馈相关 API。
  - APK 签名开始引入后量子密码（PQC）方向能力。
- 兼容变化：
  - 依赖 `MessageQueue` 私有实现、反射改常量、读取短信 OTP、本地局域网发现、HTTP 明文开关的旧实现都要重新验证。
  - Contacts Provider 查询、证书透明度、ECH、动态代码加载安全检查可能影响登录、通讯录、网络库和插件化模块。
- 迁移建议：
  - 在 Android 17 Beta 模拟器 / Pixel 设备上跑启动、页面切换、Handler、协程主线程调度、通知自定义 View、短信登录、局域网发现、HTTPS 握手回归。
  - 关闭对 `MessageQueue`、`static final`、ContactsProvider 内部行为的隐式依赖。
  - 网络层逐步改用 Network Security Configuration 管理明文和 ECH 策略。
- 官方资料：
  - [Android 17 Beta](https://developer.android.com/about/versions/17)
  - [Android 17 features and changes list](https://developer.android.com/about/versions/17/summary)
  - [Behavior changes: all apps](https://developer.android.com/about/versions/17/behavior-changes-all)
  - [Behavior changes: Apps targeting Android 17 or higher](https://developer.android.com/about/versions/17/behavior-changes-17)

### Android 16（API 36）

- 版本定位：Android 16 延续 Android 15 对系统一致性、窗口适配、安全默认值和性能调度的收紧。适配重点不只是新增 API，而是旧兼容写法是否还能继续成立。
- 机制变化：
  - 16 KB memory page 支持继续推进，新增兼容模式，部分按 4 KB 页假设构建的应用可以在 16 KB 设备上运行，但 Native 库仍应重新构建并测试。
  - 预测返回进一步默认化。target 36 后 `android:enableOnBackInvokedCallback` 默认变为 `true`，旧 `OnBackPressed` / `KEYCODE_BACK` 路径会被系统动画模型替代。
  - edge-to-edge 退出口减少，`windowOptOutEdgeToEdgeEnforcement` 被移除，应用必须正确处理 `WindowInsets`。
  - ordered broadcast 的优先级不再保证跨进程全局顺序，只在同一应用进程内更可靠。
  - JobScheduler 会根据 standby bucket、top state、是否伴随前台服务等因素调整普通/加急任务配额。
  - Intent 重定向、MediaStore 版本、GPU syscall、Health Connect 权限、本地网络权限继续收紧。
- API 变化：
  - Progress-centric notifications 面向用户主动发起、有明确开始和结束的任务，系统会给更高可见性。
  - 新增 richer haptics、live wallpaper content API、RangingManager、Health Connect 医疗记录早期能力、ApplicationStartInfo 启动组件信息。
  - 2025 年采用一个 Q2 major SDK release 加 Q4 minor SDK release 的节奏。
- 兼容变化：
  - 返回逻辑、Insets、Native so、后台任务、广播顺序、Intent 跳转安全都要纳入 target 36 升级清单。
- 迁移建议：
  - 全量检查返回链路是否迁移到 `OnBackInvokedCallback` / predictive back。
  - 全量检查沉浸式页面、底部导航、输入法弹起、视频全屏、Dialog / Popup / Toast 等 Insets 场景。
  - 含 Native 代码的应用跑 16 KB 页大小设备或模拟环境，验证 so 加载、mmap、崩溃和性能。
  - 下载、上传、导航、同步等长任务评估 progress-centric notification。
- 关联文档：
  - [Window 与输入事件](docs/5.Android/5.2.进阶/5.2.4.Window与输入事件.md)
  - [NDK 与 Native](docs/5.Android/5.2.进阶/5.2.6.NDK与Native.md)
  - [WorkManager](docs/5.Android/5.5.架构与工程化/5.5.2.Jetpack架构组件/5.5.2.3.WorkManager.md)
- 官方资料：
  - [Android 16 features and changes list](https://developer.android.com/about/versions/16/summary)
  - [API diff: API 35 -> API 36](https://developer.android.com/sdk/api_diff/36/changes/packages_index)

### Android 15（API 35）

- 版本定位：Android 15 把大屏、一致性 UI、隐私分区、前台服务和系统行为约束继续推进，对“看似能跑的旧代码”提出更多边缘条件要求。
- 机制变化：
  - 支持 16 KB 页大小，含 JNI、第三方 so、内存对齐敏感库影响最大。
  - Private Space 允许用户把敏感应用放进独立空间，影响应用可见性、跨应用发现和工作流协作。
  - Edge-to-edge enforcement 强化，target 35 后不正确处理 Insets 的页面容易被系统栏遮挡。
  - app stopped state 更严格，应用进入 stopped state 时 pending intent 会被取消。
  - 最低可安装 target API 继续抬高，target 低于 24 的应用不能安装。
  - BOOT_COMPLETED 启动部分前台服务、全局 DND 修改、音频焦点申请、WebSQL 等能力继续受限或废弃。
- API 变化：
  - OS-level app archiving / unarchiving。
  - `PdfRenderer`、OpenJDK 17、字体、自动语言切换、Camera Low Light Boost、HDR headroom、LoudnessCodecController、卫星连接、Wallet role 等增强。
  - 新增 `mediaProcessing` 前台服务类型。
- 兼容变化：
  - 含 Native 库必须跑 16 KB 环境。
  - 依赖“发现其他应用”或跨空间交互的业务要考虑 Private Space。
  - edge-to-edge、前台服务、通知、音频焦点、DND、启动广播都要按 target 35 重新验证。
- 迁移建议：
  - 全量检查沉浸式页面、底部导航页、输入法弹出页、全屏播放页。
  - Native 构建链路升级 NDK / AGP，并验证所有 ABI。
  - 前台服务按用途拆分 type；媒体转码类任务改用 `mediaProcessing`。
  - 对 app archiving 后的恢复路径、数据完整性、登录态恢复做回归。
- 关联文档：
  - [四大组件](docs/5.Android/5.1.基础/5.1.2.四大组件.md)
  - [数据存储与权限](docs/5.Android/5.1.基础/5.1.5.数据存储与权限.md)
  - [隐私合规](docs/5.Android/5.7.安全/5.7.4.隐私合规.md)
- 官方资料：
  - [Android 15 features and changes list](https://developer.android.com/about/versions/15/summary)
  - [API diff: API 34 -> API 35](https://developer.android.com/sdk/api_diff/35/changes/packages_index)

### Android 14（API 34）

- 版本定位：Android 14 是“权限和后台能力继续收紧 + 身份、健康、安全能力增强”的版本，常与 Android 13 连问。
- 机制变化：
  - Foreground service types 成为硬要求，前台服务必须明确用途，系统对合法性判断更严格。
  - Partial access to photos and videos：用户可以只授权部分媒体资源。
  - exact alarm 默认更谨慎。
  - 后台启动 Activity、隐式 Intent、安全导出、动态代码加载继续收紧。
  - Credential Manager 平台化，统一密码、Passkey、联合登录入口。
  - Health Connect 进入更重要的官方数据协作方向。
- API 变化：
  - Credential Manager、Health Connect、区域偏好、Grammatical Inflection、Ultra HDR、更多相机/媒体能力。
  - `READ_MEDIA_VISUAL_USER_SELECTED` 支持用户选择的媒体集合。
- 兼容变化：
  - 不能再假设读媒体权限等于完整相册访问。
  - 所有前台服务要补齐 type，并验证实际运行场景。
  - 动态代码加载文件必须只读，隐式 Intent 和 pending intent 安全边界更严格。
- 迁移建议：
  - 图片 / 视频选择优先使用系统 Photo Picker。
  - exact alarm 只在必要场景申请，并给用户解释入口。
  - 登录体系逐步评估 Credential Manager / Passkey。
- 关联文档：
  - [前台服务](docs/5.Android/5.1.基础/5.1.2.四大组件/5.1.2.2.Service/5.1.2.2.2.前台服务.md)
  - [权限申请](docs/5.Android/5.1.基础/5.1.5.数据存储与权限/5.1.5.6.权限申请.md)
  - [应用安全基础](docs/5.Android/5.7.安全/5.7.1.应用安全基础.md)
- 官方资料：
  - [Android 14 features and APIs overview](https://developer.android.com/about/versions/14/features)
  - [API diff: API 33 -> API 34](https://developer.android.com/sdk/api_diff/34/changes/packages_index)

### Android 13（API 33）

- 版本定位：权限模型和用户隐私体验变化很集中，真实项目最常遇到通知权限、媒体权限和系统照片选择器。
- 机制变化：
  - 通知权限 `POST_NOTIFICATIONS`：通知从默认可发变为需要用户明确授权。
  - 媒体权限拆分：`READ_EXTERNAL_STORAGE` 被拆成 `READ_MEDIA_IMAGES`、`READ_MEDIA_VIDEO`、`READ_MEDIA_AUDIO`。
  - 系统 Photo Picker：用户按项分享图片 / 视频，不必直接暴露整个媒体库。
  - `NEARBY_WIFI_DEVICES` 运行时权限。
  - Clipboard preview / 敏感内容保护。
  - Predictive back 开始进入开发者视野。
- API 变化：
  - Per-app language preferences。
  - Themed app icons、Quick Settings placement API、MIDI 2.0、Bluetooth LE Audio、OpenJDK 11 更新。
- 兼容变化：
  - 通知触达、媒体选择、Wi-Fi 配网/扫描、剪贴板读取都要重新评估授权时机和降级路径。
- 迁移建议：
  - 通知权限要设计触发时机，避免冷启动立即打断用户。
  - 访问媒体资源时同时判断系统版本和 `targetSdkVersion`。
  - 能用 Photo Picker 的地方优先用 Photo Picker。
- 关联文档：
  - [权限申请](docs/5.Android/5.1.基础/5.1.5.数据存储与权限/5.1.5.6.权限申请.md)
  - [隐私合规](docs/5.Android/5.7.安全/5.7.4.隐私合规.md)
- 官方资料：
  - [Android 13 features and APIs overview](https://developer.android.com/about/versions/13/features)
  - [API diff: API 32 -> API 33](https://developer.android.com/sdk/api_diff/33/changes/packages_index)

### Android 12L（API 32）

- 版本定位：12L 不是传统大重构版本，但对大屏、平板、折叠屏适配非常关键。
- 机制变化：
  - Taskbar 与 split-screen 体验增强。
  - Large screen compatibility mode 改善。
  - Activity Embedding 成为大屏分栏的重要方案。
  - Letterboxing、Insets、方向锁定、多窗口和任务切换在大屏设备上更显性。
- API 变化：
  - Jetpack WindowManager / Activity Embedding 成为主推大屏适配路径。
- 兼容变化：
  - 页面不能再以“手机竖屏全屏”作为默认不变假设。
- 迁移建议：
  - 检查 taskbar、split-screen、letterbox 下遮挡和布局错乱。
  - 优先支持响应式布局，而不是强锁方向。
  - 多 Activity 结构评估 Activity Embedding。
- 关联文档：
  - [Window 与输入事件](docs/5.Android/5.2.进阶/5.2.4.Window与输入事件.md)
  - [Compose 架构](docs/5.Android/5.5.架构与工程化/5.5.2.Jetpack架构组件/5.5.2.5.Compose架构.md)
- 官方资料：
  - [12L features and changes](https://developer.android.com/about/versions/12/12L/summary)
  - [API diff: API 31 -> API 32](https://developer.android.com/sdk/api_diff/32/changes/packages_index)

### Android 12（API 31）

- 版本定位：Android 12 把 UI、隐私、安全、权限同时推进，面试辨识度很高。
- 机制变化：
  - App Splash Screen API。
  - Material You 与 Widgets 改版。
  - Privacy Dashboard、摄像头/麦克风指示器、近似位置权限。
  - 新蓝牙权限：`BLUETOOTH_SCAN`、`BLUETOOTH_ADVERTISE`、`BLUETOOTH_CONNECT`。
  - Overlay 安全能力增强。
  - 声明 `intent-filter` 的组件需要明确 `android:exported`。
- API 变化：
  - SplashScreen API、RenderEffect、AVIF、RoundedCorner、VibratorManager、Game Mode、OpenJDK 11 部分更新。
- 兼容变化：
  - 自定义启动页、蓝牙、组件导出、位置精度、前台服务启动限制都容易在 target 31 升级时出问题。
- 迁移建议：
  - 自定义启动页迁移到新版 SplashScreen 体系。
  - 蓝牙相关业务改权限声明和申请逻辑。
  - 所有带 `intent-filter` 的组件显式检查导出属性。
- 关联文档：
  - [四大组件](docs/5.Android/5.1.基础/5.1.2.四大组件.md)
  - [权限申请](docs/5.Android/5.1.基础/5.1.5.数据存储与权限/5.1.5.6.权限申请.md)
  - [启动优化 SplashScreen](docs/5.Android/5.4.性能优化/5.4.1.启动优化/5.4.1.5.SplashScreen.md)
- 官方资料：
  - [Android 12 features and APIs overview](https://developer.android.com/about/versions/12/features)
  - [API diff: API 30 -> API 31](https://developer.android.com/sdk/api_diff/31/changes/packages_index)

### Android 11（API 30）

- 版本定位：隐私收紧开始变成系统常态的关键版本。
- 机制变化：
  - One-time permissions。
  - 权限自动重置。
  - Package visibility：应用默认不能随意看到其他已安装应用。
  - Scoped Storage 强制推进。
  - Data access auditing。
  - People and conversations、Bubbles、5G、媒体与设备控制增强。
- API 变化：
  - `PackageManager` 查询能力受 `<queries>` 限制。
  - `MANAGE_EXTERNAL_STORAGE` 成为高风险例外能力。
- 兼容变化：
  - 查询其他应用、拉起第三方、文件管理、相册扫描、分享下载目录都可能受影响。
- 迁移建议：
  - 查询其他应用前设计 `<queries>`，只声明必要包名 / intent。
  - 所有外部存储访问改为 MediaStore、SAF 或应用专属目录。
  - 权限申请结果不能假设用户一旦授权就永远有效。
- 关联文档：
  - [文件存储](docs/5.Android/5.1.基础/5.1.5.数据存储与权限/5.1.5.5.文件存储.md)
  - [权限申请](docs/5.Android/5.1.基础/5.1.5.数据存储与权限/5.1.5.6.权限申请.md)
- 官方资料：
  - [Android 11 overview](https://developer.android.com/about/versions/11)
  - [API diff: API 29 -> API 30](https://developer.android.com/sdk/api_diff/30/changes/packages_index)

### Android 10（API 29）

- 版本定位：现代 Android 权限、导航和存储模型的分水岭。
- 机制变化：
  - Scoped Storage 主线开启。
  - 深色主题成为平台级体验。
  - 手势导航影响全屏交互和边缘返回处理。
  - 隐私和位置限制增强，后台位置访问更敏感。
  - Foldables、5G、Biometrics、NNAPI、Vulkan 1.1 能力增强。
- API 变化：
  - BiometricPrompt 更成熟。
  - `MediaStore` 支持更细的集合式媒体访问。
  - `Settings Panel`、Bubbles 预览、共享快捷方式等能力。
- 兼容变化：
  - 外部存储访问不能再假设“拿到存储权限就能直接扫全盘”。
  - 沉浸式页面要考虑手势导航冲突。
- 迁移建议：
  - 相册、下载、导出文件、日志保存路径全部梳理。
  - 深色模式检查图片、文字、主题色、WebView。
- 关联文档：
  - [文件存储](docs/5.Android/5.1.基础/5.1.5.数据存储与权限/5.1.5.5.文件存储.md)
  - [Window](docs/5.Android/5.2.进阶/5.2.4.Window与输入事件/5.2.4.1.Window.md)
- 官方资料：
  - [Android 10 overview](https://developer.android.com/about/versions/10)
  - [API diff: API 28 -> API 29](https://developer.android.com/sdk/api_diff/29/changes/packages_index)

### Android 9（API 28）

- 版本定位：在全面可见的新交互之外，开始强推对非 SDK 接口的限制。
- 机制变化：
  - Display Cutout API。
  - 非 SDK 接口限制开始显性化。
  - JobScheduler 对网络成本感知增强。
  - HTTPS / 明文流量策略继续收紧。
  - 前台服务权限要求和后台限制继续强化。
- API 变化：
  - Notification smart reply、ImageDecoder、Multi-camera APIs。
  - Wi-Fi RTT、室内定位、TextClassifier、BiometricPrompt。
- 兼容变化：
  - 刘海屏页面不能继续假设状态栏区域不可用。
  - 使用反射或隐藏 API 的旧工程风险增大。
- 迁移建议：
  - 替换隐藏 API，避免依赖 framework 私有实现。
  - 检查状态栏、全屏、横屏、相机预览在 cutout 设备上的展示。
- 关联文档：
  - [Window](docs/5.Android/5.2.进阶/5.2.4.Window与输入事件/5.2.4.1.Window.md)
  - [相机](docs/5.Android/5.1.基础/5.1.6.多媒体与设备能力/5.1.6.3.设备能力/5.1.6.3.1.相机.md)
- 官方资料：
  - [Android 9 Pie overview](https://developer.android.com/about/versions/pie)
  - [API diff: API 27 -> API 28](https://developer.android.com/sdk/api_diff/28/changes/packages_index)

### Android 8.0 / 8.1（API 26 / 27）

- 版本定位：后台执行限制、通知渠道、Project Treble 和现代多任务体验的关键转折点。
- 机制变化：
  - Background execution limits。
  - Background location limits。
  - Notification channels。
  - Picture-in-picture。
  - Autofill framework。
  - Adaptive icons。
  - Project Treble 将平台框架与厂商实现解耦，改善系统升级链路。
- API 变化：
  - `startForegroundService()`、通知渠道、Autofill、PIP、TextView autosizing、Downloadable Fonts、Neural Networks API。
  - 8.1 增强 NNAPI、SharedMemory、WallpaperColors、Autofill 改进。
- 兼容变化：
  - 后台服务、隐式广播、后台定位旧写法要迁移。
  - 所有通知必须先规划 channel。
- 迁移建议：
  - 后台任务优先迁移 JobScheduler / WorkManager。
  - 前台任务及时调用 `startForeground()`，避免系统杀进程。
  - 通知渠道要稳定，不要随业务动态滥建。
- 关联文档：
  - [Service](docs/5.Android/5.1.基础/5.1.2.四大组件/5.1.2.2.Service.md)
  - [WorkManager](docs/5.Android/5.5.架构与工程化/5.5.2.Jetpack架构组件/5.5.2.3.WorkManager.md)
- 官方资料：
  - [Android 8.0 features and APIs](https://developer.android.com/about/versions/oreo/android-8.0)
  - [API diff: API 25 -> API 26](https://developer.android.com/sdk/api_diff/26/changes/packages_index)

### Android 7.0 / 7.1（API 24 / 25）

- 版本定位：多窗口、通知交互和文件共享安全变成真实工程问题。
- 机制变化：
  - Multi-window support。
  - Direct reply 与 bundled notifications。
  - `FileUriExposedException` 风险提升，文件共享要走 `FileProvider` 等安全路径。
  - Doze on the Go、Data Saver。
  - Vulkan、多语言、快捷方式等能力增强。
  - 7.1 增加 App Shortcuts、圆形图标、键盘图片插入等能力。
- API 变化：
  - `FileProvider` 重要性提升。
  - ICU4J 子集、OpenGL/Vulkan、Network Security Config。
- 兼容变化：
  - 页面不能再假设永远全屏单窗口。
  - 旧式 `file://` 共享会在 target 24 后抛异常。
- 迁移建议：
  - 分享图片、安装 APK、拍照输出 URI 改用 `content://`。
  - 检查 Activity resize、多窗口生命周期、配置变化。
- 关联文档：
  - [Activity](docs/5.Android/5.1.基础/5.1.2.四大组件/5.1.2.1.Activity.md)
  - [文件安全](docs/5.Android/5.7.安全/5.7.1.应用安全基础/5.7.1.3.文件安全.md)
- 官方资料：
  - [Android 7.0 for developers](https://developer.android.com/about/versions/nougat/android-7.0)
  - [API diff: API 23 -> API 24](https://developer.android.com/sdk/api_diff/24/changes/packages_index)

### Android 6.0（API 23）

- 版本定位：现代权限模型起点，直到今天仍是高频面试题来源。
- 机制变化：
  - Runtime permissions。
  - Doze / App Standby。
  - Fingerprint authentication。
  - App Links。
  - Auto Backup for Apps。
  - Direct Share。
- API 变化：
  - `requestPermissions()` / `onRequestPermissionsResult()`。
  - `FingerprintManager`、`ConfirmCredential`、`NetworkStatsManager`、MIDI、Voice Interaction。
- 兼容变化：
  - 危险权限从安装时授权变成运行时按需申请。
  - 后台任务不能假设设备一直活跃。
  - 自动备份可能把 SharedPreferences / 数据库带到新设备，需要排除敏感数据。
- 迁移建议：
  - 权限按业务场景延迟申请，并处理拒绝、永久拒绝、降级能力。
  - 定时任务结合 Doze 规则设计，必要时用高优先级推送或白名单申请。
- 关联文档：
  - [权限申请](docs/5.Android/5.1.基础/5.1.5.数据存储与权限/5.1.5.6.权限申请.md)
  - [密钥管理](docs/5.Android/5.7.安全/5.7.2.网络与数据安全/5.7.2.4.密钥管理.md)
- 官方资料：
  - [Android 6.0 APIs overview](https://developer.android.com/about/versions/marshmallow/android-6.0)
  - [API diff: API 22 -> API 23](https://developer.android.com/sdk/api_diff/23/changes/packages_index)

### Android 5.0 / 5.1（API 21 / 22）

- 版本定位：视觉体系和运行时体系都发生大变动的一代，很多“现代 Android 开发”默认前提从这里形成。
- 机制变化：
  - Material Design。
  - ART 成为默认运行时。
  - RecyclerView、转场动画、阴影、矢量与主题体系成熟。
  - Camera2 API。
  - JobScheduler。
  - MediaSession / MediaController。
  - Storage Access Framework 进一步增强。
- API 变化：
  - `JobScheduler`、`Camera2`、`MediaProjection`、`VectorDrawable`、`Palette`、`Toolbar`、`RecyclerView`（support library）。
  - 5.1 增强多 SIM、HD Voice、Device Protection。
- 兼容变化：
  - Dalvik 到 ART 的差异会暴露反射、JNI、类加载、安装优化问题。
  - 新主题和状态栏颜色改变旧 UI 假设。
- 迁移建议：
  - 相机、任务调度、媒体播放、动画过渡按 5.x 新能力重新设计。
  - 关注 ART 下冷启动、dex2oat、MultiDex、反射兼容。
- 关联文档：
  - [ART](docs/2.JVM/2.2.Android虚拟机/2.2.2.ART.md)
  - [JobScheduler / WorkManager](docs/5.Android/5.5.架构与工程化/5.5.2.Jetpack架构组件/5.5.2.3.WorkManager.md)
  - [UI 体系与资源](docs/5.Android/5.1.基础/5.1.4.UI体系与资源.md)
- 官方资料：
  - [Android 5.0 APIs overview](https://developer.android.com/about/versions/lollipop/android-5.0)
  - [API diff: API 20 -> API 21](https://developer.android.com/sdk/api_diff/21/changes/packages_index)

### Android 4.4（API 19 / 20）

- 版本定位：低内存设备优化和系统服务能力扩展的一代，也是 ART 从技术预览进入开发者视野的节点。
- 机制变化：
  - Project Svelte：系统面向 512 MB RAM 设备优化，减少系统进程内存占用，服务启动更克制。
  - 新增 `ActivityManager.isLowRamDevice()` 供应用按设备内存裁剪功能。
  - 沉浸式全屏、透明状态栏/导航栏成为常见 UI 需求。
  - Host Card Emulation（HCE）让应用可模拟 NFC 智能卡。
  - 原生打印框架、Storage Access Framework、Screen recording、WebView 基于 Chromium。
  - ART 作为开发者选项技术预览出现。
- API 变化：
  - `PrintManager`、`DocumentsProvider`、`SmsManager` 默认短信应用角色、`AlarmManager.setExact()`。
  - API 20 是 KitKat Watch / wearable 扩展相关级别。
- 兼容变化：
  - 低内存设备上后台服务、图片缓存、大对象和常驻进程风险更高。
  - 旧 WebView 行为在 Chromium WebView 下可能有差异。
- 迁移建议：
  - 针对 low-RAM 设备关闭大图、复杂动画和预加载。
  - WebView 业务补 UA、缓存、JSBridge、文件选择等回归。
- 关联文档：
  - [WebView 与混合容器](docs/5.Android/5.2.进阶/5.2.5.WebView与混合容器.md)
  - [NFC](docs/5.Android/5.1.基础/5.1.6.多媒体与设备能力/5.1.6.3.设备能力/5.1.6.3.5.NFC.md)
  - [内存优化](docs/5.Android/5.4.性能优化/5.4.3.内存优化.md)
- 官方资料：
  - [Android KitKat](https://developer.android.com/about/versions/kitkat)
  - [Android 4.4 APIs](https://developer.android.com/about/versions/android-4.4)

### Android 4.1 / 4.2 / 4.3（API 16 / 17 / 18）

- 版本定位：Jelly Bean 重点改善交互流畅度、通知能力、多用户和平板/设备能力。
- 机制变化：
  - Project Butter 改善触摸响应、动画和渲染流畅度。
  - 可展开通知、通知操作按钮、锁屏 Widget、Daydream。
  - 4.2 引入多用户（平板）、Quick Settings、Photo Sphere、Miracast。
  - 4.3 引入受限资料、Bluetooth Smart / BLE、OpenGL ES 3.0、TRIM。
- API 变化：
  - `Choreographer`、`ViewPropertyAnimator`、`Notification.BigTextStyle` 等通知样式。
  - `DisplayManager`、`MediaRouter`、`BluetoothGatt`、OpenGL ES 3.0。
- 兼容变化：
  - 动画和帧同步开始更依赖 VSync 节奏。
  - 多用户和受限资料要求应用不要把“设备 == 单用户”写死。
- 迁移建议：
  - UI 性能分析关注掉帧、主线程耗时和布局层级。
  - 蓝牙外设和低功耗设备从 4.3 起考虑 BLE 能力。
- 关联文档：
  - [VSync](docs/5.Android/5.6.Framework/5.6.3.WindowManager与SurfaceFlinger/5.6.3.4.VSync.md)
  - [蓝牙](docs/5.Android/5.1.基础/5.1.6.多媒体与设备能力/5.1.6.3.设备能力/5.1.6.3.4.蓝牙.md)
- 官方资料：
  - [Jelly Bean](https://developer.android.com/about/versions/jelly-bean)

### Android 4.0（API 14 / 15）

- 版本定位：Ice Cream Sandwich 统一手机和平板 UI，是 Android 从早期碎片化体验走向统一设计语言的重要版本。
- 机制变化：
  - Holo 主题和系统导航风格统一。
  - 硬件加速默认化范围扩大。
  - Face Unlock、Android Beam、Wi-Fi Direct、数据流量控制。
  - Contacts / Calendar / Camera / Browser / Email 等系统应用体验重构。
- API 变化：
  - `CalendarContract`、`ContactsContract` 增强。
  - `TextureView`、`GridLayout`、`Switch`、`PopupMenu`、`ShareActionProvider`。
  - Social stream、Visual voicemail、VPN、Accessibility 能力增强。
- 兼容变化：
  - 老 UI 需要适配 action bar、主题、硬件加速绘制差异。
  - NFC / Beam 分享和联系人合并影响社交类应用。
- 迁移建议：
  - 检查自定义 View 在硬件加速下的绘制行为。
  - 使用系统分享、联系人、日历 API 代替私有数据访问。
- 关联文档：
  - [自定义 View](docs/5.Android/5.1.基础/5.1.4.UI体系与资源/5.1.4.2.自定义View.md)
  - [draw](docs/5.Android/5.2.进阶/5.2.3.View体系与渲染/5.2.3.3.draw.md)
- 官方资料：
  - [Android 4.0 APIs](https://developer.android.com/about/versions/android-4.0)

### Android 3.x（API 11 / 12 / 13）

- 版本定位：Honeycomb 面向平板，奠定 Fragment、ActionBar、Loader 和更复杂大屏 UI 的基础。
- 机制变化：
  - 平板专用系统 UI、系统栏、多窗格布局。
  - Fragment 成为大屏模块化 UI 的核心抽象。
  - ActionBar、Loader、Property Animation、硬件加速、RenderScript。
  - 3.1 / 3.2 增强 USB host/accessory、可缩放 Widget、更多屏幕尺寸适配。
- API 变化：
  - `Fragment`、`LoaderManager`、`ActionBar`、`PropertyAnimator`、`RenderScript`。
  - `View` layer、drag and drop、clipboard、MTP/PTP、RTP 等能力。
- 兼容变化：
  - 平板布局不再适合简单拉伸手机界面。
  - Fragment 生命周期和 Activity 生命周期组合更复杂。
- 迁移建议：
  - 大屏页面按主从布局、双栏/三栏、横竖屏状态保存设计。
  - 老项目如仍用平台 Fragment，应明确与 AndroidX Fragment 的边界。
- 关联文档：
  - [Fragment 与 Jetpack 基础组件](docs/5.Android/5.1.基础/5.1.3.Fragment与Jetpack基础组件.md)
  - [动画和手势](docs/5.Android/5.1.基础/5.1.4.UI体系与资源/5.1.4.3.动画和手势.md)
- 官方资料：
  - [Android 3.0 APIs](https://developer.android.com/about/versions/android-3.0)

### Android 2.3（API 9 / 10）

- 版本定位：Gingerbread 强化通信、传感器、游戏输入和 Native 能力，是早期智能手机生态成熟的重要版本。
- 机制变化：
  - 新增 SIP 协议栈和 VoIP API。
  - NFC 栈和 NDEF tag 读取能力。
  - 新增陀螺仪、旋转向量、线性加速度、重力、气压计等传感器类型。
  - 支持多摄查询和前后摄能力。
  - 游戏输入、音频、图形、NativeActivity / NDK 能力增强。
- API 变化：
  - `android.net.sip`、`android.nfc`、`Sensor` 新类型、`Camera.getNumberOfCameras()`。
  - `DownloadManager`、`StrictMode`、`Gingerbread` 并发 GC 改善。
- 兼容变化：
  - 设备硬件能力差异大，NFC / SIP / 陀螺仪 / 多摄都要用 feature 检测。
- 迁移建议：
  - 使用 `<uses-feature>` 做市场过滤，并在运行时兜底判断。
  - 对游戏和音视频场景关注输入延迟与 Native 崩溃定位。
- 关联文档：
  - [传感器](docs/5.Android/5.1.基础/5.1.6.多媒体与设备能力/5.1.6.3.设备能力/5.1.6.3.2.传感器.md)
  - [Native 调试](docs/5.Android/5.2.进阶/5.2.6.NDK与Native/5.2.6.2.Native调试.md)
- 官方资料：
  - [Android 2.3 APIs](https://developer.android.com/about/versions/android-2.3)

### Android 2.2（API 8）

- 版本定位：Froyo 重点改善性能、企业能力和应用分发/安装体验。
- 机制变化：
  - Dalvik JIT 显著提升执行性能。
  - 支持安装应用到外部存储。
  - Cloud to Device Messaging（C2DM）是后来推送体系的重要前身。
  - 浏览器 V8 JavaScript 引擎、企业 Exchange 能力、便携热点等增强。
- API 变化：
  - `android:installLocation`。
  - `BackupManager`、`DevicePolicyManager`、`AccountManager` / sync 能力增强。
- 兼容变化：
  - App to SD 影响私有数据、Widget、服务、输入法、账号等组件可用性。
  - JIT 暴露部分依赖解释器时序的 Native / 反射问题。
- 迁移建议：
  - 对含 Widget、Service、IME、AccountAuthenticator 的应用谨慎允许安装到 SD。
  - 性能问题从该版本起要区分解释执行、JIT、I/O 和 GC。
- 关联文档：
  - [Dalvik](docs/2.JVM/2.2.Android虚拟机/2.2.1.Dalvik.md)
  - [JIT](docs/2.JVM/2.2.Android虚拟机/2.2.2.ART/2.2.2.2.JIT.md)
- 官方资料：
  - [Android 2.2 APIs](https://developer.android.com/about/versions/android-2.2)

### Android 2.0 / 2.1（API 5 / 6 / 7）

- 版本定位：Eclair 扩展账号、同步、地图、相机和浏览器能力，开始接近完整智能手机平台体验。
- 机制变化：
  - 多账号同步，Exchange 支持增强。
  - Bluetooth 2.1、联系人 Quick Contact、Live Wallpaper。
  - 浏览器 HTML5 能力增强。
  - 相机支持闪光灯、数码变焦、白平衡、色彩效果等能力。
- API 变化：
  - `AccountManager`、`SyncAdapter`、`BluetoothAdapter`、`WallpaperService`。
  - `ContactsContract` 替代旧联系人 API。
- 兼容变化：
  - 联系人、账号、同步模型开始从单一 Google 账号扩展到多账号。
  - 旧联系人 API 迁移成本较高。
- 迁移建议：
  - 账号体系按 `AccountManager` / sync adapter 设计，不直接耦合单一服务。
  - 旧联系人访问迁移到 `ContactsContract`。
- 关联文档：
  - [ContentProvider](docs/5.Android/5.1.基础/5.1.2.四大组件/5.1.2.4.ContentProvider.md)
  - [同步/后台任务](docs/5.Android/5.5.架构与工程化/5.5.2.Jetpack架构组件/5.5.2.3.WorkManager.md)
- 官方资料：
  - [Android 2.0 APIs](https://developer.android.com/about/versions/android-2.0)

### Android 1.6（API 4）

- 版本定位：Donut 解决早期设备形态扩展问题，开始正式支持多屏幕尺寸和密度。
- 机制变化：
  - 多屏幕尺寸 / 密度支持。
  - Quick Search Box 统一本地和网络搜索入口。
  - CDMA 支持，扩大运营商和设备覆盖。
  - 文本转语音（TTS）引擎。
  - Battery usage、Android Market 展示改进。
- API 变化：
  - `supports-screens`、density 资源适配机制。
  - `TextToSpeech`、`GestureDetector` / gesture library、VPN control panel。
- 兼容变化：
  - 不能再假设所有设备都是 HVGA / mdpi。
  - 资源目录和布局适配开始成为工程基本功。
- 迁移建议：
  - 使用 dp/sp、density-specific drawable、限定符资源。
  - 搜索能力接入系统 SearchManager 时注意隐私和索引范围。
- 关联文档：
  - [布局和控件](docs/5.Android/5.1.基础/5.1.4.UI体系与资源/5.1.4.1.布局和控件.md)
  - [资源系统](docs/5.Android/5.1.基础/5.1.4.UI体系与资源/5.1.4.4.资源系统.md)
- 官方资料：
  - [Android 1.6 Platform](https://developer.android.com/about/versions/android-1.6)

### Android 1.5（API 3）

- 版本定位：Cupcake 是早期 Android 真正开始补齐手机应用生态能力的版本。
- 机制变化：
  - 软键盘和输入法框架，支持第三方 IME。
  - AppWidget、Live Folders。
  - 视频录制、视频/图片分享 Intent。
  - 语音识别 Intent、OpenGL 辅助框架、传感器 API 重构。
- API 变化：
  - `AppWidgetProvider`、`InputMethodService`、`RecognizerIntent`、`GLSurfaceView`。
  - `SlidingDrawer`、`HorizontalScrollView` 等 UI 组件。
- 兼容变化：
  - 输入法、桌面 Widget、视频录制成为应用形态的一部分。
  - `minSdkVersion` / API Level 概念开始对兼容安装判断更重要。
- 迁移建议：
  - 早期应用如果支持 1.5，需要避免使用更高 API，或通过反射/兼容库隔离。
  - Widget 和 IME 要特别关注生命周期与进程常驻成本。
- 关联文档：
  - [View 体系与渲染](docs/5.Android/5.2.进阶/5.2.3.View体系与渲染.md)
  - [音视频](docs/5.Android/5.1.基础/5.1.6.多媒体与设备能力/5.1.6.2.音视频.md)
- 官方资料：
  - [Android 1.5 Platform](https://developer.android.com/about/versions/android-1.5)

### Android 1.1（API 2）

- 版本定位：早期维护版本，主要修正 API 和系统应用细节。
- 机制变化：
  - 地图搜索结果可展示详情和评论。
  - 通话界面、消息附件保存、系统布局 marquee 等细节增强。
- API 变化：
  - API Level 从 1 演进到 2，主要是修正和小规模补充。
- 兼容变化：
  - 早期设备和 SDK 差异很大，今天主要作为历史背景。
- 迁移建议：
  - 现代项目不再适配该版本；只需在版本演进中理解 Android API Level 兼容机制的起点。
- 官方资料：
  - [Android version history](https://en.wikipedia.org/wiki/Android_version_history)

### Android 1.0（API 1）

- 版本定位：首个商业 Android 版本，随 HTC Dream / T-Mobile G1 进入市场。
- 机制变化：
  - Android Market、浏览器、相机、Gmail / Contacts / Calendar 同步、Google Maps、YouTube、通知栏等核心体验出现。
  - Activity、Service、BroadcastReceiver、ContentProvider、Intent、Manifest、资源系统等应用模型雏形确立。
- API 变化：
  - API Level 1 是 Android 应用框架基线。
- 兼容变化：
  - 缺少软键盘、视频录制、蓝牙文件传输、多屏幕密度等后来常见能力。
- 迁移建议：
  - 现代项目无需适配；学习价值在于理解四大组件、Intent 和资源系统为什么从一开始就是 Android 应用模型核心。
- 关联文档：
  - [Android系统架构](docs/5.Android/5.1.基础/5.1.1.Android基础概述/5.1.1.2.Android系统架构.md)
  - [四大组件](docs/5.Android/5.1.基础/5.1.2.四大组件.md)
- 官方资料：
  - [Android version history](https://en.wikipedia.org/wiki/Android_version_history)

## 面试答题模板

如果被问“Android 13 / 14 / 15 有哪些变化”，推荐按这个顺序答：

1. 先给一句版本主线。
   - 例如：“Android 14 的主线是前台服务、部分媒体访问和系统安全进一步收紧。”
2. 再按三类列变化。
   - 权限 / 隐私。
   - 后台 / 组件 / 系统行为。
   - UI / 大屏 / 交互。
3. 最后补工程动作。
   - Manifest 调整。
   - 权限申请逻辑调整。
   - 页面 Insets 适配。
   - 后台任务和通知改造。

## 最后只记不踩坑版

- Android 6：先想到运行时权限和 Doze。
- Android 7：先想到多窗口和 `file://` 共享风险。
- Android 8：先想到后台执行限制和通知渠道。
- Android 9：先想到刘海屏和非 SDK 接口限制。
- Android 10：先想到分区存储。
- Android 11：先想到单次授权、自动重置、包可见性。
- Android 12：先想到 SplashScreen、Material You、蓝牙权限、`android:exported`。
- Android 13：先想到通知权限、媒体权限拆分、Photo Picker。
- Android 14：先想到前台服务类型、部分照片视频访问、Credential Manager。
- Android 15：先想到 Private Space、16 KB 页大小、edge-to-edge。
- Android 16：先想到预测返回默认化、16 KB 兼容、进度通知、UI 默认值继续收紧。
- Android 17：先想到 Beta 变更仍在流动，重点先测内存、MessageQueue、ECH、本地网络权限和 OTP 保护。

