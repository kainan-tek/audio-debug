# MT8091 (Android, MTK) — 源码路径

> **占位骨架**。MTK 侧源码路径需按实际源码树核实后填充，**勿凭空编造**。
> 参照 `sa8295_paths.md` 的结构与粒度补全。

**作者**: kainan | **日期**: 2026-06-10 | **版本**: 2.0

**源码根路径（GVM Android 侧）**: `<待填>`
**源码根路径（PVM Linux 侧）**: `<待填>`
**源码根路径（Foundation 侧）**: `<待填>`
**源码知识图输出**: 暂不支持

> **访问前提**: 根路径可能因权限或挂载不可达，查询前先检查路径可达性。

> **源码路径查找规则**
>
> - 侧根 = 文件头对应侧「源码根路径」（绝对路径）；行内 = 表格「相对路径」列；prefix = 节内 `> **prefix**:` 注（无则跳过）。
> - 绝对路径 = 侧根 + prefix + 行内。
> - 行内以 `/` 开头 → 设备运行时路径，不拼。

> **分支差异**: 表中路径基于主分支统计；不同分支可能重构/改名/增删文件，与本文不一致时在该分支自行定位。

## 调用栈

```
<待填：MT8091 数据面/控制面调用栈>
```

## GVM (Android) 侧

### · gvm_mtk_aidl — MTK AIDL HAL

- `AHAL_Module` / `AHAL_Stream` / `AHAL_ModulePrimary` / `AHAL_StreamAlsa` / `AHAL_ModuleAlsa` / `AHAL_Bluetooth` / `AHAL_Config`
- 源码路径：`<待填>`

### · gvm_mtk_mal — MTK AIDL MAL 适配层

- `MTKAdapterLayer` / `MTKAidlConversion` / `MTKAdapterStreamLayer` + `mtk_audio_hw_hal`（aud_drv 原生 HAL）
- 源码路径：`<待填>`

## PVM（Linux）侧

<待填>

## Foundation / ADSP 固件层（仅 MT8091）

<待填：ADSP / IPI 驱动相关路径>
