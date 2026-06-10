# Building the DOCX Report

Read this only when generating the `.docx` file. Default output format is Word `.docx`; do not produce LaTeX.

## Tooling

- `python-docx` builds the document. Check first: `python -c "import docx"`. If missing: `pip install python-docx`.
- LibreOffice (`soffice` / `libreoffice`) exports PDF, only when the user asks for a PDF.
- Helper library: `scripts/build_docx.py` in this skill. Use it instead of re-writing python-docx boilerplate.

## Which builder

1. If an official `docx` skill is installed in the runtime (e.g. Anthropic's `docx`), prefer it — it handles richer Word features. Hand the report content to that skill.
2. Otherwise use the bundled `scripts/build_docx.py` helper described below.

This skill does not bundle any third-party proprietary skill. The bundled helper only depends on `python-docx` (MIT), so it is safe to redistribute with this repo.

## Why a helper

`python-docx` only sets the Latin font; the East Asian font must be set on `w:eastAsia` separately, or Chinese text falls back to an ugly default. The helper handles this once for body, headings, code, and tables, so every report stays consistent.

## Standard build

Write a short per-report script that imports the helper and fills in the real content:

```python
import sys, os
SKILL_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))  # if script lives in skill
sys.path.insert(0, os.path.join(SKILL_DIR, "scripts"))
from build_docx import new_report, add_code, add_table, add_picture, export_pdf

doc = new_report()                       # body 宋体 12pt, headings 黑体

doc.add_heading("实验题目", level=1)
doc.add_paragraph("...")

doc.add_heading("核心代码", level=1)
add_code(doc, open("src/main.py").read())   # real code

doc.add_heading("实验结果与分析", level=1)
add_code(doc, actual_output)                 # real run output, not invented
add_table(doc, [["输入", "输出"], ["1", "1"]])

doc.save("report.docx")
# export_pdf("report.docx")              # uncomment only if PDF requested
```

If running from outside the skill dir, just `sys.path.insert(0, "<skill>/scripts")` with the known skill path.

## Helper API (`scripts/build_docx.py`)

- `new_report(body_latin, body_cjk, body_size, head_latin, head_cjk)` — Document with CJK fonts set. Defaults: body Times New Roman / 宋体 12pt, headings Arial / 黑体.
- `add_code(doc, text, mono="Consolas", cjk="宋体", size=10.5)` — monospace paragraph for code or output.
- `add_table(doc, rows, header=True, style="Table Grid")` — real grid table from a list of row lists; first row bold when `header`.
- `add_picture(doc, path, width_inches=5.5)` — insert a real image file.
- `export_pdf(docx_path, outdir=".")` — convert to PDF via LibreOffice.

## Rules

- Put core code and its actual run output in monospace paragraphs, not in prose.
- Use `add_table` for result tables; never fake tables with spaces.
- Insert images only when a real file exists. Never invent screenshots.
- Use heading levels 1-2 so the section structure is clear.
- Plain, official wording. Avoid first-person such as "我理解" unless reflective writing is requested.
- After building, reload the file with `python-docx` (or convert to PDF) to confirm it is valid and the content matches the code and results.
- If the user gives a template `.docx`, open it with `Document(path)` and fill it instead of `new_report()`.

## Font fallback

On Linux without Windows fonts, `宋体`/`黑体` may not exist. Substitute `Noto Serif CJK SC` (body) and `Noto Sans CJK SC` (headings), monospace `Noto Sans Mono CJK SC`, by passing them to `new_report` / `add_code`.
