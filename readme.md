## 许可证

CC0 或 CC BY-NC-SA 4.0 任选其一。

有什么问题吗？

---

## 这个库怎么用

直接当 Obsidian 库打开本目录即可：`designs/` 是分章设计稿，`images/` 是分镜图，`tasks/` 是任务板。

`designs/<章>-<缩写>/<缩写>-<n>.md` 与曲库里的 `<缩写>-<n>.ogg` **同号对应**（序号 0 基 = 碟内 track 号 − 1）：

```
designs/1-GFC/GFC-0.md   ↔   music/GFC-0.ogg     （蓮台野夜行 第 1 轨）
designs/1-GFC/GFC-2.md   ↔   music/GFC-2.ogg     （蓮台野夜行 第 3 轨）
```

## 关于 `music/`：一个**私有的可选子模块**

`music/` 是私有仓库 [`Bli-AIk/deltarune-ddd-music`](https://github.com/Bli-AIk/deltarune-ddd-music) 的子模块，
里面是 ZUN「秘封俱乐部」系列（ZUN's Music Collection）专辑的 Ogg Vorbis 转码件。

**这些曲目有版权，不能公开分发**，所以本仓库的 `.gitmodules` 给这条挂了 **`update = none`**：

- 外部 `git submodule update --init --recursive`、CI 的 `actions/checkout` + `submodules: recursive`、
  发布打包 —— 都会**静默跳过**它：不报错，也不会把曲子带进公开 release。
- 因此**公开 clone 拿不到 `music/` 的内容** —— 这是设计如此，不是坏掉。
- 作者本地想听分镜音频，显式开一次即可：

  ```bash
  git config submodule.music.update checkout
  git submodule update --init music
  ```

⚠️ 公开发布物里永远不会有这些文件，所以将来在游戏里加载 BGM 时**必须**先做存在性检查
（`love.filesystem.getInfo`），缺失就静音 + 记日志，否则发布版会崩或静音。

设计口径（为什么用 Vorbis 而不是 Opus、命名与序号怎么定、为什么不进公开仓库）见 `adr/` 与 `glossary.md`。
