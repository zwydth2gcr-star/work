# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A set of Russian-language strategic-planning notes, not code. No build system, package
manager, or test suite — the three files are static markdown, edited directly. The subject
is matching YC's public "Requests for Startups" (RFS) list against one person's real
constraints: deputy chief accountant in Belarus, ~5 hours/week available, no audience or
professional network, Belarus/RU market, working English.

## Document pipeline

The three files form a funnel, each narrowing the previous one — read them in this order:

1. **matches.md** — starts from the YC RFS batches (Fall 2026, Summer 2026, Spring 2026,
   sourced from https://www.ycombinator.com/rfs) and filters down to 5 directions that
   connect to the person's actual edge (accounting-domain expertise + agentic tools, not
   just "learning Claude Code," which the file notes hundreds of thousands of people are
   also doing). Each match is scored 1–5 on six axes: смысл (fit), ресурсы (resources),
   скорость запуска (speed to launch), спрос (demand), контент (content potential), деньги
   (money). Ends with sections on rejected directions and open questions the author
   deliberately did not guess at.
2. **opportunities.md** — takes the 5 matches from matches.md, ranks them in a summary
   table (Σ of the six scores), and turns the top 3 into concrete 1–3 day experiments and a
   two-week plan.
3. **product-ideas.md** — expands the matches into 8 concrete product/service/demo ideas,
   each following the same fixed shape: суть (what) → для кого (for whom) → что сделать
   (action) → время (time) → как проверить спрос (how to validate demand) → риск (risk).
   Closes with a single recommended starting point.

## Conventions used across these files

- **Fact vs. interpretation is marked explicitly.** matches.md defines this up front:
  **Факт** = stated in the profile, present in the working folder, or said directly in
  conversation; **Интерпретация** = the author's inference, which may be wrong. Preserve
  this distinction when editing — don't convert an Интерпретация into a Факт or drop the
  label.
- **Scoring axes are fixed** across matches.md and opportunities.md: смысл, ресурсы,
  скорость запуска, спрос, контент, деньги (all 1–5). Keep any new entries consistent with
  this rubric rather than inventing new axes.
- Risk sections are load-bearing, not boilerplate — e.g. don't fabricate deepfakes "for
  demonstration" (product-ideas.md §4), don't use real employer data in examples
  (product-ideas.md §3, §6), always date-stamp compliance checklists since rules change
  (product-ideas.md §7).
- All content is in Russian and addresses the reader informally (ты). Keep new content in
  that voice.

## Cross-references outside this directory

These files reference sibling folders that live outside `yc-rfs/` in the parent `Work/`
repo, not inside it:

- `Persona/profile.md` — the source profile these matches are built from (background,
  constraints, what the person wants to build). `Persona/` is its own nested git repo and
  is gitignored from the parent — don't assume it's tracked here.
- `Game/` and the `Persona/` site (`Persona/index.html`) — referenced in product-ideas.md
  §5 as existing, ready-to-write-about work ("уже готовы").
- `skills/` — referenced in opportunities.md's experiment 3 as a future location for
  `SKILL.md` procedure files; it does not exist yet in this repo.
- `claude-config/CLAUDE.md` — referenced in matches.md as the source of truth for which MCP
  tools are installed (e.g. playwright, used for pulling source-of-law text from websites).
