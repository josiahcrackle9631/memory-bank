# Contributing to memory-bank

Thanks for your interest in making memory-bank better. This project follows
the [agentskills.io](https://agentskills.io) open standard, so your
contributions can benefit developers across Claude Code, Cursor, Windsurf,
GitHub Copilot, and 20+ other AI agent platforms.

## Ways to Contribute

### Report Issues
- Found a bug? Memory not loading correctly? Open an issue.
- Include: what you expected, what happened, your CLAUDE.md setup.

### Improve the Core Skill
- Better memory templates
- Smarter compression logic
- New trigger phrases
- Edge case handling

### Add Advanced Patterns
- New patterns in `skills/memory-bank/references/advanced-patterns.md`
- Must solve a real problem developers face
- Include: trigger, process, and output format

### Add Example MEMORY.md Files
- New examples in `skills/memory-bank/examples/`
- Must feel realistic — specific file paths, real decisions, concrete next steps
- Cover a project type not already represented

### Improve Documentation
- Clearer explanations
- Better examples
- Fix typos and broken links

---

## How to Contribute

### 1. Fork and Clone

```bash
git clone https://github.com/YOUR_USERNAME/skills-repo.git
cd skills-repo
```

### 2. Create a Branch

```bash
git checkout -b improve/compression-algorithm
```

Use prefixes: `improve/`, `fix/`, `add/`, `docs/`.

### 3. Make Your Changes

Follow these guidelines:

**For SKILL.md changes:**
- Keep the YAML frontmatter valid
- Test with Claude Code before submitting
- Maintain backward compatibility with existing MEMORY.md files

**For reference docs:**
- Be practical, not theoretical
- Every paragraph should be actionable
- Include code blocks and examples
- No filler — if a sentence doesn't add value, remove it

**For examples:**
- Must look like a real MEMORY.md from a real project
- Use specific file paths, function names, and decisions
- Include memory health score and session count
- Cover the branch and timestamp fields

### 4. Test with Claude Code

The most important step. Actually use your changes:

```bash
# Install the skill from your local fork
npx skills add ./skills/memory-bank

# Start a Claude Code session and verify:
# - Does memory load correctly?
# - Do trigger phrases work?
# - Is the output format correct?
# - Does compression work as expected?
```

### 5. Open a Pull Request

- Keep PRs focused — one feature or fix per PR
- Write a clear description of what changed and why
- Include before/after examples if the change affects output
- Reference any related issues

---

## Skill Structure

```
skills/
  memory-bank/
    SKILL.md                      ← Core skill logic (the engine)
    references/                   ← Deep-dive docs Claude loads on demand
      memory-layers.md
      branch-aware-memory.md
      smart-compression.md
      session-diffing.md
      advanced-patterns.md
      claude-md-integration.md
    examples/                     ← Realistic MEMORY.md examples
      solo-fullstack.md
      team-backend.md
      monorepo.md
      minimal.md
```

**SKILL.md** is the core — it's what Claude reads when the skill activates.
Keep it comprehensive but scannable.

**references/** are loaded on demand when Claude needs deeper context.
These can be longer and more detailed.

**examples/** are realistic samples that show what great memory looks like.

---

## Style Guide

- **Markdown only.** No HTML except in README.md badges.
- **No fluff.** Every line should be useful. If you can remove a sentence
  without losing meaning, remove it.
- **Be specific.** "Working on auth" is bad. "Implementing JWT refresh in
  `src/auth/refresh.ts:47`" is good.
- **Use tables** for structured data (decisions, file references, comparisons).
- **Use code blocks** for file paths, commands, and output formats.
- **No emojis** in skill files or references. README badges are fine.

---

## Creating New Skills

Want to contribute an entirely new skill to this repo?

1. Copy `template/SKILL.md` into a new folder under `skills/`
2. Fill in the YAML frontmatter — especially the `description` field
   (this is what tells Claude when to activate the skill)
3. Write clear workflow steps
4. Add `references/` docs for anything that needs depth
5. Test with Claude Code
6. Open a PR explaining what the skill does and why it's useful

---

## Code of Conduct

Be kind. Be constructive. We're all here to make AI coding better.

- Review others' PRs thoughtfully
- Explain your reasoning, not just your opinion
- Welcome newcomers — not everyone has contributed to open source before

---

## Questions?

Open an issue or start a discussion. We're happy to help you contribute.
