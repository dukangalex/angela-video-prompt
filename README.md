# Angela Video Prompt（帝江）

AI 视频生成的提示词工作流（自有版）：写提示词 → 生成 → 多段拼接成长片 → 抽帧验证 → 案例沉淀。

解决的是 AI 短视频真正的难点：不是点下「生成」，而是提示词写得准、长片拼得连贯、人物跨段不变样、每轮结果可验证。

## 工作流

1. **写提示词**：按 `references/prompt-template.md` 的六段式模板写（时长画幅／人物／场景／带时间轴的节拍／镜头语言／声音对白／限制清单），另有 POV 生活流、口播、多段长片三个变体。提示词以用户定稿为准，原样传入生成端，不擅自改写。
2. **生成**：纯文本优先；参考图只给风格与大概气质，不承诺像谁（见能力边界）。
3. **长片（>10 秒）**：单段约 10 秒是硬限。按节拍拆段、逐段独立提示词、用上一段的快照 ID 接续，最后 ffmpeg 无损拼接。见 `references/stitch-workflow.md`。
4. **验证**：每段生成后用 `bin/extract_frames.py` 抽帧自查——哪些吃进了提示词、哪些偏了、偏差原因，写成结论。音频、口型、动态连贯性机器看不见听不见，必须明说、交用户终验。
5. **沉淀**：每个实验写一张案例卡进 `references/cases/`（提示词要点／结果／偏差／教训），失败案例同样入册。

## 能力边界（先说清，不 oversell）

- 内置视频类生成端的人物锁脸弱：参考图人物可能不像，强锁脸指令只能改善、不能解决。需要固定真人形象的，走带参考图锁脸能力的第三方模型或角色资产方案，本工作流不承诺。
- 快照接续只能传递上下文与风格延续的概率，不等于锁脸；多人物群戏是长片一致性的重灾区。最稳的组合是：单人物 + 简单镜头 + 人物描述逐字复用 + 抽帧检查 + 接受重抽。
- 详细实测边界见 `references/model-notes.md`（源自上游 2026-10 的实测记录，引用前以自家复测为准）。

## 部署配置

本仓库是环境中立的工作流本体：对白语言与口音、输出目录、生成通道、审片方式这类随项目而定的选择，一律写在 `DEPLOYMENT.md`（部署配置）里，不写死进模板与流程。各项目在自己的部署配置里填自己的值。

## 文件

- `SKILL.md` — 技能本体（路由与流程）
- `CONSTITUTION.md` — 五条设计原则
- `DEPLOYMENT.md` — 部署配置模板
- `references/prompt-template.md` — 六段式模板与变体
- `references/consistency.md` — 人物一致性实战策略
- `references/dialogue.md` — 对白写法规范
- `references/stitch-workflow.md` — 分段生成 → 拼接 → 验证
- `references/model-notes.md` — 生成端能力边界实测
- `references/integration.md` — 与镜头设计、审片环节的衔接方法
- `references/cases/` — 案例库（核心资产，随用随长）
- `bin/extract_frames.py` — ffmpeg 抽帧自查脚本（纯本地，无联网）

## 致谢与许可

- 本项目自有化自 masonmosen 的 [muse-video-prompt-skill](https://github.com/masonmosen/muse-video-prompt-skill)（MIT），六段式模板与工作流骨架源自该仓库的实战沉淀；其六段式思想又源自 LuoJiangYong 的 [muse-video-skill](https://github.com/LuoJiangYong/muse-video-skill)（MIT），一并致谢。
- 自有化处理：剥离原作者的个人默认项（如对白口音偏好）进部署配置，文档按自有口径重写，与上游脱钩独立维护。
- 许可证：MIT，原文见 `LICENSE`（含原作者版权声明）。
