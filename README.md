# 文件拆分合并工具

一个**纯前端、单文件**的文本拆分与合并工具。把 `index.html` 保存到本地双击打开，或部署到任意静态托管即可使用，无需安装、无需构建、无需后端。

> 所有文件都在浏览器本地读取，**不会上传到任何服务器**。

## 功能概览

### 文件拆分

- 按字符数拆分，默认每片 300 字符（可设置 1 ~ 100000）。
- 可自定义前缀 / 后缀模板，内置三位序号，默认 `start.001=` 与 `=end.001=`，序号自动递增；超过 999 片自然进位为 4 位。
- 支持「尽量不切断整行」，在片内回退到最后一个换行处断开。
- 输入方式：拖拽文件（支持多选，按顺序拼接）、点击选择文件、直接粘贴或输入文本。
- 结果区提供每片独立的「复制 001 / 002 …」按钮。
- 提供**迭代复制**按钮：每点一次按序号依次复制下一片，并显示「已复制 x / n」进度，全部完成后提示已完成，可重置。
- 支持复制全部（空行分隔）与下载分片 `chunks.txt`。

### 文件合并

- 通过前缀 / 后缀定位片段，前缀与后缀中的序号需一致（如 `start.003=` 配 `=end.003=`）。
- **校验模式**：「仅校验」只输出报告，不输出合并结果，便于先确认数据完整性。
- 校验报告分三态：
  - 错误（✗）：未找到配对片段、模板缺少数字序号、前后缀序号不匹配 —— 会阻止合并。
  - 警告（⚠）：序号缺失、重复序号、顺序错乱 —— 仍可继续合并。
  - 通过（✓）：序号完整、配对齐全且顺序正确。
- **序号错乱自动重排**：合并时按序号数值升序重新排序，避免 `10` 排在 `9` 前面。
- 可选「保留前后缀」与片间分隔符（无分隔 / 换行分隔）。
- 支持复制合并结果与下载 `merged.txt`。

## 快速开始

### 本地使用

1. 下载或克隆本仓库。
2. 用浏览器打开 `index.html`（Chrome / Edge / Safari / Firefox 均可）。
3. 在「文件拆分」或「文件合并」标签页中操作。

无需 Node.js，无需任何依赖安装。

### 部署到 GitHub Pages

1. 新建空仓库（Public），把 `index.html` 与 `.nojekyll` 推送到 `main` 分支根目录。
2. Settings → Pages → Source 选择 `Deploy from a branch`，分支 `main`、目录 `/(root)`，保存。
3. 约 1 ~ 2 分钟后访问 `https://你的用户名.github.io/仓库名/`。

> `.nojekyll` 为空文件，用于跳过 Jekyll 构建。

其他静态托管（Vercel、Netlify、阿里云 OSS、腾讯云 COS、Nginx）同样只需上传这一个 HTML 文件。

## 使用示例

**拆分**：输入 1000 个字符、拆分字符数填 300，得到 4 片，序号 `001 ~ 004`，末片 100 字符；第 2 片形如：
start.002=<第 301 ~ 600 个字符>=end.002=


**合并**：把若干片段（可乱序、可夹杂多余文字）粘贴进来，点击「校验并合并」，工具会提取配对片段、按序号重排后还原原文。若缺少 `003`，报告会以警告形式列出缺失序号。

## 快捷键

| 快捷键 | 作用 |
|---|---|
| `Ctrl / Cmd + Enter` | 在当前标签页执行「开始拆分」或「校验并合并」 |

## 技术说明

- 单文件实现：HTML + CSS + JavaScript 全部内联在 `index.html` 中，零外部依赖、零网络请求。
- 文本以 UTF-8 读取，自动归一化换行为 `\n`，并清理零宽字符与 BOM。
- 序号格式化使用 `padStart(3, '0')`；排序使用数值比较。
- 复制优先使用 Clipboard API，失败时降级为 `document.execCommand('copy')`。
- 下载通过 `Blob` + `URL.createObjectURL` 实现，并包含 `msSaveBlob` 与 data URI 兜底。

### 注意事项

- 若把本页面嵌入其他站点的 `<iframe>` 中，需要为 iframe 添加 `sandbox="allow-scripts allow-downloads"`，否则下载会被浏览器静默拦截。
- 超大文本（数 MB 以上）在渲染分片列表时可能略有延迟，建议分批处理。
- 仅支持文本类文件（txt / md / csv / json 等），二进制文件无法正确解析。

## 隐私

工具完全在浏览器端运行，不发送任何网络请求，不收集、不上传任何文件内容或使用数据。

## 许可

MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
