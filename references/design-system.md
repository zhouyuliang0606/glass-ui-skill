# Glass UI Design System（玻璃质感设计系统）

> 本文件是 glass-ui-skill 的权威依据。所有数值均来自可运行示例 `examples/rain-ui-preview.html` 与 `examples/space-ui-preview.html`，复制即可复现质感。

---

## 0. 设计哲学
- **只改视觉/交互层，绝不碰业务逻辑。**
- 让已有页面「变好看，而不是变不同」。
- 两套质感分工明确：
  - **反光透明玻璃** → 面板 / 卡片 / 容器 / 列表行：通透 + 顶部高光条 + 亮边。
  - **磨砂玻璃** → 按钮 / 图标按钮 / CTA：奶白哑光半透 + 模糊 + 无高光条。
- 整页一张 `fixed` 背景图 + 暗过渡 `veil`；页面永不显示署名标注。
- 克制动效：滚动错峰 reveal、主题切换只动颜色、按钮 hover 微反馈、`prefers-reduced-motion` 全降级。

---

## 1. 主题 Token（两套场景）

### rain（深绿 / 柠檬绿强调）
```css
.tk-rain{
  --theme-bg:#07130d; --theme-bg2:#0a1b13;
  --theme-surface:rgba(16,36,28,.26); --theme-surface2:rgba(12,28,21,.42);
  --theme-glass:rgba(9,22,17,.30);
  --theme-primary:#d7ef83; --theme-primary-fg:#0a1a10;
  --theme-fg:#eaf3ec; --theme-fg2:#b9cfc2;
  --theme-muted:rgba(255,255,255,.06); --theme-muted-fg:#a2beb1;
  --theme-border:rgba(255,255,255,.11); --theme-border2:rgba(255,255,255,.18);
  --theme-accent:rgba(190,214,198,.12); --theme-destructive:#e85555;
  --theme-ring:rgba(215,239,131,.55);
}
```
> 强调色 RGB（用于 CTA 渐变）：`215,239,131`
> 背景 RGB（用于 veil）：`7,19,13`

### space（深蓝 / 冰蓝强调）
```css
.tk-space{
  --theme-bg:#080d1a; --theme-bg2:#0d1322;
  --theme-surface:rgba(18,32,50,.26); --theme-surface2:rgba(12,24,38,.42);
  --theme-glass:rgba(12,22,38,.30);
  --theme-primary:#9ecaff; --theme-primary-fg:#071020;
  --theme-fg:#e9f0ff; --theme-fg2:#a8bdd4;
  --theme-muted:rgba(255,255,255,.06); --theme-muted-fg:#8fa4bf;
  --theme-border:rgba(255,255,255,.11); --theme-border2:rgba(255,255,255,.18);
  --theme-accent:rgba(158,202,255,.12); --theme-destructive:#e85555;
  --theme-ring:rgba(158,202,255,.55);
}
```
> 强调色 RGB（用于 CTA 渐变）：`158,202,255`
> 背景 RGB（用于 veil）：`8,13,26`

**自定义场景**：复制任一结构，改 `--theme-bg/-primary/-fg` 三件套 + 对应 RGB 即可。

---

## 2. 玻璃质感配方

### 2.1 反光透明玻璃（面板 / 卡片 / 容器 / 列表行）
关键点：背景叠加一层**顶部白色渐变高光**（反光感）；模糊控制在 **16–18px**（太糊=磨砂、太清=无玻璃感）；内沿高光 `.22–.24`；边框用更亮的 `--theme-border2`。

```css
/* §3 反光透明玻璃：顶部高光 + 清晰透出背景 */
.surface{
  background:linear-gradient(180deg,rgba(255,255,255,.10),rgba(255,255,255,0) 45%) var(--theme-surface);
  border:1px solid var(--theme-border2);
  backdrop-filter:blur(18px) saturate(135%);
  -webkit-backdrop-filter:blur(18px) saturate(135%);
  box-shadow:0 1px 0 rgba(255,255,255,.22) inset, 0 18px 38px -22px rgba(0,0,0,.5);
  border-radius:14px;
}
.glass{
  background:linear-gradient(180deg,rgba(255,255,255,.12),rgba(255,255,255,0) 42%) var(--theme-glass);
  backdrop-filter:blur(16px) saturate(140%);
  -webkit-backdrop-filter:blur(16px) saturate(140%);
  border:1px solid var(--theme-border2);
  box-shadow:0 1px 0 rgba(255,255,255,.24) inset, 0 18px 40px -28px rgba(0,0,0,.5);
}
/* 列表行 / 时间轴列等小面板同理，给一点顶部高光即可 */
.todo,.tl-col{
  background:linear-gradient(180deg,rgba(255,255,255,.07),rgba(255,255,255,0) 60%) rgba(255,255,255,.03);
  border:1px solid var(--theme-border2);
  box-shadow:0 1px 0 rgba(255,255,255,.12) inset;
}
```

### 2.2 磨砂玻璃（按钮 / 图标按钮 / CTA）
关键点：**半透奶白填充 + 模糊 + 哑光内沿，没有顶部高光条**——和面板形成对比。

```css
/* 磨砂玻璃：半透奶白 + 柔化模糊 + 哑光（无高光条） */
.btn{
  background:rgba(255,255,255,.12);
  color:var(--theme-fg);
  border:1px solid rgba(255,255,255,.20);
  backdrop-filter:blur(14px) saturate(120%);
  -webkit-backdrop-filter:blur(14px) saturate(120%);
  box-shadow:0 1px 0 rgba(255,255,255,.10) inset;
}
.btn-default{background:rgba(255,255,255,.16);border-color:rgba(255,255,255,.24)}
.btn-outline{background:rgba(255,255,255,.06);border-color:rgba(255,255,255,.28)}
.btn-ghost{background:transparent;border-color:transparent}
.btn-destructive{background:color-mix(in srgb,var(--theme-destructive) 18%,rgba(255,255,255,.10));border-color:color-mix(in srgb,var(--theme-destructive) 42%,transparent)}
/* 主 CTA：磨砂强调色，保留辨识度 */
.btn-primary{
  background:linear-gradient(180deg,rgba(<PRIMARY_RGB>,.34),rgba(<PRIMARY_RGB>,.20));
  color:var(--theme-primary-fg);
  border:1px solid rgba(<PRIMARY_RGB>,.50);
  backdrop-filter:blur(14px) saturate(130%);
  -webkit-backdrop-filter:blur(14px) saturate(130%);
  box-shadow:0 1px 0 rgba(255,255,255,.18) inset;
}
```
> 把 `<PRIMARY_RGB>` 换成场景强调色 RGB（rain=`215,239,131`，space=`158,202,255`）。

---

## 3. 摄影规则（整页背景）
```css
.backdrop{position:fixed;inset:0;width:100%;height:100%;object-fit:cover;z-index:0;pointer-events:none}
.veil{position:fixed;inset:0;z-index:1;pointer-events:none;
  background:linear-gradient(180deg,
    var(--theme-bg) 0%,
    rgba(<BG_RGB>,.34) 16%,
    rgba(<BG_RGB>,.12) 50%,
    rgba(<BG_RGB>,.32) 84%,
    var(--theme-bg) 100%)}
main,footer{position:relative;z-index:2}
```
- 背景图整页一张，`position:fixed` 铺底，`z-index:0`。
- `veil` 顶/底溶入主题色，中间只 `.12` 透明 → 背景透出但不刺眼（想要更绿/更蓝就调低中间值到 `.06`，想更暗就调高到 `.20`）。
- 内容层 `main,footer` 抬到 `z-index:2` 浮在背景之上；卡片的 `backdrop-filter` 会实时模糊背后的背景图 → 真玻璃感。
- 页面**永不**显示署名/水印标注。
- 背景图优先用线上压缩资源；示例里是 base64 内联仅作演示。

---

## 4. 动效规格
```css
/* 滚动错峰 reveal（首屏立即 in 不闪） */
.reveal{opacity:0;transform:translateY(16px);transition:opacity .65s ease,transform .65s ease;transition-delay:var(--d,0s)}
.reveal.in{opacity:1;transform:none}
/* 主题切换只动颜色，380ms */
.tk-rain *{transition:background-color .38s,border-color .38s,color .38s,box-shadow .38s}
/* 按钮 hover 微反馈 */
.btn{transition:background .15s,color .15s,transform .12s,box-shadow .15s,border-color .15s}
.btn:active{transform:translateY(1px)}
.btn-primary:active{transform:scale(.95)}
/* 全降级 */
@media (prefers-reduced-motion: reduce){
  .reveal{opacity:1;transform:none;transition:none}
  *{transition-duration:.01ms !important;animation-duration:.01ms !important}
}
```
- reveal 用 IntersectionObserver 在元素进入视口时加 `.in`；首屏元素直接加 `.in` 避免闪。
- 主题切换不要动布局，只动颜色/边框/阴影。

---

## 5. 按钮规格
- **主 CTA（打卡/提交）**：胶囊（`border-radius:999px`）+ 磨砂强调色 + 按下 `scale(.95)`。
- **variant**：`default / outline / ghost / destructive` 都用磨砂奶白，只换边框/文字/背景色（见 §2.2）。
- 图标按钮 `.icon-btn`：圆形磨砂玻璃，同 §2.2 配方。

---

## 6. 适配原生 HTML/CSS（非 Tailwind）
- 本系统产出都是**纯 CSS**，直接可用。
- 若原页面已用 CSS 变量：把 §1 token 注入 `:root` 或场景类即可。
- 若原页面用 Tailwind 工具类：改写成等价 CSS——
  - `backdrop-blur-md` → `backdrop-filter:blur(16px)`
  - `bg-white/10` → `background:rgba(255,255,255,.10)`
  - `border-white/10` → `border:1px solid rgba(255,255,255,.10)`
  - 高光条 → 叠加 `linear-gradient(180deg,rgba(255,255,255,.10),transparent)` 背景层
- 注入方式：在 `<head>` 末尾加一段 `<style>` 或在现有 CSS 文件追加 §1–§4 片段，再把对应 class 挂到元素上。

---

## 7. 迁移到目标页面的通用注意点
- 目标页面通常是学生端 / 用户端主页（如 `student.html`）。若是原生 HTML/CSS/JS、**无构建步骤**的项目，改完刷新浏览器即可生效。
- 背景图建议换成线上压缩资源（图床 / CDN），**不要 base64 内联**（会让单文件涨几百 KB）。
- 只在视觉层加 class（`.surface`/`.glass`/`.btn` 等），不要改写现有 JS 交互逻辑。
- 若页面当前是浅色主题，整页采用深色玻璃 = 视觉身份大改，落地前需与用户确认。
- 提交时按你所在项目的既有纪律操作（如代码 / 文档 / 测试分开提交）。
