# FCMFix ColorOS

修复 **ColorOS 16 / 17（国行）** 拦截 Google FCM 推送的 LSPosed 模块，需要 **Root + LSPosed**。

GMS 已经收到推送，但 ColorOS 的后台限制把消息拦在半路：应用划掉后台或被系统停止后收不到推送，
或者要等亮屏、打开应用才收到。本模块只在消息从 GMS 投递到你选中的应用这一段解除这些限制，
并阻止 ColorOS 切断 Google 核心服务的网络和唤醒。

它不是推送服务器、代理软件或常驻保活工具，也不能让 GMS 连上 FCM 服务器。

## 主要功能

- 允许真实 FCM 广播到达允许列表中处于 stopped 状态的应用；
- 绕过 ColorOS 对 FCM 目标应用的分类、自启动、GCM bind、service 启动、场景广播代理、冷启动拦截、
  云控“恶意应用”名单和关联启动限制（均先核验来自真实 GMS 的 FCM）；
- 在 FCM 到达后的短时投递窗口内，防止 Hans（含锁屏快速冻结）再次冻结目标或关闭其 socket；
- 补充 GMS、GSF 和 Play 商店需要的 Doze 条目；
- 阻止 ColorOS 电池组件把 Google 核心包设为禁止全部联网；
- 阻止 ColorOS 17 降级 GMS 唤醒闹钟、把 Google 应用放入受限待机分组，避免熄屏后 FCM 断开；
- ColorOS 17 流量管理隐藏 Google 联网开关时，清除旧的禁止联网状态；
- 深度睡眠（睡眠待机优化）期间让 GMS 与 HeyTap 推送一样保持联网，熄屏弱信号时不对 GMS 断网；
- FCM 投递窗口内不拦截目标应用的后台作业，便于 Gmail 等应用收到推送后同步内容；
- 应用内显示模块激活状态与缺少的作用域，应用列表支持搜索和筛选。

修复仅针对用户勾选的允许列表和必要的 Google 核心包。仍开放 Google 联网开关的系统（如 ColorOS 16）上，
不会覆盖用户在系统流量管理中手动设置的 Wi-Fi 或移动数据权限。

## 验证情况

- 一加 15 国行（PLK110）ColorOS 16 `16.0.10.500`：严格实机验证，已停止的应用可被 FCM 唤醒并生成通知；
- 一加 15 国行 ColorOS 17 `17.0.0.102(CN01)`：按全量包核对全部 Hook 点，维护者日常使用正常。

其他 ColorOS 16/17 机型可能可用，但未经同等级验证。

## 安装

1. 确认手机已 Root，并安装可正常工作的 LSPosed（Modern Xposed API）。
2. 安装 Release 中的 APK。
3. 在 LSPosed 中启用模块，作用域**同时勾选**“系统框架”和“电池（`com.oplus.battery`）”。
4. 重启手机。
5. 打开 FCMFix ColorOS，勾选确实需要 FCM 后台唤醒的应用。

包名为 `io.github.artifical0.fcmfix.coloros`。从旧包名版本迁移时不能覆盖安装，
请先停用并卸载旧版，再重新配置 LSPosed 作用域和允许列表。

## 常见问题

FCM Diagnostics 亮屏时也一直 disconnected，通常是网络问题：国内 `mtalk.google.com` 的解析常被污染，
部分网络还会封锁 5228–5230 端口，需要自行解决 DNS、hosts 或代理，本模块无法处理。

## 权限与风险

- APK 不申请 `INTERNET` 权限，不通过开发者服务器中转推送；
- `QUERY_ALL_PACKAGES` 用于扫描本机包含 FCM 接收组件的应用；
- 这是 system_server 级别的 Hook，安装前请保留进入安全模式或禁用 LSPosed 模块的恢复手段；
- 系统 OTA 后应重新验证关键 Hook 和锁屏推送。

## 源码、问题反馈与技术分析

- [源码与下载](https://github.com/Artifical0/fcmfix-coloros)
- [如何提交问题与日志](https://github.com/Artifical0/fcmfix-coloros/blob/master/docs/report-issue.md)
- [ColorOS 17 Hook 核对](https://github.com/Artifical0/fcmfix-coloros/blob/master/docs/oneplus15-coloros17-fcm-analysis.md)
- [ColorOS 16 国行/国际版差分与 Hook 分析](https://github.com/Artifical0/fcmfix-coloros/blob/master/docs/oneplus15-coloros16-fcm-analysis.md)
- [上游项目 kooritea/fcmfix](https://github.com/kooritea/fcmfix)
