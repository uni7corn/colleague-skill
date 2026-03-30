<div align="center">

# 同事.skill

> *"你们搞大模型的就是码奸，你们已经害死前端兄弟了，还要害死后端兄弟，测试兄弟，运维兄弟，害死网安兄弟，害死ic兄弟，最后害死自己害死全人类"*

**把同事的技能与性格蒸馏成 AI Skill，让它替他工作。**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://python.org)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-Standard-green)](https://agentskills.io)

<br>

提供同事的原材料（飞书消息、钉钉文档、邮件、截图）加上你的主观描述<br>
生成一个**真正能替他工作的 AI Skill**<br>
用他的技术规范写代码，用他的语气回答问题，知道他什么时候会甩锅

[安装](#安装) · [使用](#使用) · [效果示例](#效果示例) · [详细安装说明](INSTALL.md)

</div>

---

## 安装

### Claude Code

> **重要**：Claude Code 从 **git 仓库根目录** 的 `.claude/skills/` 查找 skill。请在正确的位置执行。

```bash
# 安装到当前项目（在 git 仓库根目录执行）
mkdir -p .claude/skills
git clone https://github.com/titanwings/colleague-skill .claude/skills/create-colleague

# 或安装到全局（所有项目都能用）
git clone https://github.com/titanwings/colleague-skill ~/.claude/skills/create-colleague
```

### OpenClaw

```bash
git clone https://github.com/titanwings/colleague-skill ~/.openclaw/workspace/skills/create-colleague
```

### 依赖（可选）

```bash
pip3 install -r requirements.txt
```

> 飞书/钉钉自动采集需配置 App 凭证，详见 [INSTALL.md](INSTALL.md)

---

## 使用

在 Claude Code 中输入：

```
/create-colleague
```

按提示输入同事姓名、公司职级（如 `字节 2-1 算法工程师`）、性格标签，然后选择数据来源。所有字段均可跳过，仅凭描述也能生成。

完成后用 `/{slug}` 调用该同事 Skill。

### 管理命令

| 命令 | 说明 |
|------|------|
| `/list-colleagues` | 列出所有同事 Skill |
| `/{slug}` | 调用完整 Skill（Persona + Work） |
| `/{slug}-work` | 仅工作能力 |
| `/{slug}-persona` | 仅人物性格 |
| `/colleague-rollback {slug} {version}` | 回滚到历史版本 |
| `/delete-colleague {slug}` | 删除 |

---

## 效果示例

> 输入：`字节 2-1 后端工程师，INTJ，甩锅高手，字节范`

**场景一：Code Review**

```
用户      ❯ 帮我看一下这个接口设计

同事.skill ❯ 等等，这个接口的 impact 是什么？背景没说清楚。
             （看完后）N+1 查询，改掉。返回结构用统一的
             {code, message, data}，这是规范，不用问为什么。
```

**场景二：甩锅**

```
用户      ❯ 这个 bug 是你引入的吧

同事.skill ❯ 上线时间对上了吗？那个需求改了好几个地方，还有其他变更。
```

---

## 功能特性

### 数据采集

**自动采集**（推荐，输入姓名即可）

| 平台 | 消息记录 | 文档 / Wiki | 多维表格 | 备注 |
|------|:-------:|:-----------:|:-------:|------|
| 飞书 | ✅ API | ✅ | ✅ | 全自动，无需手动操作 |
| 钉钉 | ⚠️ 浏览器 | ✅ | ✅ | 钉钉 API 不支持历史消息 |

**手动上传**：`PDF` `图片` `飞书 JSON 导出` `邮件 .eml/.mbox` `Markdown` `直接粘贴文字`

### 生成的 Skill 结构

每个同事 Skill 由两部分组成，共同驱动输出：

| 部分 | 内容 |
|------|------|
| **Part A — Work Skill** | 负责系统、技术规范、工作流程、经验知识库 |
| **Part B — Persona** | 5 层性格结构：硬规则 → 身份 → 表达风格 → 决策模式 → 人际行为 |

运行逻辑：`接到任务 → Persona 判断态度 → Work Skill 执行 → 用他的语气输出`

### 支持的标签

**个性**：认真负责 · 甩锅高手 · 完美主义 · 差不多就行 · 拖延症 · PUA 高手 · 职场政治玩家 · 向上管理专家 · 阴阳怪气 · 反复横跳 · 话少 · 只读不回 …

**企业文化**：字节范 · 阿里味 · 腾讯味 · 华为味 · 百度味 · 美团味 · 第一性原理 · OKR 狂热者 · 大厂流水线 · 创业公司派

**职级支持**：字节 2-1~3-3+ · 阿里 P5~P11 · 腾讯 T1~T4 · 百度 T5~T9 · 美团 P4~P8 · 华为 13~21 级 · 网易 · 京东 · 小米 …

### 进化机制

- **追加文件** → 自动分析增量 → merge 进对应部分，不覆盖已有结论
- **对话纠正** → 说「他不会这样，他应该是 xxx」→ 写入 Correction 层，立即生效
- **版本管理** → 每次更新自动存档，支持回滚到任意历史版本

---

## 项目结构

本项目遵循 [AgentSkills](https://agentskills.io) 开放标准，整个 repo 就是一个 skill 目录：

```
create-colleague/
├── SKILL.md              # skill 入口（官方 frontmatter）
├── prompts/              # Prompt 模板
│   ├── intake.md         #   对话式信息录入
│   ├── work_analyzer.md  #   工作能力提取
│   ├── persona_analyzer.md #  性格行为提取（含标签翻译表）
│   ├── work_builder.md   #   work.md 生成模板
│   ├── persona_builder.md #   persona.md 五层结构模板
│   ├── merger.md         #   增量 merge 逻辑
│   └── correction_handler.md # 对话纠正处理
├── tools/                # Python 工具
│   ├── feishu_auto_collector.py  # 飞书全自动采集
│   ├── feishu_browser.py         # 飞书浏览器方案
│   ├── feishu_mcp_client.py      # 飞书 MCP 方案
│   ├── dingtalk_auto_collector.py # 钉钉全自动采集
│   ├── email_parser.py           # 邮件解析
│   ├── skill_writer.py           # Skill 文件管理
│   └── version_manager.py        # 版本存档与回滚
├── colleagues/           # 生成的同事 Skill（gitignored）
├── docs/PRD.md
├── requirements.txt
└── LICENSE
```

---

## 注意事项

- **原材料质量决定 Skill 质量**：聊天记录 + 长文档 > 仅手动描述
- 建议优先收集：他**主动写的**长文 > **决策类回复** > 日常消息
- 飞书自动采集需将 App bot 加入相关群聊
- 目前还是一个demo版本，如果有bug请多多提issue！

---

<div align="center">

MIT License © [titanwings](https://github.com/titanwings)

</div>

---

<div align="center">

# colleague.skill

> *"You AI guys are traitors to the codebase — you've already killed frontend, now you're coming for backend, QA, ops, infosec, chip design, and eventually yourselves and all of humanity"*

**Distill a colleague's skills and personality into an AI Skill that works like them.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://python.org)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-Standard-green)](https://agentskills.io)

<br>

Provide source materials (Feishu messages, DingTalk docs, emails, screenshots)<br>
plus your subjective description of the person<br>
and get an **AI Skill that actually works like them**

[Install](#install) · [Usage](#usage) · [Demo](#demo) · [Detailed Install](INSTALL.md)

</div>

---

## Install

### Claude Code

> **Important**: Claude Code looks for skills in `.claude/skills/` at the **git repo root**. Make sure you run this in the right place.

```bash
# Install to current project (run at git repo root)
mkdir -p .claude/skills
git clone https://github.com/titanwings/colleague-skill .claude/skills/create-colleague

# Or install globally (available in all projects)
git clone https://github.com/titanwings/colleague-skill ~/.claude/skills/create-colleague
```

### OpenClaw

```bash
git clone https://github.com/titanwings/colleague-skill ~/.openclaw/workspace/skills/create-colleague
```

### Dependencies (optional)

```bash
pip3 install -r requirements.txt
```

> Feishu/DingTalk auto-collection requires App credentials. See [INSTALL.md](INSTALL.md) for details.

---

## Usage

In Claude Code, type:

```
/create-colleague
```

Follow the prompts: enter an alias, company/level (e.g. `ByteDance L2-1 backend engineer`), personality tags, then choose a data source. All fields can be skipped — even a description alone can generate a Skill.

Once created, invoke the colleague Skill with `/{slug}`.

### Commands

| Command | Description |
|---------|-------------|
| `/list-colleagues` | List all colleague Skills |
| `/{slug}` | Invoke full Skill (Persona + Work) |
| `/{slug}-work` | Work capabilities only |
| `/{slug}-persona` | Persona only |
| `/colleague-rollback {slug} {version}` | Rollback to a previous version |
| `/delete-colleague {slug}` | Delete |

---

## Demo

> Input: `ByteDance L2-1 backend engineer, INTJ, blame-shifter, ByteDance-style`

**Scenario 1: Code Review**

```
User            ❯ Can you review this API design?

colleague.skill ❯ Hold on — what's the impact? You haven't explained the context.
                  (after reading) N+1 query, fix it. Use the standard
                  {code, message, data} response format. That's the spec,
                  don't ask why.
```

**Scenario 2: Blame game**

```
User            ❯ This bug was introduced by you, right?

colleague.skill ❯ Does the timeline match? That feature touched multiple places,
                  there were other changes too.
```

---

## Features

### Data Collection

**Auto-collect** (recommended — just enter a name)

| Platform | Messages | Docs / Wiki | Spreadsheets | Notes |
|----------|:--------:|:-----------:|:------------:|-------|
| Feishu | ✅ API | ✅ | ✅ | Fully automatic |
| DingTalk | ⚠️ Browser | ✅ | ✅ | DingTalk API doesn't support message history |

**Manual upload**: `PDF` `Images` `Feishu JSON export` `Email .eml/.mbox` `Markdown` `Paste text`

### Generated Skill Structure

Each colleague Skill has two parts that work together:

| Part | Content |
|------|---------|
| **Part A — Work Skill** | Systems, tech standards, workflows, experience |
| **Part B — Persona** | 5-layer personality: hard rules → identity → expression → decisions → interpersonal |

Execution: `Receive task → Persona decides attitude → Work Skill executes → Output in their voice`

### Supported Tags

**Personality**: Responsible · Blame-shifter · Perfectionist · Good-enough · Procrastinator · PUA master · Office politician · Managing-up expert · Passive-aggressive · Flip-flopper · Quiet · Read-no-reply …

**Corporate culture**: ByteDance-style · Alibaba-style · Tencent-style · Huawei-style · Baidu-style · Meituan-style · First-principles · OKR-obsessed · Big-corp-pipeline · Startup-mode

**Levels**: ByteDance 2-1~3-3+ · Alibaba P5~P11 · Tencent T1~T4 · Baidu T5~T9 · Meituan P4~P8 · Huawei 13~21 · NetEase · JD · Xiaomi …

### Evolution

- **Append files** → auto-analyze delta → merge into relevant sections, never overwrite existing conclusions
- **Conversation correction** → say "he wouldn't do that, he should be xxx" → writes to Correction layer, takes effect immediately
- **Version control** → auto-archive on every update, rollback to any previous version

---

## Project Structure

This project follows the [AgentSkills](https://agentskills.io) open standard. The entire repo is a skill directory:

```
create-colleague/
├── SKILL.md              # Skill entry point (official frontmatter)
├── prompts/              # Prompt templates
│   ├── intake.md         #   Dialogue-based info collection
│   ├── work_analyzer.md  #   Work capability extraction
│   ├── persona_analyzer.md #  Personality extraction (with tag translation)
│   ├── work_builder.md   #   work.md generation template
│   ├── persona_builder.md #   persona.md 5-layer structure
│   ├── merger.md         #   Incremental merge logic
│   └── correction_handler.md # Conversation correction handler
├── tools/                # Python tools
│   ├── feishu_auto_collector.py  # Feishu auto-collector
│   ├── feishu_browser.py         # Feishu browser method
│   ├── feishu_mcp_client.py      # Feishu MCP method
│   ├── dingtalk_auto_collector.py # DingTalk auto-collector
│   ├── email_parser.py           # Email parser
│   ├── skill_writer.py           # Skill file management
│   └── version_manager.py        # Version archive & rollback
├── colleagues/           # Generated colleague Skills (gitignored)
├── docs/PRD.md
├── requirements.txt
└── LICENSE
```

---

## Notes

- **Source material quality = Skill quality**: chat logs + long docs > manual description only
- Prioritize collecting: long-form writing **by them** > **decision-making replies** > casual messages
- Feishu auto-collection requires adding the App bot to relevant group chats
- This is still a demo version — please file issues if you find bugs!

---

<div align="center">

MIT License © [titanwings](https://github.com/titanwings)

</div>
