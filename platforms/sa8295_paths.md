# SA8295 (Android 12L, QCOM) — 源码路径

**作者**: kainan | **日期**: 2026-06-12 | **版本**: 2.0

**源码根路径（GVM Android 侧）**: `<待填：Android 源码树根路径>`
**源码根路径（PVM QNX 侧）**: `<待填：QNX 源码树根路径>`
**源码知识图输出**: 暂不支持

> **访问前提**: 根路径可能因权限或挂载不可达，查询前先检查路径可达性。
> **路径类型**: 相对路径不以 `/` 开头 → 源码树路径。GVM Android 侧路径拼 Android 根路径；PVM QNX 侧路径拼 QNX 根路径。以 `/` 开头 → 设备运行时路径，不拼接。

## 调用栈

数据面（PCM 音频流）：

```
App (AudioTrack/AudioRecord/AudioManager)
    ↓ Java API
Framework Java (android.media)                    ┐
    ↓ JNI                                         │ GVM (Android)
Framework Native (libaudioclient → Binder IPC)    │
    ↓ Binder IPC                                  │
AudioFlinger / AudioPolicy / AAudio Service       ┘
    ↓ HIDL (libaudiohal → DeviceHalHidl)
HIDL HAL → PAL
    ↓ PAL C API (同进程)
PAL → SessionAlsaPcm → tinyalsa PCM → agm_pcm_plugin
    ↓ HwBinder IPC (libagmclient → AGMIPC@1.0)   ┐ AGM IPC daemon
AGM IPC Service → libagm (AGM core)               │ (独立进程,
    ↓ 直接函数调用 (gsl_* → libar-gsl_fe)          │ SCHED_RR prio 99)
GSL FE ───────────────────────────────────────────┘
    ↓ VM 通信 (GVM→PVM 边界)
GSL BE (QNX) → GPR → glink → ADSP
```

控制面（OEM 音频控制，GVM→PVM 跨 VM）：xxx

PVM 本地音数据面：xxx

## GVM (Android) 侧

### Framework 层 (GVM)

#### Audio 类型定义 (公共) (GVM)

| 模块 | 相对路径 | 说明 | 产物 |
|------|----------|------|------|
| audio-base | `system/media/audio/include/system/audio-base-utils.h` | Audio 工具宏 | header (included) |
| audio-base | `system/media/audio/include/system/audio.h` | Audio 设备类型定义 (legacy) | header (included) |
| audio-base | `system/media/audio/include/system/audio-hal-enums.h` | HAL 枚举定义 | header (included) |

#### FW Java (android.media) (GVM)

| 模块 | 相对路径 | 说明 | 产物 |
|------|----------|------|------|
| AudioManager | `frameworks/base/media/java/android/media/AudioManager.java` | 音频管理入口 | framework.jar |
| AudioSystem | `frameworks/base/media/java/android/media/AudioSystem.java` | 系统级音频接口 | framework.jar |
| AudioTrack | `frameworks/base/media/java/android/media/AudioTrack.java` | 音频播放流 | framework.jar |
| AudioRecord | `frameworks/base/media/java/android/media/AudioRecord.java` | 音频录制流 | framework.jar |
| AudioFocusRequest | `frameworks/base/media/java/android/media/AudioFocusRequest.java` | 音频焦点请求 API | framework.jar |
| AudioService | `frameworks/base/services/core/java/com/android/server/audio/AudioService.java` | 系统级音频服务（音量/焦点/设备连接，运行在 system_server） | services.jar |
| MediaPlayer | `frameworks/base/media/java/android/media/MediaPlayer.java` | 媒体播放器 | framework.jar |
| MediaRecorder | `frameworks/base/media/java/android/media/MediaRecorder.java` | 媒体录制器 | framework.jar |

#### JNI (GVM)

| 模块 | 相对路径 | 说明 | 产物 |
|------|----------|------|------|
| libmedia_jni | `frameworks/base/media/jni/android_media_MediaPlayer.cpp` | MediaPlayer JNI | libmedia_jni.so |
| libmedia_jni | `frameworks/base/media/jni/android_media_MediaRecorder.cpp` | MediaRecorder JNI | libmedia_jni.so |
| libandroid_runtime | `frameworks/base/core/jni/android_media_AudioTrack.cpp` | AudioTrack JNI | libandroid_runtime.so |
| libandroid_runtime | `frameworks/base/core/jni/android_media_AudioRecord.cpp` | AudioRecord JNI | libandroid_runtime.so |
| libandroid_runtime | `frameworks/base/core/jni/android_media_AudioSystem.cpp` | AudioSystem JNI | libandroid_runtime.so |

#### FW Native (GVM)

| 模块 | 相对路径 | 说明 | 产物 |
|------|----------|------|------|
| libmedia | `frameworks/av/media/libmedia/mediaplayer.cpp` | MediaPlayer 实现 | libmedia.so |
| libmedia | `frameworks/av/media/libmedia/mediarecorder.cpp` | MediaRecorder 实现 | libmedia.so |
| libmedia | `frameworks/av/media/libmedia/IMediaPlayer.cpp` | MediaPlayer Binder Proxy | libmedia.so |
| libmedia | `frameworks/av/media/libmedia/IMediaRecorder.cpp` | MediaRecorder Binder Proxy | libmedia.so |
| libaudioclient | `frameworks/av/media/libaudioclient/AudioTrack.cpp` | AudioTrack native 实现 | libaudioclient.so |
| libaudioclient | `frameworks/av/media/libaudioclient/AudioRecord.cpp` | AudioRecord native 实现 | libaudioclient.so |
| libaudioclient | `frameworks/av/media/libaudioclient/AudioSystem.cpp` | AudioSystem native 实现 | libaudioclient.so |
| libaudioclient | `frameworks/av/media/libaudioclient/IAudioFlinger.cpp` | AudioFlinger Binder Proxy | libaudioclient.so |
| libaudioclient | `frameworks/av/media/libaudioclient/IAudioPolicyService.cpp` | AudioPolicyService Binder Proxy（CarAudioService 调 AudioPolicyService 的关键通道） | libaudioclient.so |
| libaudioclient | `frameworks/av/media/libaudioclient/AudioPolicy.cpp` | AudioMix 类定义 + Parcel 序列化 | libaudioclient.so |
| libaudioclient | `frameworks/av/media/libaudioclient/AudioTrackShared.cpp` | AudioTrack 共享内存 | libaudioclient.so |
| libaudioclient | `frameworks/av/media/libaudioclient/RecordingActivityTracker.cpp` | 录音活动追踪 | libaudioclient.so |
| libaudioprocessing | `frameworks/av/media/libaudioprocessing/AudioMixer.cpp` | 多流混音 | libaudioprocessing.so |
| libaudioprocessing | `frameworks/av/media/libaudioprocessing/AudioResampler.cpp` | 重采样 | libaudioprocessing.so |
| ServiceUtilities | `frameworks/av/media/utils/ServiceUtilities.cpp` | 录音权限检查 | audioserver |
| libeffects | `frameworks/av/media/libeffects/factory/EffectsFactory.c` | 音效工厂 | libeffects.so |
| libeffects | `frameworks/av/media/libeffects/factory/EffectsXmlConfigLoader.cpp` | 音效 XML 配置加载 | libeffects.so |
| libaaudio | `frameworks/av/media/libaaudio/src/core/AAudioAudio.cpp` | AAudio 核心 | libaaudio.so |
| libaaudio | `frameworks/av/media/libaaudio/src/core/AudioStream.cpp` | 音频流基类 | libaaudio_internal.so |
| libaaudio | `frameworks/av/media/libaaudio/src/core/AudioStreamBuilder.cpp` | 流构建器 | libaaudio_internal.so |
| libaaudio | `frameworks/av/media/libaaudio/src/core/AAudioStreamParameters.cpp` | 流参数 | libaaudio_internal.so |
| libaaudio | `frameworks/av/media/libaaudio/src/client/AudioEndpoint.cpp` | 端点通信 | libaaudio.so |
| libaaudio | `frameworks/av/media/libaaudio/src/client/AudioStreamInternal.cpp` | 内部流实现 | libaaudio.so |
| libaaudio | `frameworks/av/media/libaaudio/src/client/AudioStreamInternalPlay.cpp` | 播放流 | libaaudio.so |
| libaaudio | `frameworks/av/media/libaaudio/src/client/AudioStreamInternalCapture.cpp` | 录制流 | libaaudio.so |

#### audioserver (GVM)

AudioFlinger + AudioPolicyService + AAudioService 运行在 `audioserver` 进程内。AudioControl AIDL HAL Service和 Primary Audio HIDL HAL Service是独立进程，不在 audioserver 内。

##### 入口 (GVM)

| 模块 | 相对路径 | 说明 | 产物 |
|------|----------|------|------|
| audioserver | `frameworks/av/media/audioserver/audioserver.rc` | audioserver init.rc 启动配置 | audioserver |
| audioserver | `frameworks/av/media/audioserver/main_audioserver.cpp` | audioserver 主入口 | audioserver |

##### AudioFlinger (GVM)

| 模块 | 相对路径 | 说明 | 产物 |
|------|----------|------|------|
| AudioFlinger | `frameworks/av/services/audioflinger/AudioFlinger.cpp` | AudioFlinger 主服务 | libaudioflinger.so |
| AudioFlinger | `frameworks/av/services/audioflinger/Tracks.cpp` | Track 管理 | libaudioflinger.so |
| AudioFlinger | `frameworks/av/services/audioflinger/Threads.cpp` | 工作线程 | libaudioflinger.so |
| AudioFlinger | `frameworks/av/services/audioflinger/FastThread.cpp` | Fast 线程框架 | libaudioflinger.so |
| AudioFlinger | `frameworks/av/services/audioflinger/FastMixer.cpp` | Fast Mixer | libaudioflinger.so |
| AudioFlinger | `frameworks/av/services/audioflinger/FastCapture.cpp` | Fast Capture | libaudioflinger.so |
| AudioFlinger | `frameworks/av/services/audioflinger/AudioStreamOut.cpp` | 输出流 | libaudioflinger.so |
| AudioFlinger | `frameworks/av/services/audioflinger/AudioHwDevice.cpp` | HAL 设备管理 | libaudioflinger.so |
| AudioFlinger | `frameworks/av/services/audioflinger/Effects.cpp` | 音效管理 | libaudioflinger.so |
| AudioFlinger | `frameworks/av/services/audioflinger/DeviceEffectManager.cpp` | 设备级音效管理 | libaudioflinger.so |
| AudioFlinger | `frameworks/av/services/audioflinger/PatchPanel.cpp` | 音频路由 Patch 管理 | libaudioflinger.so |

##### AudioPolicy (GVM)

| 模块 | 相对路径 | 说明 | 产物 |
|------|----------|------|------|
| AudioPolicy | `frameworks/av/services/audiopolicy/service/AudioPolicyService.cpp` | AudioPolicy 主服务 | libaudiopolicyservice.so |
| AudioPolicy | `frameworks/av/services/audiopolicy/service/AudioPolicyInterfaceImpl.cpp` | AudioPolicy Binder 接口实现 | libaudiopolicyservice.so |
| AudioPolicy | `frameworks/av/services/audiopolicy/service/AudioPolicyEffects.cpp` | AudioPolicy 音效管理 | libaudiopolicyservice.so |
| AudioPolicy | `frameworks/av/services/audiopolicy/service/Spatializer.cpp` | 空间音频管理 | libaudiopolicyservice.so |
| AudioPolicy | `frameworks/av/services/audiopolicy/managerdefault/AudioPolicyManager.cpp` | 策略决策引擎 | libaudiopolicyservice.so |
| Audio Policy Config (标准) | `vendor/qcom/opensource/audio-hal-ar/primary-hal/configs/common_au/audio_policy_configuration.xml` | 标准 audio policy（bus device/mix port/route） | (config) |
| Audio Policy Config (设备) | `/vendor/etc/audio_ar/audio_policy_configuration.xml` | QCOM Audio Policy 配置（设备运行时路径） | (config) |

##### AAudio (GVM)

| 模块 | 相对路径 | 说明 | 产物 |
|------|----------|------|------|
| AAudio | `frameworks/av/services/oboeservice/AAudioService.cpp` | AAudio 服务入口 | libaaudioservice.so |
| AAudio | `frameworks/av/services/oboeservice/AAudioServiceStreamShared.cpp` | 共享流 | libaaudioservice.so |
| AAudio | `frameworks/av/services/oboeservice/AAudioServiceStreamMMAP.cpp` | MMAP 流 | libaaudioservice.so |
| AAudio | `frameworks/av/services/oboeservice/AAudioServiceEndpoint.cpp` | 端点基类 | libaaudioservice.so |
| AAudio | `frameworks/av/services/oboeservice/AAudioServiceEndpointPlay.cpp` | 播放端点 | libaaudioservice.so |
| AAudio | `frameworks/av/services/oboeservice/AAudioServiceEndpointCapture.cpp` | 录制端点 | libaaudioservice.so |
| AAudio | `frameworks/av/services/oboeservice/AAudioServiceEndpointMMAP.cpp` | MMAP 端点 | libaaudioservice.so |
| AAudio | `frameworks/av/services/oboeservice/AAudioServiceEndpointShared.cpp` | 共享端点 | libaaudioservice.so |
| AAudio | `frameworks/av/services/oboeservice/AAudioClientTracker.cpp` | 客户端追踪 | libaaudioservice.so |
| AAudio | `frameworks/av/services/oboeservice/AAudioStreamTracker.cpp` | 流追踪 | libaaudioservice.so |
| AAudio | `frameworks/av/services/oboeservice/AAudioMixer.cpp` | AAudio 混音 | libaaudioservice.so |
| AAudio | `frameworks/av/services/oboeservice/AAudioThread.cpp` | AAudio 线程 | libaaudioservice.so |

> SA8295 QCOM automotive 平台 MMAP 模式适用性待确认。构建配置仅在 `PRODUCT_NAME == msmnile_gvmgh` 时设 `aaudio.mmap_policy=1`（NEVER），SA8295 其他 PRODUCT_NAME 可能未设此属性，默认为 AUTO（尝试 MMAP 后回退 Legacy）。SA8295 无 Linux ALSA kernel 驱动（`BYPASS_ALSA_HW`），MMAP 依赖的 `/dev/snd/` 设备节点不可达，实际效果应为回退到 Legacy 共享内存模式。

### Automotive 控制面 (GVM)

SA8295 控制流路径（GVM 侧）：xxx

#### CarAudio (GVM)

| 模块 | 相对路径 | 说明 | 产物 |
|------|----------|------|------|
| CarAudioService | `packages/services/Car/service/src/com/android/car/audio/CarAudioService.java` | 车载音频服务 | CarService.apk |
| CarAudioFocusManager | `packages/services/Car/service/src/com/android/car/audio/CarAudioFocusManager.java` | 焦点管理（焦点申请/放弃/ducking 触发） | CarService.apk |
| CarAudioVolumeManager | `packages/services/Car/service/src/com/android/car/audio/CarAudioVolumeManager.java` | 音量管理（group volume/attenuation/mute） | CarService.apk |
| CarAudioZone | `packages/services/Car/service/src/com/android/car/audio/CarAudioZone.java` | 音频区域模型（volume group/设备绑定） | CarService.apk |
| CarAudioContext | `packages/services/Car/service/src/com/android/car/audio/CarAudioContext.java` | usage↔context 映射 | CarService.apk |
| CarAudioZonesHelper | `packages/services/Car/service/src/com/android/car/audio/CarAudioZonesHelper.java` | context↔device address 映射 | CarService.apk |
| CarAudio 配置 (前排) | `vendor/qcom/opensource/audio-hal-ar/primary-hal/configs/common_au/car_audio_configuration.xml` | context↔device address 配置（前排 DHU，单/双 DHU 均使用） | (config) |
| CarAudio 配置 (后排) | `vendor/qcom/opensource/audio-hal-ar/primary-hal/configs/common_au/car_audio_configuration_rear.xml` | context↔device address 配置（后排 DHU，仅双 DHU 车型使用） | (config) |

