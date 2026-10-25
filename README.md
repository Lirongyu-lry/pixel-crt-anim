# Pixel CRT Anim

> 把一句主题描述，变成单文件、零依赖、双击就能看的像素风 CRT 网页动画。

Pixel CRT Anim 是一个遵循 [Agent Skills](https://agentskills.io) 开放标准的 Skill。它不提供运行时库，也不要求安装任何东西；它教的是一套可复用的创作规范：构图、像素画法、点阵字体、CRT 扫描线、确定性动画，以及交付前的验收方法。

仓库内自带一个完整成品：[`examples/demo.html`](examples/demo.html)。主题是“机械蜂鸟”：黄铜蜂鸟在夜间植物舱里悬停，长喙探入机械花，背景有条纹落日、天线城市、像素网格与实时 HUD。

## 它能做什么

输入一句主题，例如：

```text
机械蜂鸟
```

输出一个 `index.html`：

- 竖屏 9:16，展示尺寸 720 × 1280
- 纯 HTML / CSS / JavaScript / Canvas
- 所有图形由代码绘制，无图片、无字体、无 CDN、无接口
- 双击 `file://` 即可播放，断网也能看
- 动画由 `requestAnimationFrame` 驱动
- 不使用 `Math.random()`，刷新后画面一致
- 带 CRT 扫描线、荫罩、暗角、反光、缓慢扫描带
- 手机、平板、桌面自适应，支持点击/空格重播

## 快速开始

### 1. 安装

把整个 `pixel-crt-anim/` 文件夹放进你的 Agent 客户端使用的 Skill 目录。目录名需要与 `SKILL.md` 里的 `name` 一致。

常见位置：

```text
# Claude Code（用户级）
~/.claude/skills/pixel-crt-anim/

# Claude Code（项目级）
<你的项目>/.claude/skills/pixel-crt-anim/

# 其他支持 Agent Skills 开放标准的客户端
请使用该客户端文档中规定的 skills 根目录
```

本 Skill 没有任何构建步骤和第三方依赖，不需要 `npm install`、`pip install` 或联网。

### 2. 调用

在对话里直接描述需求，Agent 会根据 `SKILL.md` 的 `description` 自动判断是否加载：

```text
用 pixel-crt-anim 做一个“深夜便利店”的像素 CRT 动画。
```

也可以显式点名：

```text
请使用 pixel-crt-anim Skill，帮我做一个复古游戏标题画面。
```

### 3. 预览示例

直接双击打开：

```text
pixel-crt-anim/examples/demo.html
```

或者拖进浏览器。没有服务器、没有安装步骤。

## 示例里能看到什么

打开 `examples/demo.html` 后：

1. 一道横向 CRT 亮线从屏幕中间展开，进入 720 × 1280 的竖屏画面。
2. 深蓝夜空里出现一颗巨大的条纹落日，右下方是机械城市剪影与透视网格。
3. 左侧一只黄铜与青绿色机械蜂鸟悬停，翅膀以半透明残影高速振动，尾巴轻轻摆动。
4. 它把一根细长的金属喙探入右下方机械花的发光花心，花心齿轮与花瓣缓慢呼吸。
5. 顶部的大号点阵字 `MECHANICAL / HUMMINGBIRD` 逐字打出，末端的方块光标闪烁。
6. 底部实时 HUD 显示 `WING 51 HZ`、`CORE 98.0%`、`NECTAR 89%`、`SYNC 100%`。
7. 扫描线、暗角、轻微 RGB 色差与缓慢扫描带持续覆盖画面。
8. 点击画面或按空格，重新播放开机与打字动画。

## 目录结构

```text
pixel-crt-anim/
├── SKILL.md                 # Skill 入口：触发描述、五段工作规范、三个提示词模板
├── README.md                # 本文件：面向人的说明与安装方式
├── LICENSE                  # MIT 开源协议
├── .gitignore               # 忽略系统、编辑器与临时文件
├── examples/
│   └── demo.html            # 完整可运行示例：机械蜂鸟
└── references/
    ├── style-guide.md       # 像素画 / CRT 的视觉规范与代码配方
    └── checklist.md         # 交付前的逐项自检清单
```

## 技术说明

- `examples/demo.html` 的逻辑画布为 360 × 640，通过 CSS 放大到 9:16，保证像素颗粒清晰。
- 所有文字由内置 5 × 7 点阵字形绘制，不依赖外部字体。
- 所有粒子、星空、星线、闪烁与故障时机都由时间 `t` 和固定公式生成。
- CRT 效果由 CSS 叠加层完成，不占用 Canvas 动画循环。
- 动画时钟在页面重新可见时重置，避免切换标签页后跳过整段开场。
- 尊重 `prefers-reduced-motion`：降低动效与关闭扫描带/flicker。

## 自定义

想在这个示例上改主题，优先改这些位置：

| 想改什么 | 从哪里开始 |
| --- | --- |
| 标题、HUD 文案 | `examples/demo.html` 里的 `TITLE_A`、`TITLE_B`、`drawHud()` |
| 调色板 | 文件顶部的 `C` 对象 |
| 机械花颜色 | `PETAL_COLORS` 数组 |
| 主体位置 | `drawBird()` 的 `bx` / `by`，以及 `FX` / `FY` |
| 背景城市 | `SKYLINE` 数组 |
| CRT 强度 | `<style>` 里的 `.scanlines`、`.mask`、`.vignette` 透明度 |
| 文字资产 | `FONT` 表；未收录字符会被跳过 |

如果要生成一个全新主题，不要从空白页开始抄代码；让 Agent 先读 [`SKILL.md`](SKILL.md)，再按 [`references/style-guide.md`](references/style-guide.md) 选择构图和调色板，最后用 [`references/checklist.md`](references/checklist.md) 验收。

## 使用建议

- 一次只做一个主角。主体与背景同样复杂时，画面会散。
- 先把主体剪影画对，再加 CRT 和故障。CRT 只能增强，不能拯救一个看不清的主体。
- 关键信息使用 2× 点阵字，手机 360px 宽也能读清；1× 字号只做装饰。
- 不要为了“炫”使用整屏高频闪烁。扫描线要低透明度，故障要稀疏。
- 生成新主题后，至少用 Chrome/Edge 和手机实机各打开一次。

## 浏览器支持

示例使用标准 Canvas 2D、CSS 渐变与 `requestAnimationFrame`，面向最新版 Chrome、Edge、Firefox 与 Safari。

已在以下环境验证：

- Windows / Microsoft Edge Headless（Chromium 内核）：`file://` 正常渲染，控制台无错误，网络面板只有本地 HTML 一个请求
- 桌面 720 × 1280 与手机 390 × 844 视口：舞台保持 9:16，无横向滚动

## 许可

MIT，见 [`LICENSE`](LICENSE)。示例与规范均可用于个人和商业项目。

## 贡献

欢迎提交新的示例主题、调色板或验收用例。提交前请确认：

1. 仍是单文件、零依赖、可双击打开；
2. 没有 `Math.random()` 和外部资源；
3. 通过 [`references/checklist.md`](references/checklist.md)；
4. 同步更新本 README 与示例说明。
