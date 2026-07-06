# adb 活体诊断速查与证据处置

活体调试用 adb 补充日志覆盖不到的现场信息。本文件列出操作边界、诊断命令清单与证据处置规则；流程入口见 SKILL.md「活体调试」章节。

## 操作边界（按权限分层）

- 只读诊断（dumpsys / logcat 等）：自主执行，无需 root
- 重启服务（killall audioserver 等）/ 修改运行参数（setprop、settings put 等）：需用户授权 + **root**（`adb root` 会重启 adbd、也是状态变更），在主对话走授权，不外包给子 agent
- 推库 / 改系统文件：需用户协助 + **remount**

## 证据处置

- 跑诊断时若 adb 不可用 / 设备掉线 / 命令失败：告知用户一次并停用活体诊断、继续分析，不重试、不再次追问。
- 活体诊断取的是**当前**设备状态快照，与日志记录的**故障时刻**未必一致（设备可能已重启、问题未必在复现）——只作旁证交叉印证，不当故障时刻的直接证据，除非问题正在现场复现。

## 只读诊断命令

只读诊断（普通 adb shell，无需 root；可派子 agent 跑，避免大输出灌爆主上下文，子 agent 须保留关键原始行作证据）：

- `adb shell dumpsys media.audio_flinger` — AudioFlinger 运行时状态（Track/线程/缓冲区健康度）
- `adb shell dumpsys media.audio_policy` — AudioPolicy 策略与路由（可用设备/路由决策/AudioPatch）
- `adb shell dumpsys car_service | grep -A50 CarAudioZone` — CarAudio 车载音频配置（音区/设备/音量组）；CarAudio 是 car_service 子段，grep 定位避免全量 dump 灌爆上下文
- `adb shell logcat -b all -d -s AudioFlinger APM_AudioPolicyManager CarAudio AudioSystem AudioTrack` — 近期音频日志
- 其他无副作用的诊断命令（按需选用，不必全跑）
