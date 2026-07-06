# Audio Debug — 车载音频日志分析

`/audio-debug` — AAOS 音频系统日志分析 skill。Agent 直接读日志和源码，定位故障根因，并经多遍审查后产出修复方案。

**日志输入**：直接读取用户给出的日志文件（扩展名不限——`.log`/`.txt`/`.qlog`/无扩展名等均可，按内容判断是否为日志）。若提供日志全量包（`.zip`/`.7z`），需用户先解压出日志文件再提供。

**模型选择**：音频日志动辄数 MB、叠加多遍审查与多 agent，**上下文易超限**。请使用支持 **1M 上下文**的模型，例如 **GLM-5.2**，避免分析中途被截断。

## 核心设计

**Agent 是分析引擎，工具只提供导航。**

- 不需要 Python 分析代码 — Agent 原生能力覆盖模式识别、因果分析
- 不需要故障日志示例 — Agent 看到日志行就能识别故障模式
- 不需要 per-module skill.md — 模块即各平台 paths.md 的章节，无需单独索引

Agent 查源码路径文件定位代码（知识图暂不支持，详见 [SKILL.md](SKILL.md) / [设计文档](docs/DESIGN.md)）。

**分析方法不设硬约束** — Agent 已具备日志模式识别与因果分析的原生能力，能自行判断读哪段日志、追哪条传播链、何时读源码。Skill 只规定"做没做完"的完成判据（防偷懒、防草率收工），**不规定"怎么做"**——方法由 Agent 自主决定，工具只提供导航，不做过度限制。

## 使用方式

两种方式提供三项（平台 / 日志路径 / 问题描述），二选一、独立不混用。平台取 SA8295、MT8091 或 SA8397（SA8797 按 SA8397 流程处理）：

**命令行**：

```
/audio-debug <SOC平台> <日志文件路径> <问题描述>
```

三项齐全直接开始，缺少任何一项仅追问缺失项：

```
/audio-debug SA8295 /data/logs/audio.log "AudioFlinger underrun"    ← 直接开始
/audio-debug SA8295 /data/logs/audio.log                            ← 追问问题描述
/audio-debug                                                        ← 逐项追问
```

**文件**：三项预填在 `issues/issue_info.txt`（标签 `SOC平台：`/`日志文件：`/`问题描述：`），调用时提示词说明「从文件读取」，Agent 读该文件取三项。另可选填三项背景字段（非必须，留空即可）：

- `补充信息：` — 专业背景/已知约束/怀疑方向，Agent 作上下文参考、非结论，与日志冲突以日志为准
- `活体调试：` — `可用`/`不可用`，不填则分析开始前弹问
- `范围：` — `暂不实施代码修改（仅步骤2-4）`/`全流程`；`暂不实施`止于步骤4，`全流程`或不填到步骤5弹问实施粒度

详细字段格式见 [SKILL.md](SKILL.md)「分析工作流」。

## 首次使用

> **须填源码根路径**：各 `platforms/*_paths.md` 顶部的「源码根路径（GVM Android 侧）」「源码根路径（PVM ... 侧）」当前是占位 `<待填：…>`。请填入你本地对应的源码树根目录——paths.md 表内是相对路径，按侧拼对应根路径才能定位到文件。

## 工作流

分析到修复一条链路（问题分析 → 写修复方案文档 → 多遍审查 → 实施修改 → 改后审查 → 生成 commit message），按文档类型分流：`fix_plan`（根因确定且源码可达）走全流程；`analysis`（根因待证或源码不可达）止于轻量审查。详细步骤、可达性分叉与查无问题处理见 [SKILL.md](SKILL.md)。

## 活体调试（按需）

用 adb 补充日志覆盖不到的现场信息；是否可用在分析开始前一次性问清。操作边界（只读自主 / 改运行参数需 root 授权 / 推库需 remount）见 [SKILL.md](SKILL.md)「活体调试」。

## 模块

各平台模块即对应 paths.md 的章节，源码路径见 [platforms/](platforms/)。

## 项目结构

```
audio-debug/
  SKILL.md              # /audio-debug 入口（指令性工作流 + 标签映射）
  CLAUDE.md             # Claude Code 项目指引
  README.md             # 项目说明

  docs/                 # 设计文档（DESIGN.md）+ 标签映射 TODO
  issues/               # 问题输入文件（issue_info.txt，可预填三项）
  reference/            # 调试参考（架构总览、调试参考类知识文档，按需查）
  output/               # 分析产出
    reports/            # 单次 bug 报告（<soc>_<主题>_<类型>.md）
  platforms/            # 平台参考（源码路径 MD；知识图暂不支持）
```

## 文档

- [设计文档](docs/DESIGN.md) — Agent 驱动架构设计
- [调试参考](reference/) — 架构总览、调试参考类知识文档（按需查）
- [分析产出](output/) — bug 报告（reports/）
- [源码路径参考](platforms/) — 各 SOC 平台音频全栈路径

## 许可证

MIT License - Copyright (c) 2026 kainan
