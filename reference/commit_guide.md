# commit message 模板与提交能力

步骤 7 按本文件决定提交方式与模板填写。换版改本文件，不动 SKILL.md 流程。

## 当前提交能力：仅生成不提交

当前按「仅生成不提交」设计——agent 不自动执行 `git commit`，commit message 在对话内生成，由用户复制后到源码仓库手动提交。待具备可自动提交的条件（源码树为 git 工作树、且 agent 有提交权限）后，本节改为「支持自动提交」。

> 安全要求（不随环境改变）：提交为不可逆对外操作，即便环境支持自动提交，也须经用户明确授权后执行。

## 模板填写约束

**简洁**：commit message 以说清"问题 + 解法"为度，不堆砌日志原文、不铺陈分析过程。一句话概括精炼到一行；Root cause / Solution 只保留讲清场景、逻辑、现象所必需的信息。

**整次一条**：一次 /audio-debug 改了多个问题/文件，合并成一条 commit message。多个独立问题时，`Root cause` / `Solution` 按问题分条枚举——每条对应一个 fix_plan 文档，条数 = 本次落地的 fix_plan 文档数。

按源码仓库模板填，字段映射：

- `FIX XXX [XXX][XXX][XXX] <一句话概括>`：XXX（票号 / 模块标签）由用户填，skill 不动；一句话概括本次改了什么、解决了什么现象。
- `Project: XXX`：用户填，skill 不动。
- `Root cause:`：清晰描述在什么场景下 + 什么代码逻辑 + 导致什么问题现象。
- `Solution:`：用什么代码逻辑解决了问题。
- `Test suggest:`：列出验证方式（复现步骤 / 关键日志检查点 / dumpsys 核对项），用户照此验证。

**只写最终落地项**：已回退的改动不写入 Solution，commit message 只反映步骤 6 后最终保留的代码改动。

## 样例

单问题（票号 / Project 为占位，实际由用户填）：

```
FIX XXX-12345 [XXX][ANDROID][AUDIO] 修复设备切换后深缓冲播放偶发 underrun

Project: XXX
Root cause: 设备切换（如蓝牙→扬声器）时，AudioFlinger 未按新设备采样率重算 fast mixer 帧数，仍沿用旧设备周期，导致新设备播放缓冲不足、偶发 underrun 卡顿。
Solution: 在 AudioFlinger 设备切换处理处补一次 fast mixer 帧数重算，按新设备采样率刷新缓冲。

Test suggest: 设备切换后 logcat 无 AudioFlinger underrun、播放连续无卡顿。
```

多问题：`Root cause:` / `Solution:` 按上面格式逐条重复，每条对应一个 fix_plan 文档，整次合并成一条 commit。
