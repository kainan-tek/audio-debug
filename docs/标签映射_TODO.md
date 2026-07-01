# 日志标签 → 模块映射 TODO

**作者**: kainan | **更新**: 2026-07-01

`SKILL.md` 末尾「日志标签 → 模块映射」表的补充/待核实项。

## 待补标签

- [ ] 补充 SA8397 / SA8797 厂商特有标签（Gunyah / ADSP 相关）
- [ ] 核实 MT8091 Foundation/ADSP 层标签（dmesg / ADSP trace 非前缀格式）
- [ ] QNX slog 相关标签的 module_id 归属

## 待核实

- [ ] AHAL 系列同名异义在各平台的实际触发条件
- [ ] `AGMCallback` IPC 客户端 tag 的完整族成员

## 已知限制

- 标签表只是找路径的途径之一；未登记模块（内核/固件/QNX 底层）直接读 paths.md 对应章节，未登记不等于找不到路径。
