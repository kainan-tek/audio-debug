# audio-debug Skill — Claude Code 项目指引

本目录是 `audio-debug` Skill（车载 AAOS 音频日志分析与调试）。Claude Code 在本目录或被本 Skill 触发时遵循此指引。

## Skill 入口

- `SKILL.md` —— `/audio-debug` 指令性工作流 + 日志标签→模块映射。**以 SKILL.md 为准**，本文件仅给项目级约定。
- 触发：用户提供日志文件路径、请求日志分析、询问音频故障时自动触发；或显式 `/audio-debug`。

## 路径约定

本 Skill 内所有相对路径（`platforms/`、`reference/`、`issues/`、`output/`、各 `paths.md`）一律相对本 Skill 自身目录 `${CLAUDE_SKILL_DIR}` 解析，与会话 cwd 无关。任意目录启动 `claude` 均可正常工作。

## 目录职责

| 目录 | 用途 | 写入时机 |
|------|------|----------|
| `issues/` | 问题输入（`issue_info.txt` 预填三项，另三项选填） | 用户预填 |
| `platforms/` | 各 SOC 平台源码路径索引（`<soc>_paths.md`） | 维护，已建 |
| `reference/` | 架构总览 / 调试参考类知识文档，按需查 | 维护，按需补充 |
| `output/reports/` | 单次 bug 报告（`<主题>_<类型>.md`） | `/audio-debug` 步骤 2 产出 |
| `docs/` | 设计文档（`DESIGN.md`）+ 标签映射 TODO | 维护 |

## 工作准则

- **日志驱动**：主线是 log→根因。分析范围 = 报告现象的时间窗，不假装扫整条 boot。
- **平台一致性先行**：以行首主体标签核对 SOC 平台，矛盾先问用户。
- **源码可达性定模式**：可达给文件路径 + 实际行号；不可达给 paths.md 路径 +「源码未验证」，不硬凑。
- **改代码必须经用户同意**：步骤 4 起，AskUserQuestion 征得明确同意才动真实源码；不顺手改周边。
- **不编造**：paths.md 未登记的路径用 Glob 在源码树定位，找不到标「源码未验证」，不猜路径。
