<!-- readme:hero -->

> **中文配音版**（在原仓库基础上改造，MIT 协议，可自由修改分发）
> 原仓库：https://github.com/kaomei/stickman-video-director
> 改动：① 旁白由英文改为**中文普通话**（240–300 字 / 约 60 秒），去掉"英文文案 + 参考译文"双栏；
> ② 生成提示词采用**指令层英文 + 台词层中文**的双语策略（模型理解稳、台词不出错）；
> ③ 补充「本机出片路径」：画面用 VideoGen / MiniMax H3，配音用 edge-tts `zh-CN-XiaoxiaoNeural`
> 或 apiz `minimax/t2a` 的 `female-yujie`，ffmpeg 合成时画面与音频一起加速。

---

# 三步开始用

## 第 1 步 · 复制文件夹

把整个文件夹（里面的 `SKILL.md`、`references`、`agents` 一起）复制到你的 WorkBuddy 技能目录：

| 系统 | 技能目录 |
|---|---|
| Windows | `C:\Users\<你的用户名>\.workbuddy\skills\` |
| macOS / Linux | `~/.workbuddy/skills/` |

复制后把文件夹**重命名为 `stickman-video-director`**（去掉"-分享版"后缀）。

最终应该能看到这个文件：
`...\.workbuddy\skills\stickman-video-director\SKILL.md` ← 有它就装好了

## 第 2 步 · 确认依赖（大部分人可以直接跳过）

这是**纯提示词技能**：装完立刻能出分镜提案和视频提示词，**不需要装任何东西**。

只有要"真正把视频渲染出来"时，才需要下面这些，缺哪个补哪个：

| 用途 | 需要什么 | 没有会怎样 |
|---|---|---|
| 生成画面 | WorkBuddy 自带的视频生成功能，或你自备的视频模型 API（Veo / MiniMax H3 / 可灵等） | 只能拿到 6 条提示词，需自己粘到别的平台生成 |
| 中文配音 | `edge-tts`（`pip install edge-tts`，免费）或任意 TTS 服务 | 没有声音，只能出无声视频 |
| 拼接合成 | `ffmpeg` | 6 段素材无法合成一整支 |

> 提示：上面这些路径/版本不必照抄，按你自己机器的实际情况来就行。

## 第 3 步 · 说一句话测试

在 WorkBuddy 里直接说：

> 用火柴人视频，把"拖延不是懒，是情绪在报警"做成 9:16 竖屏、黑底白线的励志短片

它会先跟你确认三件事（**素材 / 画幅 / 浅色还是深色主题**），然后给出**分镜提案并停下来等你点头**。
这不是卡住了，是设计好的闸门——确认后它才输出 6 条提示词。

---

# 你说什么，它做什么

| 你说 | 它做 |
|---|---|
| "用火柴人视频做这个主题……9:16 竖屏，黑底白线" | 出一份完整分镜提案（标题/钩子/6 场画面/中文旁白/配色），等你确认 |
| "第三场换个比喻" | 只改那一场，重出提案再确认 |
| "改成白底黑线" | 全局改主题 → 作废之前的批准，整套重做 |
| "确认，出提示词" | 输出 6 条可直接喂给视频模型的独立提示词 + 缝合指南 |
| "帮我配好音" | 用中文旁白生成配音（默认 edge-tts 晓晓音色） |
| "把这段文案做成视频"（什么都没说清） | 一次性把缺的三样问全，然后停下等你回答 |

---

# 常见问题

**它为什么不直接给我视频？**
它是「导演」不是「摄影机」。分工是：它保证分镜质量和提示词质量，画面交给视频模型、声音交给 TTS。
这样你可以把同一套提示词拿去喂不同模型比价、比效果。

**为什么提示词里英文夹着中文？**
故意的。镜头、构图、风格这些"指令"用英文写，模型理解更稳；台词部分原样保留中文并标注
`spoken in Mandarin Chinese`。反过来——全中文提示词容易让模型把汉字画进画面，全英文又会念成英文配音。

**画面里出现了汉字或字幕怎么办？**
提示词里已经写了禁止规则，但模型偶尔会犯。把那段重新生成一次；仍不行就把"负面约束"里的
`no visible words, captions, subtitles` 再往前提、加重语气。

**为什么非要我先选画幅和主题？**
这两个决定后面所有分镜的构图方式和运镜几何，改一个就要整套重做。先定好最省时间。

**能做多长？**
设计是 6 段 × 约 10 秒 ≈ 60 秒。想更长就让它出两组提案，或把每段拉长（注意多数模型单条上限 10–15 秒）。

**能配英文音吗？**
能，说"旁白用英文"即可，它会按原来的英语节奏重新算字数（130–150 词）。

---

> 以下为原作者 README 简体中文版原文。

<div align="center">

[**简体中文**](README.md) · [English](README.en.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Português do Brasil](README.pt-BR.md)

# Stickman Video Director

### 把任何想法，变成一支真正“动起来”的一分钟火柴人视频。

一个 Codex Skill，就能把你的文案变成经过确认的英文旁白、以画面为先的导演提案，以及六条可直接生产的 Gemini Omni Flash 提示词。

![Codex Skill](https://img.shields.io/badge/Codex-Skill-111827?style=flat-square)
![Gemini Omni Flash](https://img.shields.io/badge/Gemini-Omni%20Flash-6d28d9?style=flat-square)
![一分钟视频](https://img.shields.io/badge/Video-≈60%20seconds-0066ff?style=flat-square)
![MIT License](https://img.shields.io/badge/License-MIT-16a34a?style=flat-square)

适合制作发布在 **YouTube Shorts、TikTok、Instagram Reels 和 YouTube** 上的知识解释、励志故事、教育短片与快节奏视觉内容。

</div>

<!-- readme:demos -->

## 两种高对比风格，一套统一的视觉语言

| 白底黑火柴人 | 黑底白火柴人 |
|:---:|:---:|
| <!-- demo:light:start --><a href="assets/readme/light-theme-demo.mp4"><img src="assets/readme/light-theme-demo.gif" alt="白底黑火柴人和高饱和强调色的动态效果演示" width="600"></a><!-- demo:light:end --> | <!-- demo:dark:start --><a href="assets/readme/dark-theme-demo.mp4"><img src="assets/readme/dark-theme-demo.gif" alt="黑底白火柴人和高饱和强调色的动态效果演示" width="600"></a><!-- demo:dark:end --> |
| 白色画布 · 黑色人物 | 黑色画布 · 白色人物 |

> 点击任一动态预览，即可打开带声音的完整 10 秒视频。如果这种视觉风格也让你有了创作灵感，欢迎给仓库点一个 Star，让更多创作者发现它。

## 有文案，不等于已经有了视频

一个好想法仍然可能生成一段平淡的动画：一个人物、一个背景，十秒钟里几乎没有新的视觉变化。真正导演完整的一分钟，需要设计开场钩子、控制解释节奏、创造贴合内容的视觉隐喻、推动镜头、连接场景，并在多次独立生成之间锁住一致性。

**Stickman Video Director 会在你消耗生成额度之前，先完成这些制作层面的思考。**

<!-- readme:advantages -->

## 为什么短视频创作者会需要这个 Skill

| 优势 | 你会得到什么 |
|---|---|
| **更强的故事结构** | 在保留核心含义的前提下，把原始材料重组成强开场、递进解释和结尾回扣。 |
| **真正的确认节点** | 先展示清晰可读的六幕导演提案，再生成最终模型提示词；在修改成本最低的时候调整故事。 |
| **丰富且相关的动态画面** | 每段规划三个时间节拍，并加入视觉隐喻、环境变化、镜头运动、文字节点、人物互动、转场、BGM 与音效。 |
| **完整的生产锁定** | 在每条独立提示词中重复人物、线条粗细、配色、声音、台词、音频、转场和负面约束。 |
| **真正适配画幅的导演方式** | 针对 `9:16`、`16:9` 或 `1:1` 重新设计构图、镜头路径和文字位置，而不是只替换一个比例标签。 |
| **可控的视觉反差** | 支持白底黑人、黑底白人，以及最多三种高饱和强调色。 |
| **忠于原始材料** | 不随意编造缺乏依据的事实、数据、引语或产品卖点。 |

无需 API，也不依赖 MCP。安装 Skill、调用它，然后在对话中完成整个制作流程即可。

<!-- readme:platforms -->

## 同一个想法，为不同屏幕重新构图

| 比例 | 适合场景 | 导演重点 |
|---|---|---|
| `9:16` | YouTube Shorts、TikTok、Instagram Reels | 纵向纵深、醒目的中央轮廓、层叠式揭示、适合手机阅读的文字 |
| `16:9` | YouTube 知识视频、教育内容、视觉随笔 | 横向调度、侧向镜头运动、分屏对比、充足的负空间 |
| `1:1` | 社交平台信息流、紧凑的产品故事 | 强中心构图、放射式运动、清晰的边缘留白 |

<!-- readme:workflow -->

## 粘贴 → 选择 → 确认 → 生成 → 拼接

1. **粘贴**文案、笔记、文章，或者只给出一个主题。
2. **选择** `16:9`、`9:16` 或 `1:1`，再选择浅色或深色主题。
3. **确认**包含英文 VO、参考翻译、画面、镜头、转场、BGM 和音效的详细导演提案。
4. **生成**当前提案获批后的六条独立 Gemini Omni Flash 提示词。
5. **拼接**六段约十秒的视频，组成一支连贯的一分钟成片。

画幅、主题、旁白、场景结构、配色、声音或基调都可以修改。发生全局变化时，Skill 会回到提案阶段并重新请求确认。

<!-- readme:output -->

## 最终会得到什么

- 面向创作者的英文标题、核心观点、开场钩子、基调、配色、声音与音乐方向
- 约 **130–150 个英文单词**的一分钟旁白
- 六个彼此不同的画面场景，每两到三秒出现一次明显变化
- 精确英文台词与参考翻译
- 六条带时间节拍和负面约束的独立 Gemini Omni Flash 提示词
- 前后匹配的结尾与开场，让片段之间更容易衔接
- BGM、音效、一致性和最终拼接建议

<details>
<summary><strong>示例请求</strong></summary>

```text
Use $directing-stickman-videos to turn this copy into a one-minute English stickman video:

Gravity bends space and time so strongly around a black hole that even light cannot escape.
```

Skill 会先询问缺失的画幅和主题，然后展示六幕导演提案供你确认，确认前不会生成最终模型提示词。

</details>

<!-- readme:install -->

## 安装

克隆仓库：

```bash
git clone https://github.com/kaomei/stickman-video-director.git
cd stickman-video-director
```

把可安装的 Skill 文件夹复制到 Codex skills 目录：

```bash
cp -R skills/directing-stickman-videos "${CODEX_HOME:-$HOME/.codex}/skills/"
```

重启 Codex，让 Skill 出现在可用列表中。然后调用它并粘贴你的素材：

```text
$directing-stickman-videos
```

<!-- readme:reliability -->

## 为反复修改而设计，也诚实面对生成差异

- **确认必须明确。** 当前提案没有得到批准前，不会进入 Phase B。
- **全局变化会触发重新构图。** 新画幅或新主题会重新设计导演提案，而不是机械替换文字。
- **提示词可以独立使用。** 每条都会重复独立生成所需的关键锁定条件。
- **内容始终有依据。** Skill 可以强化结构与表达，但不会添加没有来源的主张。
- **音频仍可能存在差异。** 独立生成的视频可能出现轻微的声音或音乐差别。追求最高一致性时，可以保留每段同步音效，并在拼接时使用一条连续的外部旁白和 BGM。

## 仓库结构

```text
skills/directing-stickman-videos/  可安装的 Skill
assets/readme/                     README 演示素材
tests/                             行为场景与验证脚本
docs/superpowers/specs/            已确认的产品设计
docs/superpowers/plans/            实施计划
```

<!-- readme:contribute -->

## 一起把它做得更好

欢迎提交使用案例、提示词改进、真实生成记录与具体建议。你可以创建 issue，或者通过 pull request 提交一个范围明确、能够复现的改动。

如果这个 Skill 帮你把一个迟迟没有完成的想法，变成了一支真正可以发布的视频，**请给仓库点一个 Star**。它会帮助下一个正在寻找同样工作流的创作者发现这个项目。

## 许可证

MIT
