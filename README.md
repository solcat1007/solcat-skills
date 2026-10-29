# Solcat Skills

个人 Agent Skills 集合，按 [Agent Skills](https://docs.claude.com/en/docs/claude-code/skills) 规范组织。

## 包含的 Skills

| Skill | 用途 |
|---|---|
| [`blender-modeling`](./skills/blender-modeling/SKILL.md) | Blender 建模、拓扑、UV、材质、灯光与渲染的完整工作流与排错 |
| [`photography-bts`](./skills/photography-bts/SKILL.md) | 摄影拍摄幕后的勘景、布光方案、分镜、器材清单与花絮记录规范 |
| [`murder-mystery-gm`](./skills/murder-mystery-gm/SKILL.md) | 让 AI 扮演剧本杀主持人（GM），掌控节奏、分幕推进、控场救场与复盘 |

## 目录结构

```
solcat-skills/
└── skills/
    ├── blender-modeling/
    │   ├── SKILL.md
    │   └── references/
    │       └── parameters.md
    ├── photography-bts/
    │   ├── SKILL.md
    │   └── references/
    │       └── production-handbook.md
    └── murder-mystery-gm/
        ├── SKILL.md
        └── references/
            ├── gm-runbook.md
            └── ruling-cases.md
```

每个 skill 为独立目录，核心文件是 `SKILL.md`，含 YAML frontmatter（`name`、`description`）与正文。`references/` 存放补充资料，按需加载。

`description` 决定了 skill 何时被触发，因此写成了「做什么 + 何时用 + 不要用来做什么」的结构，避免误触发。

## 使用方式

把 `skills/` 下的子目录复制或软链接到本地 skills 目录：

```bash
# Claude Code
cp -r skills/* ~/.claude/skills/

# CodeBuddy
cp -r skills/* ~/.codebuddy/skills/
```

## 各 Skill 说明

### blender-modeling

从参考图到成品的完整建模流程。特色是**所有参数都给具体数值**而非「适当调整」：面数预算按用途分档，修改器给推荐值与顺序，材质给 PBR 数值表，灯光给瓦数与距离，渲染给采样数。附常见报错排查（Boolean 失败、法线翻转、细分尖刺、内存不足）。

### photography-bts

拍摄幕后的可执行清单。五种布光方案（三点/蝴蝶/伦勃朗/窗光/高调低调）各有位置、附件、功率表；勘景十项记录；器材清单含「常被遗忘的」五项；日程强调布光时间单列。BTS 素材有统一命名规范与必拍清单。

### murder-mystery-gm

**定位是让 AI 当主持人，不是写本。** 三条底线：不泄露真相、不替玩家推理、不做剧情主角。含开本前真相地图、每幕必达点、五类提问处理方式、六种卡点救场方案、复盘六步法。两份参考文档分别是逐幕话术手册和疑难判定表。

## License

个人使用。
