# Solcat Skills

个人 Agent Skills 集合，按 [Agent Skills](https://docs.claude.com/en/docs/claude-code/skills) 规范组织。

## 包含的 Skills

| Skill | 用途 |
|---|---|
| [`blender-modeling`](./skills/blender-modeling/SKILL.md) | Blender 建模、拓扑、UV、材质、灯光与渲染的工作流与排错 |
| [`photography-bts`](./skills/photography-bts/SKILL.md) | 摄影拍摄幕后的勘景、布光方案、分镜、器材清单与花絮记录规范 |
| [`murder-mystery-script`](./skills/murder-mystery-script/SKILL.md) | 剧本杀剧本创作：凶案设计、角色、时间线、线索配比与一致性校验 |

## 目录结构

```
solcat-skills/
└── skills/
    ├── blender-modeling/
    │   ├── SKILL.md
    │   └── references/
    ├── photography-bts/
    │   ├── SKILL.md
    │   └── references/
    └── murder-mystery-script/
        ├── SKILL.md
        └── references/
```

每个 skill 是一个独立目录，核心文件为 `SKILL.md`，包含 YAML frontmatter（`name`、`description`）与正文说明。`references/` 用于存放该 skill 的补充资料与模板。

## 使用方式

将此仓库的 `skills/` 目录下的子目录复制或链接到本地 skills 目录即可被识别：

- Claude Code：`~/.claude/skills/`
- CodeBuddy：`~/.codebuddy/skills/`

## 状态

三个 skill 均为初版骨架，基于领域通识构建。待补充内容已在各 `SKILL.md` 末尾的「待补充」小节标出，将随实际项目素材迭代完善。

## License

个人使用。
