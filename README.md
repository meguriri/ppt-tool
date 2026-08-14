# PPT 工具（可复用 HTML 演示框架）

这是把"一页一个 HTML 的演示"做成可复用工具。根 html 不写任何页面，
自动扫描同目录下的编号页 `1.html`、`2.html`、`3.html`… 并按顺序展示。

## 目录结构

```text
ppt-tool/
├── index.html      # 根 html（通用模板，复制到每个演示目录即可）
├── skill/
│   ├── SKILL.md                   # 给 agent 的规范：怎么写页、怎么接入
│   ├── assets/index.html          # 根 html 模板
│   ├── assets/skin.css            # 默认皮肤（整体样式）
│   └── references/style.md        # 样式与排版规范
├── .gitignore      # 忽略 ppt/ 与 ppts/
└── ppt/            # 使用 skill 时的输出位置（被 git 忽略）
    └── <演示名>/
        ├── index.html   # 复制自根 html，无需修改
        ├── skin.css     # 整体皮肤，页面都 link 它
        ├── 1.html       # 第 1 页
        ├── 2.html       # 第 2 页
        └── …
```

## 新建一个演示

```bash
mkdir -p ppt/我的演示
cp index.html ppt/我的演示/
cp skill/assets/skin.css ppt/我的演示/
# 然后创建 1.html、2.html、3.html … 每页一个完整 html，并在 <head> 里
# <link rel="stylesheet" href="skin.css">
```

页面必须满足：

- 文件名只能是数字：`1.html`、`2.html`…（顺序即页序），目录里除了 `index.html` 不要放别的 html；
- 每页是**完整独立**的 html，按 **1280×720** 设计（根 html 会自动等比缩放）；
- 默认白底黑字；公式可用 MathJax CDN（需要联网）或纯 HTML 排版；
- 需要交互时，在页内自带按钮/滑块，不要依赖根 html。
- 封面页只放标题；整体样式统一放 `skin.css`，换皮肤只换这个文件。

## 预览

```bash
cd ppt/我的演示
python3 -m http.server 8000
# 浏览器打开 http://127.0.0.1:8000/
```

直接双击 `index.html`（file://）也可以，会自动扫描编号页。

## 操作

- 翻页：← → / 空格 / PageUp / PageDown / Home / End；触屏左右滑动
- 全屏：F 或右下角 ⛶
- 跳页：底部圆点
- 导出 PDF：浏览器 Cmd/Ctrl+P → 另存为 PDF（每页一页）

## 注意

- 根 html 通过探测 `1.html`、`2.html`… 是否存在来发现页面（http 下用 fetch，file:// 下用 `<object>`，Chrome 已验证），所以页号必须连续从 1 开始；
- 公式页（MathJax CDN）和嵌入外部服务的页面（如 localhost 前端）在别人电脑上可能打不开，交付前确认；
- `ppt/`（以及旧的 `ppts/`）已被 `.gitignore` 忽略，生成的内容不入库。
