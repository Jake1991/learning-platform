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
