# Reddit Post: r/ClaudeAI or r/ChatGPTCoding

**Title:** Built a system where Claude Code can install new capabilities like apps — here's how

---

Hey, wanted to share something I've been building that might be useful for others who use Claude Code heavily.

**The problem I kept running into:** Every new Claude Code session, I'd spend the first few minutes re-explaining my conventions. "Use TypeScript strict mode, prefer Result types over throwing, kebab-case filenames, etc." It works, but it's friction — and Claude's consistency across a long session degrades compared to when it has explicit structure to follow.

**What I built:** A system called SKILL.md files.

The idea is simple. You create a markdown file that describes a *workflow* — not just preferences, but a structured set of phases, rules, and verification steps for a specific task. You drop it in a `~/.claude/skills/` directory. When you need Claude to do that task, you reference the skill, and Claude follows the structure consistently.

Here's a rough example of what a skill file looks like for a refactoring task:

```markdown
# mega-refactor

## Purpose
Systematically refactor a codebase to enforce consistent conventions.

## Phase 1: Audit
- Scan all source files
- Catalog violations by type (naming, types, error handling)
- Do not modify anything yet

## Phase 2: Plan
- Group changes by risk level
- Identify execution order (dependencies first)
- Flag files needing human review

## Phase 3: Execute
- Process in batches of 10
- After each batch: run tsc --noEmit
- Rollback if new errors introduced

## Verification
- Full typecheck pass
- Output diff summary by change type
```

When Claude reads this before starting a refactor, it follows the phases. It doesn't jump ahead. It checks its own work. It's the difference between "here's a thing I want" and "here's the process for doing that thing correctly."

**The practical result:** I refactored a 200-file TypeScript codebase in about 10 minutes of actual work. Claude handled ~180 files autonomously, flagged 20 for human review, and introduced zero new type errors. Normally that's a week of tedious work.

---

**Free install if you want to try it:**

```bash
curl -sSL https://raw.githubusercontent.com/jpippo364/claude-skill-up/main/install.sh | bash
```

This gives you three free skills:
- `prompt-to-skill` — describe a workflow, Claude writes the skill file for you
- `changelog-release-notes` — reads git log, writes human-readable release notes
- `claude-skill-up` — meta-skill for managing your skill library

---

There are also some premium skills I've built for more specific use cases (codebase refactoring, SEO audits, project distribution) but the free ones should give you a solid sense of the pattern.

The broader idea I'm exploring: most of the value of AI coding assistants isn't in the model — it's in how well you can encode your domain knowledge into something the model can follow consistently. Skills are one approach to that. Curious if others have built similar systems or have different approaches to the "how do I make Claude consistent across sessions" problem.

What workflows would you want a skill for?
