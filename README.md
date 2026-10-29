# Solcat Skills

个人 Agent Skills 集合，按 [Agent Skills](https://docs.claude.com/en/docs/claude-code/skills) 规范组织，面向 Claude Code、CodeBuddy 等 Agent CLI 使用。

每个 skill 是一个独立目录：`SKILL.md` 是核心（含 YAML frontmatter），`references/` 存放按需加载的补充资料。

## 包含的 Skills

| Skill | 一句话用途 | 模式/亮点 |
|---|---|---|
| [`murder-mystery-gm`](./skills/murder-mystery-gm/SKILL.md) | 剧本杀主持人（GM/DM）+ AI 扮演其他玩家 | GM 模式 / 扮演模式双模式 |
| [`blender-modeling`](./skills/blender-modeling/SKILL.md) | Blender 建模、拓扑、UV、材质、灯光与渲染 | 全流程 + 报错排错 |
| [`photography-bts`](./skills/photography-bts/SKILL.md) | 摄影幕后：勘景、布光、分镜、器材、花絮 | 现场可执行清单 |

## 目录结构

```
solcat-skills/
└── skills/
    ├── murder-mystery-gm/
    │   ├── SKILL.md
    │   └── references/
    │       ├── gm-runbook.md        # 逐幕主持手册：开场词、话术、救场
    │       ├── ruling-cases.md      # 疑难判定与争议场景
    │       ├── jargon.md            # 黑话/行话词典（DM、AP、开车翻车…）
    │       └── deduction-tips.md    # 微表情八技巧、凶手逃脱打法、复盘用法
    ├── blender-modeling/
    │   ├── SKILL.md
    │   └── references/
    │       └── parameters.md        # 参数表：面数、修改器、PBR、灯光、渲染
    └── photography-bts/
        ├── SKILL.md
        └── references/
            └── production-handbook.md
```

`description` 决定了 skill 何时被触发，因此统一写成「做什么 + 何时用 + 不要用来做什么」的结构，避免误触发。

## 使用方式

### 方式一：复制安装

```bash
# Claude Code
cp -r skills/* ~/.claude/skills/

# CodeBuddy
cp -r skills/* ~/.codebuddy/skills/
```

### 方式二：直接对话触发

安装后无需手动调用。比如说出以下任意一句话，对应的 skill 就会被 Agent 加载：

- murder-mystery-gm：「来开一局剧本杀，你当 DM」「人不够，你帮我扮演一个玩家」
- blender-modeling：「帮我在 Blender 里建一个杯子，拓扑要干净」
- photography-bts：「下周拍一组人像，帮我出布光方案和器材清单」

## 各 Skill 详细说明

### murder-mystery-gm（剧本杀主持人 / AI 玩家）

**双模式设计：**

- **GM 模式**：让 AI 当主持人，不是写本。三条底线贯穿全程——不泄露真相、不替玩家推理、不做剧情主角。覆盖开本前五步准备（真相地图、分幕必达点、预判卡点、节奏表、开场词）、五类提问分类处理、六种卡点救场方案、复盘六步法（公布答案 → 还原真相 → 回指线索 → 解释误导 → 点评亮点 → 收尾）。
- **扮演模式**：真人玩家凑不齐时，AI 在主持的同时扮演 1~2 个"AI 玩家"补位。按人设发言、该撒谎就撒谎（九真一假）、不超额推理把关键推理留给真人玩家、复盘时解绑回归 GM 身份公布全部秘密。

**四份参考文档：**

| 文档 | 内容 |
|---|---|
| `gm-runbook.md` | 逐幕主持手册，含开场词模板、各环节话术、救场方案 |
| `ruling-cases.md` | 常见疑难判定与争议场景的处理先例 |
| `jargon.md` | 黑话词典：DM/MC、AP/BP、HE/BE、互证、软逻辑、开车翻车跳车等 |
| `deduction-tips.md` | 微表情八大识别信号、证据链交叉验证方法、隐瞒者自保打法、复盘点评用法 |

### blender-modeling（Blender 建模与出图）

从参考图到成品的完整流程。特色是**所有参数都给具体数值**而非「适当调整」：

- 面数预算按用途分档（游戏实时 / 影视渲染 / 3D 打印）
- 建模顺序固定：Block out → Merge → Refine → Subdivision → Cleanup
- 拓扑原则：四边面优先、支撑环线、极点避让
- UV、修改器链（Boolean/Subdivision/Bevel 等的配置与顺序）、PBR 材质数值表
- Cycles / EEVEE 渲染设置与打光方案
- 常见报错排查：Boolean 失败、法线翻转、细分尖刺、内存不足

### photography-bts（摄影幕后策划与执行）

拍摄幕后的可执行清单，而非泛泛的审美建议：

- **拍摄前**：五项锁定表（主题情绪/场景/光线/人物服装/成片用途）、勘景十项记录
- **拍摄中**：五种布光方案（三点/蝴蝶/伦勃朗/窗光/高调低调）各有位置、附件、功率参数表；平方反比定律的实操应用
- **拍摄后**：BTS 素材统一命名规范与必拍清单、拍摄手记模板

## Skill 编写规范（给想贡献的人）

1. `SKILL.md` 必须含 YAML frontmatter：`name`（小写连字符）与 `description`
2. `description` 写「做什么 + 何时触发 + 何时不该用」，这是触发准确性的关键
3. 正文用命令式语气写给 Agent 执行，多用表格固化参数，避免「适当」「酌情」这类模糊词
4. 大篇幅参考资料放 `references/`，在正文末尾列出并说明各自用途，按需加载
5. 每个 skill 必须自带「自检清单」或「边界声明」，防止 Agent 越界

## 更新日志

- 2026-10：murder-mystery-gm 新增「AI 扮演其他玩家」模式，新增黑话词典与推理/反推理技巧两份参考文档；README 全面扩充

## License

个人使用与学习，欢迎参考与二次修改。
