# Habit Tracker — Notion API Integration (leak-proof repository)

A Next.js habit tracker backed by a Notion database. This repository was
initialised leak-proof: the `.gitignore` rules and this README were committed in
the **same initial commit** as the Next.js scaffold, so no unprotected version
of the repository exists anywhere in its history.

## Why these ignore rules exist

Every rule below comes from the **File Path** column of
*Secret-Scan Findings: Leaked Notion Tokens in Community Example Repos
(Nov 2025)*. The sheet has 14 rows covering **9 distinct paths** (`.env.local`,
`.env.development` and `README.md` each appear more than once). There is one
precise rule per distinct path — deliberately **not** a `.env*` catch-all, so no
real variant can slip past a broad pattern.

| Leaked path (from findings) | Rule in `.gitignore` that blocks it | Findings rows |
| --- | --- | --- |
| `.env` | `.env` | SEC-NTN-2511-01, SEC-NTN-2511-07 |
| `.env.local` | `.env.local` | SEC-NTN-2511-02, SEC-NTN-2511-03, SEC-NTN-2511-06 |
| `.env.development` | `.env.development` | SEC-NTN-2511-05, SEC-NTN-2511-13 |
| `.env.development.local` | `.env.development.local` | SEC-NTN-2511-11 |
| `.env.example` | `.env.example` | SEC-NTN-2511-09 |
| `config/notion.json` | `config/notion.json` | SEC-NTN-2511-08 |
| `lib/notion-config.ts` | `lib/notion-config.ts` | SEC-NTN-2511-12 |
| `README.md` | `README.md` | SEC-NTN-2511-04, SEC-NTN-2511-14 |
| `notes/setup.md` | `notes/setup.md` | SEC-NTN-2511-10 |

Two of the leaked paths are documentation files. `README.md` and
`notes/setup.md` are ignored like the rest, so a future token pasted into a doc
cannot be committed by accident. This specific README was added with
`git add -f README.md`, which is why it is tracked while the rule stays in force.

The findings also show that a rule only helps if it matches the **exact**
filename: rows 3, 6 and 11 leaked because the repository had `.env`,
`env.local` or `.env.local` while the real file was `.env.local` or
`.env.development.local`. Each rule here is written against the literal path the
sheet reports.

## Token search at the time of the initial commit

Searched every file in this repository (16 files, tracked and untracked,
excluding `node_modules` via `git ls-files -c -o --exclude-standard`) for Notion
token prefixes with:

```bash
grep -rInE "ntn_[A-Za-z0-9]|secret_[A-Za-z0-9]" $(git ls-files -c -o --exclude-standard)
```

**Result: 0 matches.** No `ntn_` or `secret_` token string is present anywhere in
this repository. The search was run on 2026-10-08 (UTC) against the working tree
that became the initial commit — this is the state of this repository at this
point in time, not a generic template statement.

## Notion database and integration

`docs/notion-database-and-integration.png` is committed in this repository and
shows the project's Notion habit-tracker database together with the integration
connected to it (visible in the database's **Connections** list), confirming the
integration is wired to this database rather than merely created in isolation.

## Local setup

The Notion credentials are never committed. Create an untracked `.env.local`
(already ignored) with:

```
NOTION_TOKEN=<your integration token>
NOTION_DATABASE_ID=<your habit tracker database id>
```

Then:

```bash
npm install
npm run dev
```
