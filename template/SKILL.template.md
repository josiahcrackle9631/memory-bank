---
name: my-skill-name
description: >
  Write this as a TRIGGER, not a summary. Tell Claude WHEN to activate this skill.
  Include specific phrases the user might say, contexts where it applies, and
  types of output it produces. The more specific, the better Claude gets at
  knowing when to use it automatically.
  Example: "Use this skill when the user says 'optimize this query', 'why is
  this slow', or asks about database performance. Also trigger when reviewing
  code that contains SQL queries or ORM calls with potential N+1 issues."
tags: [tag1, tag2, tag3]
version: 1.0.0
author: your-name
---

# Skill Name

One sentence: what does this skill enable Claude to do that it can't do well
without it?

---

## When to Use

Trigger this skill when:

- User says: "[specific phrase 1]", "[specific phrase 2]"
- User is working on: [specific context]
- User asks for: [specific type of output]
- Claude detects: [specific pattern in code or conversation]

Do NOT trigger when:
- [Situation where this skill would be wrong or overkill]

---

## Workflow

### Step 1: Gather Context

[What Claude should read, check, or ask before doing anything.]

```bash
# Commands to run for context gathering, if any
```

### Step 2: Execute

[The core action. Be specific about what Claude should produce, but don't
over-prescribe HOW. Give goals and constraints, not rigid steps.]

### Step 3: Verify

[How Claude confirms the output is correct. What to check.]

### Step 4: Present

[How to show the result to the user. Format, tone, level of detail.]

---

## Output Format

[Define the structure of what Claude produces. Use a template:]

```markdown
## [Output Title]

### Section 1
[what goes here]

### Section 2
[what goes here]
```

---

## Examples

**Example 1: [Scenario name]**

User says: "[realistic user input]"

Claude produces:
```
[realistic output]
```

**Example 2: [Scenario name]**

User says: "[realistic user input]"

Claude produces:
```
[realistic output]
```

---

## Guidelines

- [Most important rule — the thing that makes or breaks quality]
- [Common mistake to avoid]
- [Edge case to handle gracefully]
- [Quality bar — when is the output "good enough"?]

---

## Gotchas

[Things that will go wrong. Add to this section over time as you discover
failure modes. This is the highest-signal section in any skill.]

- [Gotcha 1: what goes wrong and how to handle it]
- [Gotcha 2: what goes wrong and how to handle it]

---

## Reference Files

- `references/[file].md` — [What it contains and when Claude should read it]
