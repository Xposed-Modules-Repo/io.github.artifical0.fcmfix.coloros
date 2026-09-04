# FCMFix ColorOS

FCMFix ColorOS 是一个需要 **Root + LSPosed** 的系统级修复模块，用于解决一加 15
国行 ColorOS 16 阻止 Google FCM 唤醒后台、无进程或已停止应用的问题。

它不是推送服务器、代理软件或常驻保活工具。它在 Google Play 服务已经收到消息后，
修复消息从 GMS 传递到目标应用时被 ColorOS 拦截的问题。

## 主要功能

- 允许真实 FCM 广播到达允许列表中处于 stopped 状态的应用；
- 绕过 ColorOS 对 FCM 目标应用的分类、自启动、GCM bind 和 service 启动限制；
- 在 FCM 到达后的短时投递窗口内防止 Hans 再次冻结目标和关闭 socket；
- 补充 GMS、GSF 和 Play 商店需要的 Doze 条目；
- 阻止 ColorOS 电池组件自动将 Google 核心包设为禁止全部联网。

修复仅针对用户勾选的允许列表和必要的 Google 核心包，不会覆盖用户在系统
流量管理中手动设置的 Wi-Fi 或移动数据权限。

## 已验证环境

- 设备：一加 15 国行版（PLK110）；
- 系统：ColorOS `16.0.10.500` / Android 16；
- 环境：Root + LSPosed Modern Xposed API；
- 强停测试：Nekogram 在 `stopped=true` 且无进程时，可被 FCM 广播唤醒并生成通知。

其他 ColorOS 16 设备和后续 OTA 可能可用，但未经过同等级别的实机回归。

## 安装

1. 确认手机已 Root，并安装可正常工作的 LSPosed。
2. 下载并安装 Release 中的 APK。
3. 在 LSPosed 中启用模块，作用域只勾选“系统框架”和“电池（`com.oplus.battery`）”。
4. 重启手机。
5. 打开 FCMFix ColorOS，勾选确实需要 FCM 后台唤醒的应用。

包名为 `io.github.artifical0.fcmfix.coloros`。从旧包名版本迁移时不能覆盖安装，
请先停用并卸载旧版，再重新配置 LSPosed 作用域和允许列表。

## 权限与风险

- APK 不申请 `INTERNET` 权限，不通过开发者服务器中转推送；
- `QUERY_ALL_PACKAGES` 用于扫描本机包含 FCM 接收组件的应用；
- 这是 system_server 级别 Hook，安装前请保留进入安全模式或禁用 LSPosed 模块的恢复手段；
- 系统 OTA 后应重新验证关键 Hook 和锁屏推送。

## 源码、问题反馈与技术分析

- [源码与问题反馈](https://github.com/Artifical0/fcmfix-oneplus15-coloros16)
- [ColorOS 16 国行/国际版差分与 Hook 分析](https://github.com/Artifical0/fcmfix-oneplus15-coloros16/blob/master/docs/oneplus15-coloros16-fcm-analysis.md)
- [上游项目 kooritea/fcmfix](https://github.com/kooritea/fcmfix)
