# Learning Platform — v1 Spec

Oct 13, 2026 · @Jake

## Overview

Version 1 gives the agency's students one web address where they find their resources, see homework, hand it in and get feedback. Today that work is spread across email, shared files and lesson chat, so nobody has one place to see what is set, what is done and what is late.

The platform is also a deliberate project for learning to build software with Claude Code. Each feature in version 1 pairs with one Claude Code technique (see the starter kit), so scope stays small and each step ships.

## Users and roles

Three roles get logins in version 1, sized for 2–5 tutors and 20–60 students, mostly aged 13–18. Parents get no login yet.

| Role | Count | Can do | Cannot do |
| --- | --- | --- | --- |
| Student | 20–60 | See resources and homework from their own tutors; upload submissions; read feedback | See other students' work or details |
| Tutor | 2–5 | Upload resources; set homework for their own students; mark and give feedback | See students who aren't assigned to them |
| Admin (Jake) | 1 | Everything a tutor can do; create and deactivate accounts; assign students to tutors | Nothing restricted |

Accounts are created by the admin, with no public sign-up. That keeps under-18s off an open registration form and keeps the user list under the agency's control.

## Version 1 features

Version 1 has five features, built in this order. Each is finished only when its "done when" checks pass as automated tests.

| # | Feature | What it does | Done when |
| --- | --- | --- | --- |
| 1 | Accounts and roles | Email and password login; admin creates users, sets roles and links students to tutors | A student, a tutor and an admin each land on their own home page; a student can't open a tutor page |
| 2 | Resource library | Tutors upload files (PDF, images, docs) and links, tagged by subject and level, and share them with chosen students | A student sees only resources shared with them; filter by subject works |
| 3 | Homework | Tutor sets a task: title, instructions, optional attachments, due date, students assigned | A student sees an "upcoming" list sorted by due date; overdue items are flagged |
| 4 | Submissions | Student uploads one or more files (photos of written work, PDF, Word) against a task, and can resubmit before the deadline | Files are stored privately; only that student, their tutor and the admin can open them |
| 5 | Feedback and dashboards | Tutor marks a submission (comment plus optional mark); tutor dashboard shows not handed in, waiting to be marked, and marked | A tutor sees every outstanding item for their students on one screen |

Homework moves through one fixed set of stages: **set → submitted (or late) → marked**. The student is notified when feedback is ready.

## Out of scope for version 1

These are deliberately left out so version 1 ships. Each is a good candidate for version 2.

- **Parent accounts:** view-only access to a child's homework and feedback
- **Typed answers and auto-marked quizzes:** version 1 is file upload only
- **Lesson scheduling and booking:** stays in Google Calendar for now
- **Payments and invoicing:** stays in Xero
- **Messaging or chat** between students and tutors
- **Video lessons or live classroom:** lessons stay on the current video tool
- **Public sign-up or a marketing site**
- **Native mobile apps:** the web app must work well in a phone browser instead
- **AI features** such as auto-feedback or hint generation

## Data model sketch

Eight tables cover version 1. The highlighted row, one per student per assignment, carries the homework status and drives both dashboards.

&#91;embedded content: data model · 8 tables\]

Every table that holds student data has a Row Level Security policy: a student reads only their own rows, and a tutor only the rows of students linked to them. Files sit in a private storage bucket and open through short-lived signed links.

## Privacy and safeguarding

Most users are 13–18, so UK GDPR and the ICO's Age Appropriate Design Code (the Children's Code) apply. The design rule is to collect the least data possible and make every default private. This section is a design brief, not legal advice. Have it checked before real students go on the platform.

**Built into the product**

- **Minimal student data:** name, email, tutor, subjects. No date of birth, address, phone or school unless a feature needs it.
- **Private by default:** a student's work is visible only to them, their tutor and the admin. Postgres Row Level Security enforces this, and a test covers every table.
- **No tracking:** no ad or third-party analytics scripts, and no profiling.
- **UK or EU data region:** host the database and file storage in London (or the EU).
- **Retention:** delete or anonymise a student's data within a set period after they leave.
- **Secure accounts:** strong passwords, rate-limited login, password reset by email, and tutor and admin accounts protected with two-factor authentication.

**Paperwork outside the code**

- A plain-English privacy notice written for teenagers, plus a parent-facing version
- Parent or guardian agreement collected through the tutoring contract
- Data processing agreements with Supabase and Vercel
- ICO registration (data protection fee) for the agency, if not already in place
- A short process for handling a data breach

## Tech stack and constraints

The suggested stack is Next.js on Vercel with Supabase. It's well documented, cheap at this size, and every part can be run and tested by Claude Code.

| Layer | Choice | Why |
| --- | --- | --- |
| App | Next.js (App Router, TypeScript), Tailwind | One codebase for pages and server logic; strong typing helps Claude |
| Auth, database, files | Supabase (Postgres, Auth, Storage), London region | Row Level Security enforces privacy in the database itself |
| Hosting | Vercel | Deploys on every push, with a preview URL for each branch |
| Email | Supabase Auth emails; a transactional provider (e.g. Resend) for notifications | Password resets and "feedback ready" emails |
| Tests | Vitest (unit), Playwright (end to end), RLS tests against a local Supabase | Gives Claude a way to check its own work |
| Code and CI | GitHub, GitHub Actions, Claude GitHub app | Pull request checks plus @claude reviews |

**Constraints**

- **Phones first:** many students will hand in photos taken on a phone, so upload has to work well in a phone browser.
- **Cost:** free tiers are fine while building, but Vercel's free Hobby plan is for non-commercial use. Budget for paid Vercel and Supabase plans at launch, and check current pricing.
- **Upload limits:** each file has a size limit (to be set), and images are compressed in the browser before upload.

## Success criteria and open questions

Version 1 succeeds when every tutor sets and marks homework on the platform instead of by email, for a full half-term.

**Success criteria**

- All active students and tutors have accounts and use them weekly
- Every piece of homework is set, handed in and marked on the platform
- No student can reach another student's work (shown by automated tests and a manual check)
- A tutor can see everything outstanding for their students in under a minute

**Open questions**

- [ ] Which subjects and levels should the resource tags cover?
- [ ] Can a resource be shared with all of a tutor's students at once, or only one at a time?
- [ ] Should a mark be a grade, a score out of a number, or comment only?
- [ ] What size limit should apply to each uploaded file?
- [ ] How long should student data be kept after a student leaves?
- [ ] "Feedback ready" notice: email, on-screen only, or both?
- [ ] Platform name and domain
- [ ] Pilot group: which tutor and students try it first?
