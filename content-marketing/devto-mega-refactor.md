# I Refactored 200 Files in 10 Minutes with Claude Code (Here's the Skill)

Last month I inherited a codebase. 200+ TypeScript files, inconsistent naming conventions, a mix of `any` types scattered everywhere, and error handling that ranged from "try/catch with a console.log" to "nothing at all." The original dev was gone. The deadline wasn't.

You know the feeling.

---

## The Old Way: Death by Refactor

My first instinct was to open a PR and start working through the files one by one. Three hours in, I had touched maybe 20 files, introduced two regressions, and still had 180 to go.

The problem with large refactors isn't that they're hard — it's that they're *tedious*. Every file is slightly different. Every "just find-and-replace" turns into a judgment call. You lose context. You make mistakes. You drift.

I needed Claude Code to handle the grunt work, but every time I dropped a handful of files into a prompt, it would handle maybe 10-15 files before losing track of the pattern. And starting a new conversation meant re-explaining the conventions from scratch.

There had to be a better way.

---

## Discovering Claude Code Skills

I stumbled across a concept called **SKILL.md files** — essentially a system prompt on disk that Claude Code loads automatically for a given task. Instead of re-explaining your conventions every session, you encode them once. Claude reads the skill file, understands the rules, and executes consistently across every file it touches.

The `mega-refactor` skill takes this further. It's a structured workflow skill that:

1. **Audits the codebase first** — builds an understanding of existing patterns before touching anything
2. **Defines a refactor plan** — identifies what needs to change, in what order, with what rules
3. **Executes in batches** — works through files systematically, maintaining consistency
4. **Self-verifies** — checks its own output against the defined rules before moving on

The key insight: Claude Code is incredibly powerful, but it performs better with *explicit structure* than with open-ended prompts. A skill file is that structure, made portable and reusable.

---

## What It Looked Like in Practice

After installing the skill, I ran it against the codebase with a brief brief: "eliminate `any` types, standardize error handling to use a Result pattern, rename all camelCase files to kebab-case."

Here's a simplified version of what the skill directed Claude to do:

```
Phase 1 — Audit
- Scan all .ts files, catalog: any types, error patterns, naming violations
- Output: structured list of changes needed, grouped by file

Phase 2 — Plan
- Determine safe execution order (dependencies first)
- Flag files that need human review before touching

Phase 3 — Execute
- Process files in batches of 10
- After each batch: run tsc --noEmit, check for new errors
- Rollback batch if errors introduced

Phase 4 — Verify
- Full typecheck pass
- Diff summary by change type
- List any files skipped with reasons
```

The skill doesn't just tell Claude *what* to do — it tells Claude *how to think about* the task. That's the difference between a prompt and a skill.

---

## The Result

200 files. 10 minutes of active work on my end.

Claude handled roughly 180 files autonomously. The remaining 20 were flagged for human review (complex generic types, some files with business logic entangled in the error handling). I reviewed those manually in another hour.

The output: zero new TypeScript errors, consistent Result pattern throughout, and a codebase that a new dev could actually read. What would have been a week of tedious work became a focused morning.

More importantly: the skill is *reusable*. Next codebase, same skill. I've now run it on three projects.

---

## The Bigger Pattern

This is where I think the dev tooling conversation needs to go. We spend a lot of time talking about what AI can do — but not enough about *how to encode what we know* so AI can do it reliably.

SKILL.md files are the answer to that. They're not prompts. They're not configuration files. They're workflow knowledge, made executable.

The free skills I've published cover things like generating changelogs, distributing projects across platforms, and a meta-skill for creating new skills from scratch. The premium skills go deeper — the mega-refactor skill being the most opinionated.

---

## Try It

The whole system is open to install:

```bash
curl -sSL https://raw.githubusercontent.com/jpippo364/claude-skill-up/main/install.sh | bash
```

That gives you the free skills. The premium ones (including mega-refactor) live at the [Skill Vault](https://site-dusky-kappa-44.vercel.app) — worth checking if you're doing serious work with Claude Code.

The 200-file refactor took 10 minutes. The skill took 30 seconds to install. That's a trade I'll make every time.

---

*Have you built your own Claude Code skills? Drop them in the comments — I'd like to see what patterns others have encoded.*
