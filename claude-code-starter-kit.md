# Claude Code Starter Kit: Tutoring Learning Platform

This is a guide to learning how to work with Claude Code by building a real project. Each feature of the platform comes with one Claude technique to practise, so by version one you'll have tried the whole toolkit.

> **Suggested stack:** Next.js (TypeScript) + Supabase (auth, Postgres, storage) + Vercel. It's cheap, well documented and easy for Claude to work with. Swap in your own preferences, but keep it to tools Claude can **run and test**.

---

## 1. Day-one setup

1. **Create the repo** and scaffold the app (`npx create-next-app`). Push it to GitHub and connect it to Vercel, so you have a live URL from the start.
2. **Install Claude Code** and run `claude` in the repo.
3. **Run `/init`.** Claude reads the project and drafts a `CLAUDE.md`. Replace or extend it using the template in section 2.
4. **Give Claude ways to check its work.** Set up these commands before writing any features:
   - `npm run lint`, `npm run typecheck`, `npm test` (Vitest)
   - `npx playwright test` for end-to-end tests. Claude can drive a real browser to check the UI.
5. **Set permissions** (`/permissions`). Allow safe commands you run often (tests, lint, git status/diff) so it doesn't ask every time. Keep deploys, database migrations and `git push` on "ask".

---

## 2. CLAUDE.md template

```markdown
# Project: <Platform name>
Online learning platform for a small UK tutoring agency. Users: students
(mostly under 18), parents, tutors, admin (me).

## Stack
- Next.js (App Router, TypeScript, server components by default)
- Supabase: auth, Postgres, storage. All access goes through Row Level Security.
- Tailwind for styling. Deployed on Vercel.

## Commands
- Dev: `npm run dev`
- Check everything before saying a task is done:
  `npm run lint && npm run typecheck && npm test`
- E2E: `npx playwright test`
- New migration: `supabase migration new <name>` (never edit applied migrations)

## Conventions
- Data access lives in `lib/data/`, never fetch from components directly.
- Every new table gets RLS policies + a test proving a student can't read
  another student's data.
- Follow existing patterns: look at `app/resources/` before building a new page.
- Small, reviewable commits. One feature per branch.

## Hard rules
- Never weaken or disable RLS, auth checks, or tests to make something pass.
- Collect the minimum student data. No new personal-data fields without asking me.
- Don't add dependencies without saying why.

## Working with me
- I'm an experienced developer. Be concise and skip basics.
- For anything non-trivial: propose a plan first, then implement.
- Tell me about trade-offs and anything you were unsure about when you finish.
```

Keep it short and current. When Claude gets something wrong twice, add a line.

---

## 3. Roadmap: one feature, one technique

| # | Feature | Claude technique to practise | What you're learning |
|---|---------|------------------------------|----------------------|
| 0 | Scaffold, CI, CLAUDE.md | `/init`, permissions, verification commands | How context and checks shape output quality |
| 1 | Auth and roles (student / parent / tutor / admin) | **Plan mode** (Shift+Tab). Review the plan before any code is written. | Steering design instead of reviewing code afterwards |
| 2 | Resource library (upload, tag, browse by subject/level) | **Point at examples** and let it **self-verify** with Playwright | Getting it to match your patterns and prove its own work |
| 3 | Homework: tutors set assignments, students see what's due | **Subagents** (`/agents`): one researches, another reviews the diff | Keeping the main context clean and getting a second opinion |
| 4 | Submissions (file and text) plus storage and RLS | **Hooks** (`/hooks`): auto-run lint and tests after edits, block edits to applied migrations | Enforcing rules automatically instead of repeating them |
| 5 | Feedback and marking, plus parent view | **Parallel sessions with git worktrees**: feedback UI in one, notifications in another | Working on several things at once |
| 6 | Tutor dashboard (who's behind, what's due) | **Custom slash command or skill**, e.g. `/new-page` that follows your conventions | Turning repeated workflows into one-liners |
| 7 | Review and CI | **GitHub app** (`/install-github-app`): `@claude` on PRs and issues; `claude -p` in CI | Delegating work that runs without you |
| 8 | Database tooling | **MCP server** (`/mcp`) for Supabase, to inspect schema and query dev data | Connecting Claude to your real tools |

**Throughout:** use `/rewind` (or Esc twice) to undo an approach you don't like, `/clear` between unrelated tasks, and `/compact` when a long session gets sluggish.

---

## 4. Prompt patterns that work

- **Brief like you would a senior contractor:** the goal, the constraints, where to look, and how to know it's done.
  > "Add homework submission. Students upload a file or text against an assignment. Follow the pattern in `app/resources/`. Done means: RLS stops students seeing each other's work, there are Playwright tests for submit and view, and lint, typecheck and tests all pass."
- **Ask for a plan first on anything over ~50 lines.** Push back on the plan, not on the code.
- **Ask it to explore before editing:** "Read the auth code and explain how roles work before changing anything."
- **Ask for a self-review:** "Review your diff as a sceptical senior engineer. What would you flag?"
- **Ask what it was unsure about.** It will tell you, and that's where to focus your review.
- **When it goes wrong, rewind and give a better brief** rather than piling on corrections.

---

## 5. Where you stay in charge

- **Architecture and data model.** Agree these in plan mode, and write key decisions into CLAUDE.md.
- **Security and student data.** Read every RLS policy and auth change yourself. UK GDPR and the ICO Children's Code apply: minimal data, parental consent, secure logins. Get a proper check before real students use it.
- **Merging.** Read the diff. Claude writes fast; your job is steering and review.
- **Dependencies and cost.** Approve new packages and paid services.

---

## 6. Using Cowork alongside Claude Code

- **Spec and planning docs:** write and refine them here, then drop them in the repo for Claude Code to build from.
- **Content:** your Education Skills Library (`/practice-problem-sequence-designer`, `/retrieval-practice-generator`, `/adaptive-hint-sequence-designer`, etc.) can produce the resources and homework that fill the platform.
- **Business side:** Small Business skills plus Gmail/Xero handle invoicing and parent emails while you build.

---

## 7. Quick reference

| Command / key | What it does |
|---------------|--------------|
| `/init` | Draft CLAUDE.md from the codebase |
| Shift+Tab | Cycle modes (normal → auto-accept edits → plan) |
| `/permissions` | Choose which commands run without asking |
| `/agents` | Create and manage subagents |
| `/hooks` | Configure hooks |
| `/mcp` | Manage MCP servers |
| `/rewind` / Esc Esc | Roll back to an earlier checkpoint |
| `/clear`, `/compact` | Reset or summarise context |
| `/config` | Settings, including the output style |
| `/install-github-app` | Set up `@claude` on GitHub |
| `claude -p "<task>"` | Headless mode for scripts and CI |

Commands change over time. Type `/` in Claude Code to see the current list.
