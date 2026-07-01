# MT8091 (Android, MTK) — 源码路径

> **占位骨架**。MTK 侧源码路径需按实际源码树核实后填充，**勿凭空编造**。
> 参照 `sa8295_paths.md` 的结构与粒度补全。

**作者**: kainan | **日期**: 待填 | **版本**: 0.1（骨架）

**源码根路径（GVM Android 侧）**: `<待填>`
**源码根路径（PVM 侧）**: `<待填>`

> **访问前提**: 根路径可能因权限或挂载不可达，查询前先检查路径可达性。
> **路径类型**: 相对路径不以 `/` 开头 → 源码树路径；以 `/` 开头 → 设备运行时路径。

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

## PVM 侧

<待填>

## Foundation / ADSP 固件层（仅 MT8091）

<待填：ADSP / IPI 驱动相关路径>
