# Invoice Manager

OCR识别PDF发票，按模板生成报销汇总表，按行程自动归档。

## 功能

- 📄 OCR 识别 PDF 发票（增值税发票、电子发票、酒店账单等）
- 📊 按模板生成报销汇总 Excel
- 📁 按行程自动归类归档

## 依赖

```bash
# 系统依赖
brew install poppler tesseract tesseract-lang

# Python 依赖
pip install openpyxl pdf2image pytesseract
```

## 目录结构

```
invoice-manager/
├── SKILL.md              # Agent Skill 定义
├── scripts/
│   ├── process_invoices.py   # OCR 识别
│   └── generate_summary.py   # 汇总表生成 + 归档
├── templates/
│   └── 报销模板.xlsx        # 报销汇总模板（需自行填入选项）
├── input/                # 待处理发票 PDF（已 gitignore）
└── archive/              # 归档输出（已 gitignore）
```

## 使用方式

### 1. OCR 识别

将 PDF 发票放入 `input/` 目录，运行：

```bash
python3 scripts/process_invoices.py input/ --output /tmp/invoice_results.json
```

### 2. 确认信息

检查 OCR 结果，确认金额、日期、费用类型、地点是否正确。

### 3. 生成汇总表 + 归档

准备 JSON 数据（确认后的发票信息），运行：

```bash
python3 scripts/generate_summary.py invoices.json --month 2026-03
```

JSON 格式示例：

```json
[
  {
    "original": "xxx.pdf",
    "start": "2026-03-13",
    "end": "2026-03-13",
    "location": "上海",
    "amount": 88.00,
    "type": "餐费",
    "project": "项目名称",
    "new_name": "20260313_餐费_销售方.pdf",
    "folder": "2026-03上海项目名称差旅"
  }
]
```

## 归档规则

- 汇总表：`archive/{年月}_报销汇总.xlsx`（一个月一个，支持追加）
- 子文件夹：`archive/{年月}{出差地}{项目名称}差旅/`
- PDF 重命名：`{日期}_{费用类型}_{销售方简述}.pdf`

## 费用类型

餐费、住宿费、差旅费用、市内交通费（含路桥费）、通讯费、信息服务费、快递费用、招待费/礼品费、其他费用

## License

MIT
