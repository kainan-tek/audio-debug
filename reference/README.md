# reference — 调试参考

按需查的参考资料。非日志驱动主线——开发场景或建立全栈心智模型时查用。

## 现有文档

- `commit_guide.md` — 步骤 7 commit message 模板、字段映射与填写约束
- `build_guide.md` — 编译命令备忘与各平台编译模块（当前不支持自动编译）
- `adb_diagnostics.md` — 活体调试只读诊断命令清单与证据处置

## 待补文档

以下为可选补充，未补不影响主线（agent 原生能力覆盖；`platforms/*_paths.md` 提供源码路径索引）。补时由领域知识撰写，避免凭空编造路径或机制。

- `architecture.md` —— AAOS 音频全栈架构总览（GVM/PVM/ADSP 分层、数据面/控制面调用栈）
- `debug_reference.md` —— 常见故障模式与调试手法（underrun / 路由异常 / ADSP 超时 等）
- 各平台架构差异要点（SA8295 QCOM / MT8091 MTK / SA8397 Gunyah）
