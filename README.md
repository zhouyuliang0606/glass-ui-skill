# glass-ui-skill

把已有网页原地升级为「深色玻璃质感」UI 的可移植技能包。面板用**反光透明玻璃**、按钮用**磨砂玻璃**、整页一张固定背景图 + 暗过渡 veil。只动视觉/交互层，不碰业务逻辑。

## 署名 / Attribution（必读）
本技能包的**设计语言与玻璃质感配方改编自 [gdut4140/summer-checkin-ui-skill](https://github.com/gdut4140/summer-checkin-ui-skill)（MIT License）**。我们在此基础上做了适配与扩展（见下方说明）。

- 原作者的版权与 MIT 许可全文见 **[NOTICE.md](./NOTICE.md)**，依 MIT 要求一并提供。
- 本包**修改部分**以 MIT 许可发布，见 **[LICENSE](./LICENSE)**。
- 若你将其用于公开仓库/比赛，请保留 `NOTICE.md` 与 `LICENSE`，并视情况在 README 致谢原作者。

## 目录结构
```
glass-ui-skill/
├── SKILL.md                      # 技能说明 + 工作流（给 AI Agent 读）
├── README.md                     # 本文件
├── references/
│   └── design-system.md          # 权威依据：两套 token + 玻璃配方 + 摄影/动效/按钮规格
└── examples/
    ├── rain-ui-preview.html      # 深绿林间主题，完整可运行示例
    └── space-ui-preview.html     # 深蓝星空主题，完整可运行示例
```

## 怎么用到 Qoder（或其他 AI 编码 Agent）
本技能是「纯 markdown + 示例 HTML」，任何能读文件的 AI Agent 都能直接用：

1. **整包投喂**（最省事）：把 `glass-ui-skill/` 整个文件夹作为上下文给 Qoder，并说：
   > 用 glass-ui-skill 这个技能，把 `<你的项目>/app/static/student.html`（或任意目标页面）升级成深色玻璃质感 UI（反光面板 + 磨砂按钮 + 固定背景图 + veil）。只改视觉层，不要动 JS 逻辑。背景图用我指定的线上图片。

2. **只给关键文件**（更轻）：把 `SKILL.md` + `references/design-system.md` 贴进对话，再附 `examples/rain-ui-preview.html` 让它照着复刻。

3. **当 skill 安装**：若 Qoder 支持 skill 目录，把 `glass-ui-skill/` 放进它的 skills 目录，触发词可定「玻璃 UI / glass ui / 给页面加质感」。

> 注意：示例里的背景图是 base64 内联（仅演示）。正式落地请换成你自己的**线上图片 URL**，避免单文件膨胀。

## 想自己本地先看效果
直接双击打开 `examples/rain-ui-preview.html` 或 `space-ui-preview.html` 即可在浏览器预览（无需任何依赖）。

## 主题切换
- 深绿：`examples/rain-ui-preview.html` / token 见 design-system.md §1 `rain`。
- 深蓝：`examples/space-ui-preview.html` / token 见 §1 `space`。
- 自定义：复制任一结构，改 `--theme-bg / --theme-primary / --theme-fg` 三件套 + 对应 RGB 即可。

## 红线（务必遵守）
- 只改视觉/交互层，不碰业务 JS / 数据结构 / 路由。
- 面板反光、按钮磨砂，两套质感不要混。
- 页面不显示任何署名/水印标注。
- 尊重 `prefers-reduced-motion`。
