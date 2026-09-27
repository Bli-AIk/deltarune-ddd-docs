# 0002 · 曲库转码格式选 Ogg Vorbis 而非 Opus

- 状态：已定
- 日期：2026-09-27

## 背景

母带是 126 个无损 FLAC（44.1kHz/16bit 立体声、~813kbps、4.35GB、9.96 小时）。需要一份压缩件：
既给 Obsidian 分镜踩点听，也允许游戏内**临时复用**同一批文件。目标：尽量保音质、尽量省盘
（本机 460G 盘已用 95%）。

## 备选方案

| 方案 | 体积（101 首 / 7.51h） | 能否进游戏 |
|---|---|---|
| 保留 FLAC | ~3.4 GB | 可以，但太占 |
| **Ogg Vorbis `-q:a 5`（VBR≈160k）** | **实测 531 MiB** | ✅ |
| Ogg Vorbis q4 / q6 | 413 / 619 MiB | ✅ |
| Opus 128k | ~574 MB | ❌ |
| Opus 96k | ~430 MB | ❌ |

## 决定

Ogg Vorbis，`ffmpeg -c:a libvorbis -q:a 5`，保持 44.1kHz / 立体声，不重采样。

## 理由

- **Opus 被硬性排除**：本机 LÖVE 11.5 只链了 `libvorbisfile / libogg / libmpg123 / libmodplug`，
  **没有 opus 解码器**。Kristal 跑在 LÖVE 上，选 Opus 就等于放弃"游戏内复用"。
- Vorbis 一次转码两边通吃（Obsidian 是 Electron，Vorbis 也能播），不必日后从 Opus 二次有损重转。
- q5 压到母带的 1/8，531 MiB 在 21G 可用空间下无压力。

## 后果

- ⚠️ 比 Opus 方案大约 25% 体积，这是换兼容性的代价，已接受。
- 日后换成编曲版（调研里的既定方向）时，曲库这份原曲只做参考，不进公开发布物。
- 不嵌封面、不搬 `.cue`、不搬专辑扫图；`-map 0:a:0` 丢掉 FLAC 内嵌的封面图流。
