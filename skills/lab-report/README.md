# 实验报告 Skill

一个专门写实验报告的 Agent Skill。

这一版先只做实验报告，不把课程论文、普通作业、课程总结、反思日志这些东西揉进来。后面要扩展，就按分类一个个加。

## 适用场景

- 中文或英文实验报告
- 计算机实验报告
- 代码型实验说明
- 实验结果与分析整理
- docx 实验报告生成（需要时用 LibreOffice 导出 PDF）
- 实验报告润色，去掉太明显的 AI 味

## 不适用场景

- 普通课程问答
- 课程论文
- 课程总结
- 读后感、心得体会
- 非实验类水课作业

这些后续可以单独做成别的 skill。

## 目录结构

```
SKILL.md                 # 薄。何时用、规则、报告结构
references/docx-build.md  # 生成 docx 的细节，按需读
scripts/build_docx.py     # python-docx 封装，复用的构建函数
```

## 依赖

- 生成 docx：`pip install python-docx`（MIT 许可，可随仓库分发）
- 导出 PDF（可选）：LibreOffice（`soffice`）
- 若 runtime 已装官方 `docx` skill，会优先交给它生成；本仓库不内置任何第三方专有 skill。

## 安装

把本仓库克隆到对应 runtime 的 skills 目录即可。

Codex CLI:

```bash
git clone <this-repo-url> ~/.codex/skills/lab-report
```

Claude Code:

```bash
git clone <this-repo-url> ~/.claude/skills/lab-report
```

## 使用

安装后可以这样触发：

```text
用 lab-report 帮我写这个实验报告
根据这段代码生成实验报告
把这份实验报告改得像本科生写的，不要太 AI
生成实验报告的 docx（需要的话再导出 PDF）
```

## 隐私规则

Skill 不内置姓名、学号、班级、课程号等个人信息。需要这些内容时，必须由用户在当前任务中明确提供。
