# FCMFix ColorOS

修复 **ColorOS 16 / 17（国行）** 拦截 Google FCM 推送的 LSPosed 模块，需要 **Root + LSPosed**。

应用划掉后台或被系统停止后收不到推送、要等亮屏或打开应用才收到，多数是 ColorOS 的后台限制把消息拦在了
GMS 和应用之间。本模块解除这一段的限制，并阻止 ColorOS 切断 Google 核心服务的网络和唤醒。

基于上游 kooritea/fcmfix 53 版，13.0 起使用独立的版本号（此前形如 `53-coloros-12.1`）。

## 安装

1. 确认手机已 Root，并安装可正常工作的 LSPosed（libxposed API 100 及以上）。
2. 安装 Release 中的 APK。
3. 在 LSPosed 中启用模块，作用域**同时勾选**“系统框架”和“电池（`com.oplus.battery`）”。
4. 重启手机。
5. 打开 FCMFix ColorOS，在“应用”页打开确实需要 FCM 后台推送的应用的开关，或开启“自动放行包含 FCM 的应用”，
   并确认这些应用的通知权限已开启。
6. “状态”页应显示“一切正常”，并逐项检查作用域、Hook 和 FCM 连接；收不到推送时，“应用”页会标出通知被关闭等问题，
   并显示每个放行应用最近一次收到推送的时间。

从旧包名版本迁移时不能覆盖安装：先停用并卸载旧版，再安装新版并重新配置。不要同时启用两个 FCMFix。

## 它能解决什么

- 应用划掉后台、被系统清理或处于“已停止”状态时，FCM 仍能唤醒它并显示通知；
- 推送到达后，应用不会马上被 ColorOS 再次冻结或断网，能及时拉取消息内容；
- ColorOS 不再把 GMS、Play 商店等 Google 核心服务设为禁止联网（包括开机解锁后的自动禁网）；
- 熄屏、Doze 以及“睡眠待机优化”按应用断网期间，GMS 保持联网；网络恢复后自动重连。

## 它不能解决什么

- **GMS 连不上 FCM 服务器**：国内 `mtalk.google.com` 的解析常被污染，部分网络封锁 5228–5230 端口。
  FCM Diagnostics 亮屏时也一直 disconnected，需要自行解决 DNS、hosts 或代理；
  “状态”页的“FCM 连接”会显示是否已连接、连接是否无响应，以及开机以来的重连和断线次数；
- 应用本身不使用 FCM、服务器没有发送消息，或通知权限已关闭；
- 系统整机断网（例如深度睡眠直接关闭 Wi-Fi / 移动数据）。

## 验证情况

一加 15 国行（PLK110）ColorOS 16 `16.0.10.500` 严格实机验证；ColorOS 17 `17.0.0.102(CN01)` 按全量包核对 Hook 点，
维护者日常使用正常。其他 ColorOS 16/17 机型可能可用，但未经同等级验证。

## 权限与风险

- APK 不申请 `INTERNET` 权限，不通过开发者服务器中转推送；导出的报告只保存在本机，由你决定是否分享；
- `ACCESS_NETWORK_STATE`（安装时自动授予，不能联网）用于在“时间线”中记录网络切换；`QUERY_ALL_PACKAGES` 仅用于扫描包含 FCM 的应用；
- 这是 system_server 级别的 Hook，安装前请保留进入安全模式或禁用 LSPosed 模块的恢复手段。

## 更多

- [源码、常见问题与上游对比](https://github.com/Artifical0/fcmfix-coloros)
- [工作原理](https://github.com/Artifical0/fcmfix-coloros/blob/master/docs/how-it-works.md)
- [如何提交问题与日志](https://github.com/Artifical0/fcmfix-coloros/blob/master/docs/report-issue.md)
- [上游项目 kooritea/fcmfix](https://github.com/kooritea/fcmfix)
- Telegram 更新频道 [@ByteJuno](https://t.me/ByteJuno)：新版本发布、构建结果都会推送，欢迎关注
