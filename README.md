# TextConverter

## 主要功能
- `index.html`：美观的转换页面（会调用后端API）
- `server.js`：Node.js 转换服务（示例实现）
- 支持（本ZIP落地的三类输出）：
  - Office（Word/Excel/PPT） → PDF（LibreOffice）
  - PDF → 图片 PNG（Poppler: pdftoppm）
  - PDF → TXT（Poppler: pdftotext）
  - 图片 → TXT（OCR，可选：Tesseract）
  - 图片 → PDF（pdf-lib）
  - TXT → PDF（pdf-lib）
  - PDF 指定页抽取（pdf-lib），再转图片/导出PDF

> 说明：真正做到“PDF→Word/Excel/PPT 且高质量还原”需要额外工具（通常不完美）。

## 运行步骤（本地）
1) 安装 Node.js（18+ 推荐）
2) 安装 npm 依赖：
```bash
npm i
```

3) 安装系统依赖（必须）
- LibreOffice（提供 `soffice` 命令）
- Poppler（提供 `pdftoppm` 与 `pdftotext`）
- 可选：Tesseract（提供 `tesseract`，用于图片OCR）

4) 启动后端：
```bash
npm run start
# 默认 http://localhost:3000
```

5) 打开 `index.html`（用浏览器打开即可），上传文件并转换。

## API
POST `http://localhost:3000/api/convert`

表单字段：
- files: 文件（multipart，支持多文件；示例只处理第一个）
- fromFmt: auto/word/pdf/excel/ppt/txt/image（展示用）
- toFmt: pdf/image/txt
- mode: direct/pages
- pageSpec: 1-3,5,8-10（仅 pages 用）
- quality: balanced/high/small（仅 PDF→图片 用）

返回：
```json
{ "ok": true, "outputs": ["/download/xxx.pdf"] }
```

