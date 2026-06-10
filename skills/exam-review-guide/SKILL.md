---
name: exam-review-guide
description: Build a complete, well-highlighted exam review guide in Markdown from all the lecture slides and documents (ppt, pptx, doc, docx, pdf, md, txt) found in the current project directory. Use when the user mentions exam review, 复习资料, 复习提纲, 考点整理, 期末复习, 考试重点, or asks to summarize course materials for an exam.
---

# Exam Review Guide

Use this skill to turn a folder of course materials (slides, handouts, notes in ppt/pptx/doc/docx/pdf/md/txt) into one Markdown exam review document with clear importance markers. Default output language is Chinese unless the user asks otherwise.

## Hard Rules

- Never invent content that is not in the source files. Every knowledge point must come from the extracted material.
- If a file cannot be parsed, list it explicitly in the final output under "未能解析的文件" instead of silently skipping it.
- Keep the review document self-contained: a student should be able to revise from it without opening the original files.
- Mark importance honestly. Do not mark everything as 必考; the markers lose meaning if overused.
- If the directory contains no supported files, say so and stop; do not produce an empty template.
- Default output file is `复习资料.md` in the workspace root, unless the user names a course (then use `<课程名>-复习资料.md`) or gives an explicit name.

## Workflow

### Step 1: Scan for source files

Recursively find all supported files in the current project directory:

```bash
find . -type f \( -iname "*.ppt" -o -iname "*.pptx" -o -iname "*.doc" -o -iname "*.docx" -o -iname "*.pdf" -o -iname "*.md" -o -iname "*.txt" \) -not -path "*/.*"
```

List the files found to the user so coverage is traceable. If filenames suggest an ordering (chapter numbers, lecture numbers, dates), process them in that order.

Since `.md`/`.txt` matching also catches non-course files, exclude files that are clearly not course material (e.g. `README.md`, `LICENSE.txt`, config or tool-generated notes) and tell the user which ones were skipped. When unsure whether a file is course material, ask instead of guessing.

### Step 2: Extract text

Run the bundled extractor on the directory:

```bash
python3 <SKILL_DIR>/scripts/extract_text.py . -o extracted_text.md
```

Dependencies:

- `.pptx` needs `python-pptx`, `.docx` needs `python-docx`, `.pdf` prefers `pymupdf`. If missing, install first: `pip install python-pptx python-docx pymupdf` (or `uv pip install --system ...`).
- If `pymupdf` cannot be installed, the script falls back to the `pdftotext` command automatically.
- `.md`/`.txt` files are read directly with no dependency. The extractor automatically skips its own output (`extracted_text.md`) and generated `复习资料*.md` files.
- Legacy `.ppt`/`.doc` files cannot be parsed directly. If `soffice` is available, convert them first (`soffice --headless --convert-to pptx/docx`); otherwise report them as unparsed.

The extractor labels every slide and page (e.g. `[Slide 3]`, `[Page 12]`); `.md`/`.txt` files are included verbatim under their filename. Read `extracted_text.md` afterwards; for large courses read it section by section rather than all at once.

### Step 3: Organize the content

- Group the material by chapter or lecture, following the natural order of the course.
- Merge duplicated knowledge points that appear in multiple files; keep the most complete statement.
- Keep definitions, formulas, algorithms, comparison points, and worked examples; drop administrative slides (syllabus logistics, homework deadlines, contact info).

### Step 4: Identify and rank key points

Use these signals to assign importance:

- Explicit markers in the slides: 考试, 考点, 重点, 必考, 掌握, 要求, important, key, required, exam.
- Repetition: concepts appearing in several lectures or repeated in a review/summary slide.
- Worked examples and exercises: the underlying knowledge point is likely tested.
- Emphasis structure: items in summary slides, boxed/bold content, "本章小结" sections.

Importance markers:

| Marker | Meaning |
|--------|---------|
| ⭐⭐⭐ 必考 | Explicitly flagged for the exam, or heavily repeated with exercises |
| ⭐⭐ 重点 | Core concept of a chapter, likely tested |
| ⭐ 了解 | Background or context, low probability |

Mark common mistakes and easily-confused pairs with a `> ⚠️` blockquote.

### Step 5: Write the review document

Follow the structure in [references/review-template.md](references/review-template.md). Required sections:

1. 课程概览与考点地图 — per-chapter table with importance levels
2. 各章节核心知识点 — definitions, formulas, comparison tables, with markers
3. 高频考点汇总 — all ⭐⭐⭐ items collected in one list
4. 易错点与辨析 — confusion pairs and common mistakes
5. 公式/术语速查表 — quick-reference table
6. 自测清单 — checkbox list for self-testing

End the document with a "资料来源" section listing every parsed file, plus "未能解析的文件" if any failed.

## Writing Style

- Plain, compact, student-facing wording. No filler transitions.
- Prefer tables and bullet lists over long paragraphs.
- Keep formulas in LaTeX (`$...$` / `$$...$$`) when the source contains math.
- Keep code snippets short and only when the course content is code-based.
- Use the source's own terminology consistently; do not rename concepts.

## Final Check

Before finishing, verify:

- every source file is either covered in the document or listed as unparsed
- importance markers are used selectively, not on everything
- all ⭐⭐⭐ items also appear in the 高频考点汇总 section
- the output is a single valid Markdown file with the requested or default filename
- no fabricated content was added beyond what the sources support
