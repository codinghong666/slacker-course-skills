# slacker-course-skills

水课生存技能包。

## 有什么

### lab-report 实验报告

把代码和实验材料变成一份正常的实验报告，docx 格式。用词朴素，结构齐全但不过度，不会一股 AI 味。缺什么材料它会直接说，不瞎编数据和截图。

适合：计算机类实验课，要交 Word 报告的那种。

### exam-review-guide 考试复习资料

把一个文件夹里的课件（ppt、pptx、docx、pdf，连 md 和 txt 笔记也行）全部读一遍，整理成一份 Markdown 复习资料。考点按 ⭐⭐⭐ 必考 / ⭐⭐ 重点 / ⭐ 了解 分级，易错点单独标出来，最后还有公式速查表和自测清单。

适合：考前一周，课件攒了一学期没看的时候。

## 怎么用

把 `skills/` 下的目录复制到 `~/.cursor/skills/`（或项目里的 `.cursor/skills/`），然后正常使用 Agent 即可，比如"帮我写这次的实验报告"或者"根据这个文件夹的课件给我整理复习资料"。

依赖按需装：`pip install python-pptx python-docx pymupdf`。

## 免责声明

工具只负责整理和排版，学没学进去是另一回事。
