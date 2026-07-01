# SA8397 / SA8797 (Android, QCOM + Gunyah) — 源码路径

> **占位骨架**。SA8397 源码路径需按实际源码树核实后填充，**勿凭空编造**。
> 参照 `sa8295_paths.md` 的结构与粒度补全。SA8797 按 SA8397 流程处理。

**作者**: kainan | **日期**: 待填 | **版本**: 0.1（骨架）

**源码根路径（GVM Android 侧）**: `<待填>`
**源码根路径（PVM 侧）**: `<待填>`

> **访问前提**: 根路径可能因权限或挂载不可达，查询前先检查路径可达性。
> **路径类型**: 相对路径不以 `/` 开头 → 源码树路径；以 `/` 开头 → 设备运行时路径。

## 调用栈

```
<待填：SA8397 数据面/控制面调用栈（含 Gunyah hypervisor 边界）>
```

## GVM (Android) 侧

### · gvm_qcom_aidl — QCOM AIDL HAL

- `AHAL_*_QTI`：Module / Stream / ModulePrimary / StreamIn / StreamOut / Platform / CoreService 等
- 源码路径：`<待填>`

### · gvm_qcom_pal — QCOM PAL

- `PAL: <Name>`：API / ResourceManager / USB / SndMonitor 等
- 源码路径：`<待填>`

## PVM 侧

<待填>

## ADSP / Gunyah

> SA8397 虽有 ADSP/Gunyah，但 paths.md 暂未索引 foundation 层。

<待填：如需索引再补>
