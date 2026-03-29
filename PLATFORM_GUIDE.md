# Platform Adaptation Guide / 平台适配指南

> This document explains how to use Industry Oracle across different AI platforms, and how to configure your own MCP tools for search, document generation, and more.
>
> 本文档说明如何在不同 AI 平台上使用 Industry Oracle，以及如何配置你自己的 MCP 工具来实现搜索、文档生成等功能。

---

## Platform Compatibility Matrix / 平台兼容性矩阵

| Feature | Mira | Claude Code / OpenClaw | Claude API / claude.ai | ChatGPT / GPTs | Other LLMs |
|---------|------|----------------------|------------------------|-----------------|------------|
| Core 4-step process | ✅ Native | ✅ Full | ✅ Full | ✅ Full | ✅ Full |
| Analysis frameworks | ✅ Auto-load | ✅ Auto-load | ✅ Paste into context | ✅ Paste into context | ✅ Paste into context |
| Document generation | ✅ Feishu docs | ✅ Local `.md` files | ⚠️ Text output only | ⚠️ Text output only | ⚠️ Text output only |
| Web search gap-fill | ✅ Built-in | ✅ Via MCP | ❌ Manual | ✅ With browsing | Varies |
| Material intake (PDF/URL) | ✅ Built-in | ✅ Via tools/MCP | ⚠️ File upload | ✅ File upload | Varies |
| Progressive disclosure | ✅ Auto | ✅ Auto | ❌ Paste manually | ❌ Paste manually | ❌ Paste manually |

---

## Setup by Platform / 各平台安装方式

### 1. Mira (Recommended for Feishu/Lark users / 飞书用户推荐)

Mira natively supports the skill format. Simply install the skill from the repository.

```
Repository structure used directly:
├── SKILL.md              ← Auto-loaded as skill prompt
└── references/           ← Auto-loaded on demand
```

No modifications needed. All features — Feishu doc generation, web search, PDF reading — work out of the box.

无需修改，所有功能开箱即用。

---

### 2. Claude Code (Full Support / 完整支持)

Claude Code uses the **Agent Skills** standard. Industry Oracle is a native Claude Code skill.

**One-command install / 一键安装：**

```bash
git clone https://github.com/chengjialu8888/industry-oracle.git ~/.claude/skills/industry-oracle
```

That's it. Claude Code will automatically detect the `SKILL.md` and `references/` directory. The skill triggers when you mention industry research, competitive analysis, startup analysis, etc.

就这样，Claude Code 会自动检测 `SKILL.md` 和 `references/` 目录。提到行业研究、竞品分析等关键词时自动触发。

**Manual install / 手动安装：**

```bash
# Clone to any location
git clone https://github.com/chengjialu8888/industry-oracle.git

# Copy skill files to Claude Code skills directory
mkdir -p ~/.claude/skills/industry-oracle
cp industry-oracle/SKILL.md ~/.claude/skills/industry-oracle/
cp -r industry-oracle/references/ ~/.claude/skills/industry-oracle/
```

**Output**: Each Step's analysis is saved as a Markdown file in your project's `output/` directory.

#### Configuring MCP Tools (Optional but Recommended) / 配置 MCP 工具（可选但推荐）

Industry Oracle works without any MCP tools — it uses `curl` and local file operations. But adding MCP servers **significantly improves** the search gap-fill and material processing steps.

没有 MCP 工具也能用——会用 `curl` 和本地文件操作。但添加 MCP 服务器会**显著提升**搜索补全和材料处理能力。

Add to your project's `.mcp.json` or `~/.claude/settings.json`:

```jsonc
{
  "mcpServers": {
    // Web search — pick one that suits you
    // 网页搜索——选一个适合你的
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-brave-search"],
      "env": { "BRAVE_API_KEY": "$BRAVE_API_KEY" }
    },

    // Web page fetching — read full articles
    // 网页抓取——读取完整文章
    "fetch": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-fetch"]
    },

    // File system access (if needed beyond project root)
    // 文件系统访问（如果需要访问项目根目录之外的文件）
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-filesystem", "/path/to/research-materials"]
    },

    // Google Drive (if your materials are on GDrive)
    // Google Drive（如果你的材料在 GDrive 上）
    "gdrive": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-gdrive"],
      "env": { "GDRIVE_CREDENTIALS": "$GDRIVE_CREDENTIALS" }
    },

    // Notion (if you want to output to Notion instead of local files)
    // Notion（如果你想输出到 Notion 而不是本地文件）
    "notion": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-notion"],
      "env": { "NOTION_TOKEN": "$NOTION_TOKEN" }
    }
  }
}
```

**Popular MCP combinations by use case / 按使用场景推荐的 MCP 组合：**

| Use Case | Recommended MCPs |
|----------|-----------------|
| **Minimal** (just search) | `brave-search` + `fetch` |
| **Research-heavy** (lots of web sources) | `brave-search` + `fetch` + `puppeteer` (browser) |
| **Document-heavy** (materials on cloud) | `fetch` + `gdrive` or `notion` |
| **Full stack** (search + output + storage) | `brave-search` + `fetch` + `notion` + `filesystem` |

> **Note**: You can use ANY MCP server that provides search or document capabilities. The ones above are examples. Check [MCP Registry](https://github.com/modelcontextprotocol/servers) for the full list.
>
> **注意**：你可以使用任何提供搜索或文档功能的 MCP 服务器。以上只是示例。完整列表请查看 MCP 注册表。

---

### 3. OpenClaw (Full Support / 完整支持)

OpenClaw uses the same Agent Skills format as Claude Code. Industry Oracle works as a native OpenClaw skill.

OpenClaw 使用和 Claude Code 相同的 Agent Skills 格式。Industry Oracle 可以作为原生 OpenClaw 技能使用。

**Install / 安装：**

```bash
git clone https://github.com/chengjialu8888/industry-oracle.git ~/.claude/skills/industry-oracle
```

**OpenClaw-specific notes / OpenClaw 特别说明：**

- The skill uses `SKILL.md` + `references/` directory — fully compatible with OpenClaw's skill loading mechanism
- OpenClaw's token budget per file is 20,000 chars. `SKILL.md` is ~34K chars — it may be truncated. If this happens, use the `adapters/claude-code/CLAUDE.md` (a condensed version at ~11K chars) instead:
  ```bash
  cp adapters/claude-code/CLAUDE.md ~/.claude/skills/industry-oracle/SKILL.md
  ```
- If your OpenClaw agent has MCP tools configured (search, web fetch), the skill will use them automatically
- Output goes to local files. If your agent has a Notion or Google Docs MCP, it can output there instead

---

### 4. Claude API / claude.ai (System Prompt)

For direct Claude API usage or claude.ai conversations:

**Option A — Full context** (if context window allows, ≥64K tokens):
1. Paste the entire `SKILL.md` content (without YAML frontmatter) as the system prompt
2. Append the content of all 3 reference files after the main prompt
3. Remove the "Tools & Format Guide" section
4. Add this at the end:

```
## Output Format
Since you don't have document generation tools, output each Step's analysis
as structured Markdown directly in the conversation. Use clear headers and
formatting so the user can copy the output into their preferred document tool.
```

**Option B — Minimal context** (recommended for shorter context windows, ≥16K tokens):
1. Paste only `SKILL.md` (without YAML frontmatter and Tools section) as system prompt
2. When reaching Step 2, paste `references/frameworks.md` as a user message
3. When reaching Step 3, paste `references/investor-personas.md` as a user message
4. This manually replicates the "progressive disclosure" architecture

---

### 5. ChatGPT / Custom GPTs

**As a Custom GPT**:
1. Create a new GPT
2. In "Instructions", paste the `SKILL.md` content (remove YAML frontmatter)
3. Upload `references/frameworks.md`, `references/investor-personas.md`, and `references/examples.md` as Knowledge files
4. Replace the "Tools & Format Guide" section with:

```
## Output Format
Output each Step's analysis as structured Markdown in the conversation.
When you need reference frameworks (Step 2), investor personas (Step 3),
or quality examples (any Step), retrieve them from your Knowledge files.
For web search gap-fill, use your browsing capability.
```

**As a conversation starter**:
1. Start a new conversation
2. Paste the SKILL.md content as your first message, prefixed with: "Use the following as your operating instructions for this conversation:"
3. Manually paste reference files when needed

---

### 6. Other LLMs (Gemini, Llama, Mistral, DeepSeek, etc.)

The core methodology works with any LLM that:
- Accepts system prompts or instruction preambles
- Has sufficient context window (≥32K tokens recommended; ≥16K minimum with Option B)
- Can follow multi-step structured workflows

**Setup**: Same as "Claude API — Option A or B" above.

**Quality note**: Analysis quality depends heavily on the model's reasoning capability. Models with strong analytical reasoning (Claude Sonnet 4+, GPT-4+, Gemini Pro+, DeepSeek-R1+) will produce significantly better results than smaller models.

---

## Bring Your Own Tools / 自带工具配置

If you're **not on Mira** and want tool-augmented capabilities, here's how to set up equivalent functionality with your own tools:

如果你**不在 Mira 上**，想要工具增强的功能，以下是如何用你自己的工具实现等效功能：

### Search (for Step 0 Gap-Fill) / 搜索（用于 Step 0 信息补全）

The skill needs web search to fill information gaps. Options:

| Platform | How to Add Search |
|----------|------------------|
| Claude Code / OpenClaw | Add a search MCP server (Brave Search, Tavily, SerpAPI, Exa) to `.mcp.json` |
| ChatGPT | Built-in browsing — no config needed |
| Claude API | Call your own search API before passing results to Claude |
| Self-hosted | Use any search API and inject results as user messages |

**MCP search servers that work well / 好用的 MCP 搜索服务：**

```jsonc
// Option 1: Brave Search (free tier available)
"brave-search": {
  "command": "npx",
  "args": ["-y", "@anthropic-ai/mcp-server-brave-search"],
  "env": { "BRAVE_API_KEY": "your-key" }
}

// Option 2: Tavily (optimized for AI agents)
"tavily": {
  "command": "npx",
  "args": ["-y", "tavily-mcp-server"],
  "env": { "TAVILY_API_KEY": "your-key" }
}

// Option 3: Exa (semantic search)
"exa": {
  "command": "npx",
  "args": ["-y", "exa-mcp-server"],
  "env": { "EXA_API_KEY": "your-key" }
}
```

### Document Output (for Step 1-4 deliverables) / 文档输出（用于 Step 1-4 交付物）

The skill generates a document per Step. Options:

| Platform | Output Target |
|----------|--------------|
| Mira | Feishu/Lark docs (automatic) |
| Claude Code / OpenClaw | Local Markdown files in `output/` (automatic) |
| With Notion MCP | Notion pages (add Notion MCP server) |
| With Google Docs MCP | Google Docs (add GDrive MCP server) |
| No tools | Markdown in conversation (copy-paste to your preferred editor) |

### PDF / URL Reading (for Step 0 Material Intake) / PDF/URL 阅读（用于 Step 0 材料接收）

| Platform | How to Process Materials |
|----------|------------------------|
| Mira | Built-in PDF tool + fetch tool |
| Claude Code / OpenClaw | `pdftotext` CLI + `curl` (built-in) or `fetch` MCP |
| ChatGPT | Upload files directly in conversation |
| Claude API | Extract text before sending to API |

---

## What's Lost Without Full Tool Integration / 没有完整工具集成会失去什么

| Capability | Impact | Workaround |
|-----------|--------|------------|
| Auto document generation | No formatted deliverable docs | Copy Markdown output into Notion / Google Docs / your preferred editor |
| Progressive disclosure (auto-load references) | Must manually manage context | Paste reference files when reaching the relevant Step |
| Built-in web search | Can't auto-fill information gaps | Use MCP search server, or manually search and paste results |
| PDF/URL reading tools | Can't auto-process uploaded materials | Pre-extract text from PDFs; paste article content directly |

**Bottom line / 结论**: The analytical methodology and thinking frameworks are **100% portable**. The automation and tool integration are platform-specific. On any platform, you get the same depth of analysis — the difference is just how much manual work you do versus what's automated.

分析方法论和思维框架是 **100% 可移植的**。自动化和工具集成是平台特定的。在任何平台上，你都能获得同样深度的分析——区别只是你手动操作多少 vs 自动化了多少。
