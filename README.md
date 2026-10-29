# Solcat Skills

个人 Agent Skills 集合，按 [Agent Skills](https://docs.claude.com/en/docs/claude-code/skills) 规范组织，面向 Claude Code、CodeBuddy 等 Agent CLI 使用。

每个 skill 是一个独立目录：`SKILL.md` 是核心（含 YAML frontmatter），`references/` 存放按需加载的补充资料。所有内容均为**原创方法论提炼**（源自个人学习资源的二次总结），不包含任何受版权保护的原始素材（剧本、题库、模型、课程文件等）。

## 全部 Skills 总览（9 个）

| Skill | 一句话用途 | 触发示例 |
|---|---|---|
| [`murder-mystery-gm`](./skills/murder-mystery-gm/SKILL.md) | 剧本杀主持人（GM/DM）+ AI 扮演其他玩家，双模式 | 「来开一局剧本杀，你当 DM」「人不够，你帮我扮演一个玩家」 |
| [`turtle-soup-gm`](./skills/turtle-soup-gm/SKILL.md) | 海龟汤主持人：出汤面、判题、救场、复盘；也支持 AI 代猜模式 | 「出个海龟汤」「这题我主持你来猜」 |
| [`no-dm-script-guide`](./skills/no-dm-script-guide/SKILL.md) | 封闭式（无需主持）剧本杀组织指南：分工、流程表、规则自运转 | 「我们凑不到 DM，怎么组织玩封闭本」 |
| [`cosplay-retouch`](./skills/cosplay-retouch/SKILL.md) | 漫展场照 / cos 后期修图全流程：磨皮、液化、补妆、抠图、特效溶图 | 「帮我修这张 cos 场照，背景有路人」 |
| [`cos-con-shooting`](./skills/cos-con-shooting/SKILL.md) | 漫展场照拍摄：行前清单、机位用光、与 coser 配合、交付规范 | 「下周去漫展拍照，帮我列个清单」 |
| [`photography-bts`](./skills/photography-bts/SKILL.md) | 通用摄影幕后：勘景、布光方案、分镜、器材、花絮记录 | 「拍一组人像，出布光方案」 |
| [`blender-modeling`](./skills/blender-modeling/SKILL.md) | Blender 建模全流程：拓扑、UV、材质、修改器、渲染与排错 | 「用 Blender 建个杯子，拓扑要干净」 |
| [`design-learning-path`](./skills/design-learning-path/SKILL.md) | 设计自学路线：四阶段路线图、方向选择、卡点诊断 | 「我想自学 PS 修图，该从哪开始」 |
| [`design-freelance-toolbox`](./skills/design-freelance-toolbox/SKILL.md) | 设计接单渠道对比 + 免费商用素材/图标/字体站导航 | 「想接设计单，去哪个平台」「找免费商用图片」 |

## 目录结构

```
solcat-skills/
├── README.md
├── assets/                          # 开源素材收录（含署名文件）
│   ├── ATTRIBUTION.md
│   └── blender/isometric-room/      # MIT 协议等距房间场景
└── skills/
    ├── murder-mystery-gm/
    │   ├── SKILL.md
    │   └── references/
    │       ├── gm-runbook.md        # 逐幕主持手册：开场词、话术、救场
    │       ├── ruling-cases.md      # 疑难判定与争议场景
    │       ├── jargon.md            # 剧本杀黑话词典
    │       └── deduction-tips.md    # 微表情信号、证据链、隐瞒者打法
    ├── turtle-soup-gm/
    │   ├── SKILL.md
    │   └── references/
    │       └── hosting-examples.md  # 判题示例、救场话术、汤面自检
    ├── no-dm-script-guide/
    │   └── SKILL.md
    ├── cosplay-retouch/
    │   └── SKILL.md
    ├── cos-con-shooting/
    │   └── SKILL.md
    ├── photography-bts/
    │   ├── SKILL.md
    │   └── references/production-handbook.md
    ├── blender-modeling/
    │   ├── SKILL.md
    │   └── references/parameters.md
    ├── design-learning-path/
    │   └── SKILL.md
    └── design-freelance-toolbox/
        └── SKILL.md
```

## 使用方式

### 安装

```bash
# Claude Code
cp -r skills/* ~/.claude/skills/

# CodeBuddy
cp -r skills/* ~/.codebuddy/skills/
```

### 触发

安装后无需手动调用，说人话即可命中对应 skill（见总览表的触发示例列）。也可以显式引用：`/skill murder-mystery-gm`。

## 各 Skill 详细说明

### murder-mystery-gm（剧本杀 GM / AI 玩家）⭐ 双模式

- **GM 模式**：三条底线（不泄露真相 / 不替玩家推理 / 不做剧情主角）贯穿全程。开本前五步：真相地图 → 分幕必达点 → 预判卡点 → 节奏表 → 开场词。主持中：五类提问分类处理、线索只描述物理事实、私聊限时、六种救场方案。终局复盘六步法：公布答案 → 还原真相 → 回指线索 → 解释误导 → 点评亮点 → 收尾。
- **扮演模式**：真人不够时 AI 补位扮演玩家。按人设发言、九真一假地撒谎、不超额推理、复盘时解绑公布秘密。
- **4 份参考文档**：逐幕话术手册、疑难判定先例、黑话词典（DM/AP/BP/开车翻车）、推理与反推理技巧（微表情八信号、证据链交叉验证、隐瞒者自保）。

### turtle-soup-gm（海龟汤主持）

海龟汤 = 情境猜谜：主持人持汤底，玩家用只能回答「是/否/无关」的问题还原真相。本 skill 覆盖：三档判题规范与一致性原则、「已确认/已排除/未知」三栏播报、五级递进救场（维度提示 → 关键词 → 缩小包围 → 半个汤底）、结束判定四条件、按「类型 × 难度 × 玩家构成」三维选题、原创汤面质量自检清单，以及玩家当主持时的 **AI 解题者模式**。参考文档含判题示例表与五种冷场救场话术。

### no-dm-script-guide（封闭式剧本杀组织）

把 DM 职能拆给流程设计：组织者/计时员/记录员三角色分工、8~9 环节标准时间表、五条规则自运转设计（流程印剧本、线索卡编号、轮转发言、矛盾公示、投票前三问）、五个翻车点补救表、按车队水平选本建议。

### cosplay-retouch（cos 后期）

八步总流程（校正 → 清瑕 → 调色 → 磨皮 → 液化 → 补妆 → 细节 → 特效），每步有工具与参数：磨皮三法对比表（高低频/中性灰/双曲线）与"熨斗脸"避坑、液化顺布纹与背景直线检查、补妆骨相修容与五步眼妆重绘、抠图四手法速查表、特效溶图的明度/饱和度/色温三匹配。

### cos-con-shooting（漫展拍摄）

漫展特殊性应对：行前器材与功课双清单、五场景机位用光表（暗区跳闪、幕墙逆光、压光法暗化人群、1/500 动作戏）、与 coser 的现场配合五条（当场指出假发/妆面问题、指令具体化、连拍 3 张一组）、人流与场务默契、24 小时精选图交付规范。

### photography-bts（通用摄影幕后）

五项锁定表把模糊需求变可执行清单；勘景十项记录（光源方位、插座、可动空间…）；五种布光方案参数表（三点/蝴蝶/伦勃朗/窗光/高调低调）；平方反比定律实操；器材"常被遗忘五项"；BTS 素材命名规范。

### blender-modeling（Blender 建模）

所有参数给具体数值：面数预算按用途分档；建模固定顺序 Block out → Merge → Refine → Subdivision → Cleanup；拓扑四原则；UV 接缝与 Smart UV 取舍；修改器链配置顺序；Cycles/EEVEE 渲染设置；常见报错排查表。

### design-learning-path（设计自学路线）

四阶段路线图（软件操作 → 专项技能 → 作品集 → 接单变现）各配通关标志；阶段一六项工具清单与 10 分钟自检；三个专项方向（人像精修/合成/平面）的技能链与变现场景对照；「临摹 → 分析 → 变体」练习法；四种卡点症状诊断表。

### design-freelance-toolbox（接单与素材导航）

五大接单渠道优劣势对比表与新手策略（收定金 30~50%、改稿次数写进约定）；免费可商用图片站、图标库、免抠/PSD 站、字体工具分类清单；中文字体商用授权警告；素材授权三步过滤法（用途 → 授权 → 风格）。

## Skill 编写规范（贡献指南）

1. `SKILL.md` 必须含 YAML frontmatter：`name`（小写连字符）与 `description`
2. `description` 写「做什么 + 何时触发 + 何时不该用」——这是触发准确性的关键
3. 正文用命令式写给 Agent 执行；参数用表格固化，禁用「适当」「酌情」
4. 大篇幅资料放 `references/`，正文末尾列明各自用途，按需加载
5. 每个 skill 必须带「自检清单」或「边界声明」
6. **只收录原创方法论与公开可验证的资源链接；不收录任何受版权保护的原始素材**

## 更新日志

- 2026-10 v3：新增 6 个 skill（turtle-soup-gm、no-dm-script-guide、cosplay-retouch、cos-con-shooting、design-learning-path、design-freelance-toolbox）；murder-mystery-gm 升级双模式并加 2 份参考文档；新增开源素材目录 assets/
- 2026-10 v2：三个初始 skill 重写为完整版；murder-mystery-script 更名 murder-mystery-gm
- 2026-10 v1：初始三个 skill（blender-modeling、photography-bts、murder-mystery-script）

## License

方法论内容：个人使用与学习，欢迎参考与二次修改。
`assets/` 内素材按其各自的开源协议（见 `assets/ATTRIBUTION.md`）。
