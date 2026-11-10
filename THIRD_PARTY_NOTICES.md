# 开源来源与修改说明

本项目以 GPL-3.0-only 提供。许可证全文见 LICENSE。不是 Trancy、沉浸式翻译或 Amazon 官方产品。

## KISS Translator — fishjar 及贡献者

- 项目：https://github.com/fishjar/kiss-translator
- 参考文件：https://github.com/fishjar/kiss-translator/blob/dev/src/libs/pool.js
- 获取时间：2026-11-05；原始快照保存在 vendor/kiss-pool.original.js。
- 许可证：GPL-3.0。
- 实际改编到 extension/lib/pool.js：去掉上游配置／诊断依赖，设置本项目并发参数，增加优先级排序；保留取消、并发控制和重试逻辑。
- 已阅读 domManager.js，但没有复制其 React 实现；亚马逊 DOM 适配、UI、缓存、后台服务为本项目编写。

## BilingualTube — rxliuli 及贡献者

- 项目：https://github.com/rxliuli/bilingualtube
- 参考文件：https://github.com/rxliuli/bilingualtube/blob/main/lib/subtitles/cues-utils.ts
- 获取时间：2026-11-05；原始快照保存在 vendor/bilingualtube-cues.original.ts。
- 许可证：GPL-3.0。
- 实际改编到 extension/lib/core.js：从播放点预取至多 10 条，增加 45 秒窗口、排除请求中字幕，改为标准起止时间匹配。
- 阅读了 Google provider，但本项目没有使用该实现里的内置共享 API Key；使用免 Key 的 gtx 网页接口。

修改日期：2026-11-05。0.2.0 新增 Amazon camelCase 评论适配、模块识别、整轨预译、双层缓存与媒体文稿导出。分发包包含可编辑的运行源码、许可证和此说明。


## 0.4.0 图片 OCR

- Tesseract.js 7.0.0： https://github.com/naptha/tesseract.js ，Apache-2.0；使用原始浏览器 API 与 Worker 构建产物。
- tesseract.js-core 7.0.0： https://github.com/naptha/tesseract.js-core ，Apache-2.0；打包普通／SIMD／Relaxed SIMD 的 LSTM Wasm 核心与加载脚本，保留设备兼容选择。
- @tesseract.js-data/eng 1.0.0 的 4.0.0_best_int/eng.traineddata.gz： https://github.com/naptha/tessdata ，上游训练数据为 Apache-2.0。npm 包元数据声明包装为 MIT，本项目只复制训练数据文件。
- 上游许可证、Worker 的依赖授权说明以及实际版本清单保存在 extension/vendor/ocr/。
- scripts/vendor-ocr.mjs 从锁定 npm 依赖复制这些资源；识别调度、图片读取、UI、缓存与文本翻译接入为本项目代码。没有改动上述第三方构建文件。

## 0.7.0 媒体保存流程参考：猫抓（Cat Catch）

- 项目：https://github.com/xifangczy/cat-catch
- 审阅快照：https://github.com/xifangczy/cat-catch/tree/2f84456f86230a88bbc8c67ba9ea15fcd64635d6
- 上游许可证：GPL-3.0；本项目继续采用 GPL-3.0-only。
- 读取了 js/background.js（资源分类与标签页归属）、js/m3u8.js（播放列表解析与独立下载页）、js/downloader.js（任务、取消与进度）以及 catch-script/search.js（页面媒体发现）。
- 本项目据这些流程编写精简的媒体保存实现：仅响应用户右键选择，读取对应播放器公开源，有限并发下载并按顺序合并非加密 HLS 点播。没有复制其全站请求 Hook、密钥捕获或线上 FFmpeg 实现，没有把猫抓整包作为依赖。媒体模块为本项目重写，非上游文件的逐行改编。
- 审阅与编写日期：2026-11-06。

## 0.7.0 本地 MP4 转换：FFmpeg.wasm

- `@ffmpeg/ffmpeg` 0.12.15：MIT，Copyright (c) 2019 Jerome Wu；`@ffmpeg/core` 0.12.10 单线程 WebAssembly：GPL-2.0-or-later，包含 FFmpeg 和第三方编解码库。API 的 MIT 许可不替代 core 的 GPL 许可。
- 精确上游发布：[v12.15](https://github.com/ffmpegwasm/ffmpeg.wasm/releases/tag/v12.15)，提交 `71aa99d37c02a7b4c435275ca9ef50e612f6efa1`。该发布明确对应 API 0.12.15 / core 0.12.10；并非旧标签 v0.12.10。
- [完整上游源码](https://github.com/ffmpegwasm/ffmpeg.wasm/archive/71aa99d37c02a7b4c435275ca9ef50e612f6efa1.tar.gz) 包含 API、绑定、修改过的命令行源码和依赖构建脚本；该版本 Dockerfile 选择 FFmpeg n5.1.4、Emscripten SDK 3.1.40，启用 libx264、libx265 等 GPL 组件。
- 原样复制已锁定 npm 包的 ESM API、单线程 core JS/Wasm，保存在 `extension/vendor/ffmpeg/`；许可证、编解码库版权说明、精确发布来源、构建脚本参考和文件哈希均保存在该目录。未改动第三方运行文件；转封装、转码、取消和校验逻辑为本项目代码。
- `scripts/vendor-media.mjs` 保留上述许可与来源文件并校验锁定版本。运行时完全使用扩展内资源，不从 CDN 运行脚本，不上传视频。当前产物未做逐字节重现构建；上游若干依赖使用分支名，详细来源边界见该目录 README。

## 0.8.5 合并字幕的本地中文字体

- Noto Sans SC：[Google Fonts 源码快照](https://github.com/google/fonts/tree/2894aab31764f10f29c421bdfd2340d3b382d384/ofl/notosanssc)，SIL Open Font License 1.1，Copyright 2014–2021 Adobe。原始许可证随字体保存在 `extension/vendor/fonts/OFL.txt`。
- 从上游可变字体生成 `wght=400` 静态字重，保留全部上游字形，未按示例文本裁剪。字体仍按 OFL 分发。开发转换脚本为 `scripts/vendor-subtitle-font.py`；原文件和输出文件哈希见 `extension/vendor/fonts/VERSIONS.json`。
- 字体只供本机 FFmpeg 将字幕绘制到视频画面使用，运行时无需联网下载字体。
