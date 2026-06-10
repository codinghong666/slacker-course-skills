---
name: lab-report
description: Use this skill when the user wants to write, rewrite, polish, or generate an experiment report or lab report, especially for computer science or code-based labs. Keep the report student-like, concise, verifiable, and free of fabricated results or personal metadata.
---

# Lab Report

Use this skill only for experiment reports and lab reports. It can handle Chinese or English reports, university or non-university labs. Do not use it for ordinary course essays, general homework answers, course summaries, reflection journals, or other tasks unless the task is explicitly an experiment report.

## Goals

- Write like an undergraduate experiment report.
- Use the language requested by the user; if the user does not specify one, default to Chinese.
- Keep the structure complete but not overbuilt.
- Keep wording plain, short, and student-like.
- Avoid obvious AI tone.
- Keep code concise and course-level.
- Do not add code comments unless the user explicitly asks.

## Hard Rules

- Do not fabricate experiment results, data, screenshots, references, environment details, names, student IDs, classes, or submission metadata.
- If key material is missing, state the missing items first.
- Follow the user's report template, rubric, naming rule, and required format before this skill's defaults.
- If the user provides a submission naming rule, follow it exactly.
- If no naming rule is provided, use neutral filenames such as `report.docx`, `report.pdf`, `code/`, or `src/`.
- The report should sound like a normal student submission, not a formal paper, product document, or AI summary.
- Code, experiment steps, result, and analysis must match each other.
- If the report needs runnable code and the environment allows it, run the code and use the real output.
- When writing a docx report, put both the code and the actual run result into the document.

## Default Report Structure

Use this order unless the user provides another template:

1. 实验题目
2. 实验目的
3. 实验环境
4. 实验内容
5. 实验设计或实现方法
6. 核心代码
7. 实验结果与分析
8. 遇到的问题及解决方法
9. 实验总结

For short or low-stakes lab reports, keep each section brief. Do not add long background sections unless the assignment requires them.

## Output Format

When the user asks for a file, default to a Word `.docx` report. 

- source file: `report.docx` or the required submission name with `.docx`
- PDF: export with LibreOffice only when the user asks for one
- If an official `docx` skill is installed in the runtime, prefer it to build the file; otherwise use the bundled helper.
- For how to build it (tooling, fonts, code/output blocks, tables, PDF export), read `references/docx-build.md` and use the helper at `scripts/build_docx.py`.
- Put both the code and its actual run output into the document. Use real tables, not spaces. Never invent screenshots.

## Working Method

### When Material Is Incomplete

Do not guess missing facts.

State the missing items clearly, such as:

- experiment requirements
- input data
- expected or actual runtime result
- screenshots
- environment version
- required name, student ID, class, or submission format

Then choose the least risky output:

- produce a fillable lab report draft
- write only the sections supported by the given material
- ask for the missing result if the result section is required

### When The User Gives Code

- Base the report on the given code.
- Do not rewrite the code into a different style unless the user asks.
- Keep the explanation aligned with the actual logic.
- If the code quality is poor but usable, describe it plainly instead of beautifying the explanation too much.
- If execution is required and the environment allows it, run the code and record the actual output.

### When The User Gives Only A Lab Title Or Requirement

- Build a conservative report framework first.
- Avoid making up measured data or screenshots.
- Use generic environment descriptions only when clearly safe.
- Mark uncertain parts as pending completion.
- If the task requires implementation, create a minimal working solution first, then run it before writing the result section.

## Writing Style

- Prefer plain statements.
- Prefer short paragraphs.
- Avoid long theoretical background.
- Focus on what was done, how it was done, and what result appeared.
- Avoid phrases that sound like formal research writing.
- Avoid phrases that sound like AI transition text.
- Keep the tone slightly ordinary; do not make the work look far above the course level.

## Code Style

- No comments unless requested.
- Simple variable names are acceptable if clear.
- Prefer direct implementations.
- Avoid unnecessary abstraction.
- Avoid advanced syntax unless the course level or user code already uses it.
- Keep the solution close to what a student would normally submit.
- When output is needed in the report, prefer code that is easy to run and easy to verify.

## Output Modes

Choose the mode that fits the request:

- full lab report
- fillable lab report template
- one section rewrite
- code explanation for a lab report
- lab report polishing
- AI-tone reduction rewrite
- docx lab report built with `python-docx` (PDF via LibreOffice when requested)

## Final Check

Before finishing, verify:

- the report uses the language requested by the user, or Chinese if no language was specified
- the task is an experiment report or lab report
- the tone sounds like an undergraduate lab report
- no code comments were added unless requested
- no fabricated results, screenshots, or personal metadata were introduced
- the structure is complete or missing parts are explicitly marked
- the wording is concise and not overloaded with connectors
- if code was required, the code was actually run when possible
- if the report is a docx file, the code and actual run result were put into the document
- if the report is docx, the file was built with `python-docx` and opens as a valid document
- if a PDF was requested, `soffice --headless --convert-to pdf` ran successfully
- if the user provided a template path, the generated file follows that template instead of the default layout
