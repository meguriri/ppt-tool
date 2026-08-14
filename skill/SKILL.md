---
name: html-ppt-builder
description: Build reusable HTML slide decks where every slide is a separate numbered HTML file (1.html, 2.html, ...) auto-discovered by a shared root index.html. Use when the user asks to create a presentation / PPT / slide deck / 演示 / 汇报页 with HTML pages, add or reorder pages, or maintain the ppt-tool framework in this project (including writing new pages, wiring them in, previewing, and PDF export).
---

# HTML PPT Builder

每个演示是一个目录：根 `index.html`（通用模板，**不要改**）+ 同目录下编号页
`1.html`、`2.html`、`3.html`… 根 html 自动扫描编号页并按顺序展示。

根 html 模板见 **`assets/index.html`**：每次新建演示直接复制它，不要手写、
不要修改它的扫描/导航逻辑。

## 工作流

1. **建目录**：在**当前使用 skill 的输出位置**新建一个叫 `ppt` 的目录（不存在才建），
   演示放在 `ppt/<演示名>/`。**不要**把演示放到某个固定的 `ppts/` 目录。
2. **复制根 html**：把 `assets/index.html` 复制成 `ppt/<演示名>/index.html`。
3. **写页面**：逐页创建 `1.html`、`2.html`… 每页一个完整独立 html。
4. **接入**：页面放对目录、按数字命名即自动接入，不需要改根 html、不需要注册清单。
5. **预览**：`python3 -m http.server` 打开目录，或直接双击 `index.html`。
6. **交付**：整目录打包 zip；需要静态版时用浏览器打印导出 PDF（每页一页）。

## 目录结构

```text
<当前输出位置>/
└── ppt/
    └── <演示名>/
        ├── index.html   # 来自 assets/index.html，无需修改
        ├── 1.html
        ├── 2.html
        └── …
```

## 页面规范

- 文件名只能是数字（从 1 开始、连续），目录里除 `index.html` 外不要有别的 html；
- 按 **1280×720** 设计；根 html 会等比缩放，页面不需要自己适配小屏；
- 每页自包含（内联 CSS/JS）；白底黑字为默认风格；
- 公式：MathJax CDN（`cdnjs.cloudflare.com/.../tex-chtml.js`，需要联网）或纯 HTML 上下标排版；
- 交互（滑块、下一步、逐步演示）写在页面内部，用原生 button/input；
- 需要嵌入本地服务（如 localhost 前端）时在页内加 iframe，并提示该页在别的机器不可用。

**样式与排版**（字号、内容量、防越界）：写页面前先读
`references/style.md`，并按其中的检查清单自检。

## 预览与导出

```bash
cd ppt/<演示名>
python3 -m http.server 8000        # 打开 http://127.0.0.1:8000/
```

- 翻页：← → / 空格；全屏：F；导出：Cmd/Ctrl+P → 另存为 PDF。
- 根 html 通过探测 `N.html` 是否存在发现页面：http 下用 fetch HEAD，file:// 下用隐藏 iframe 探测。

## 注意事项

- 页号必须连续且从 1 开始，否则后面页面不会被发现；
- 不要修改根 `index.html` 的扫描/导航逻辑；样式想统一调，直接在页面 html 里改；
- 给他人看之前，确认公式（MathJax 需要联网）与本地嵌入（如 localhost）不依赖本机；
- `ppt/` 应被 git 忽略（工具目录的 `.gitignore` 已忽略 `ppt/` 与 `ppts/`），交付用 zip，不入库。
