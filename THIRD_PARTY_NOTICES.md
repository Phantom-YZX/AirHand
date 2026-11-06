# Upstream components

This distribution repository does not publish the application source code or grant an open-source license to the application. Upstream components retain their respective copyrights and licenses.

| Component | Version | Upstream / license |
| --- | --- | --- |
| AndroidX CameraX | 1.5.3 | Google / Apache License 2.0 |
| AndroidX Lifecycle | 2.9.4 | Google / Apache License 2.0 |
| MediaPipe Tasks Vision | 0.10.32 | Google / Apache License 2.0 |
| Kotlin standard library | resolved by AGP built-in Kotlin | JetBrains / Apache License 2.0 |
| JUnit (tests only, not packaged in app) | 4.13.2 | Eclipse Public License 1.0 |

Apache License 2.0 text is included in `THIRD-PARTY-APACHE-2.0.txt`. Dependency archives can contain further notices applicable to their transitive components.

## Pretrained model

- Name: MediaPipe Hand Landmarker, float16, version 1
- Original file: `hand_landmarker.task`, unmodified, 7,819,105 bytes
- [Official model download](https://storage.googleapis.com/mediapipe-models/hand_landmarker/hand_landmarker/float16/1/hand_landmarker.task)
- [Official overview and model cards](https://developers.google.cn/edge/mediapipe/solutions/vision/hand_landmarker/index)
- SHA-256: `fbc2a30080c3c557093b5ddfc334698132eb341044ccee322ccf8bcf3607cde1`

The official Hand Tracking model card linked from the Hand Landmarker model table identifies Apache License 2.0 on page 2: https://storage.googleapis.com/mediapipe-assets/Model%20Card%20Hand%20Tracking%20%28Lite_Full%29%20with%20Fairness%20Oct%202021.pdf . This is the model-specific licensing basis; no weights were trained or changed by this project.

## Packaged notice inventory (0.1.18)

The build resolves the runtime classpath and generates `assets/legal/index.json`, per-component documents, original archive notices/POMs and SHA-256 hashes. The NPU build currently lists 55 components and 62 offline documents. This includes transitive Protobuf, Guava, Flogger, DataTransport, Kotlin/AndroidX and annotation dependencies. Artifact POM declarations and additional reviewed upstream texts are preserved rather than replacing all notices with a generic license. Checker annotations use their MIT option; the upstream explanatory license context is included. Animal Sniffer annotations retain the MIT copyright and conditions. JSR305 retains the POM-declared Apache terms and the included JCIP annotation attribution to Brian Goetz under CC-BY-2.5.

Original Qualcomm `QNN_NOTICE.txt` (also distributed as identical `NOTICE.txt` in SDK 2.47) is preserved unchanged, SHA-256 `c8a22fa8b9ed3e9266c4a3366965474cef5cb147ee2a1f33e488bbb24a071c09`. Qualcomm agreement PDF and a reading text are included; it is not relicensed by this project. LiteRT AAR license and third-party notice texts are preserved verbatim. Complete applicable license and notice documents are included in the APK and can be read offline in About software → Third-party technology and complete licenses.

The inventory covers resolved JVM artifacts plus the explicitly integrated model/LiteRT/QNN material. It does not claim a complete build-level provenance reconstruction of every statically linked native subcomponent, patent clearance, or trademark clearance.

## Runtime privacy

The optional `-Pairhand.npu=true` experiment additionally packages LiteRT 2.2.0 (Google, Apache 2.0) and Qualcomm AI Runtime 2.47.0.260601 libraries for HTP V81, obtained from their official distributions. Original license/third-party notices and the Qualcomm SDK license PDF are packaged in its assets. Qualcomm libraries are distributed only within the application; this repository does not distribute standalone SDK binaries. Unmodified palm and landmark TFLite files are extracted from the model bundle above. `HandRegionGeometry` and the experimental crop/ROI pipeline follow MediaPipe's documented SSD anchors, rotation conventions and region transformation settings; upstream MediaPipe code is Apache 2.0. The optional public-image diagnostic uses `https://storage.googleapis.com/mediapipe-assets/woman_hands.jpg`, downloaded separately for local testing and not redistributed in the APK .

AirHand does not initialize analytics, advertising or account services, and its manifest grants no network permission. Model loading uses the packaged asset. RGB frames are converted and released on the camera analysis thread and are not written to disk.

MediaPipe's distributed Tasks Core AAR includes a remote performance logger and transitive DataTransport classes. AirHand's build uses an explicit ASM transform on `TasksStatsLoggerFactory.create` to select MediaPipe's existing `TasksStatsDummyLogger` instead of `TasksStatsProtoLogger`. This is a local packaging modification, documented in `app/build.gradle.kts`; the dependency cache and inference classes remain unchanged. The merged manifest excludes Internet/network-state permissions and DataTransport scheduler components. Transitive library classes may still be present but the logger factory does not initialize their transport runtime. This was checked statically, not by observing a device network trace.
