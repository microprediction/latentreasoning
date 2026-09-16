# CLAUDE.md — latentreasoning

This repository builds a **public website** (latentreasoning.net). Everything
committed here is world-readable once pushed. Treat every file as published.

---

## HARD RULE 1 — Never look outside this repository

This project has **no permission to read, list, open, search, or reference anything outside
`/Users/petercotton/github/latentreasoning`.**

- No `ls`, `cat`, `grep`, `find`, `Read`, or `Glob` targeting a parent, sibling, or home directory.
- No reading other repositories, projects, notes, or scratch directories on this machine.
- This includes *listing directory names*, not just reading file contents. Names are information.
- Repo-scoped commands only. Prefer `git ls-files` over `find .`; never pass a path starting with
  `/` or `../`.
- The session scratchpad may be used for temporary working files. It is not a source of content.

There is no research, verification, or convenience reason that overrides this. If something seems
to require looking outside the repo, **stop and ask** instead.

**Standing exception, granted by the repo owner:** the `microprediction/style` repository on
GitHub may be read, via `gh`, for prose guidance, and its house style is to be followed. That
grant covers reading that one remote repository and nothing else. It does not weaken Hard Rule 1
for the local filesystem, and it does not license putting anything from a private repository into
this public one (see Hard Rule 2).

## HARD RULE 2 — Public sources only

Every fact, number, quotation, citation, name, and URL on this site must trace to a **public source
fetched and verified during the session that added it.**

- Acceptable: arXiv, published papers, public GitHub/Hugging Face pages, public lab write-ups,
  named news reporting.
- Not acceptable: anything from another local project; internal, unpublished, or proprietary
  material; anything a person said privately; anything remembered from a prior session but not
  re-verified.
- If a claim cannot be supported by a public URL that resolves, it does not go on the site.

## HARD RULE 3 — No local paths, hosts, or identifiers in output

Never commit absolute filesystem paths, machine names, internal hostnames or IPs, private URLs,
email addresses, credentials, or the names of other projects on this machine — not in page content,
comments, commit messages, or this file.

---

## Verification before publishing

Run from the repo root. All three must come back clean.

```sh
# 1. every external host referenced must be a known public one
git ls-files -z | xargs -0 grep -rhoE 'https?://[^"'"'"' )]+' \
  | sed -E 's#https?://([^/]+).*#\1#' | sort -u

# 2. no local paths, internal hosts, or credentials
git ls-files -z | xargs -0 grep -rniE \
  '(localhost|127\.0\.0\.1|192\.168\.|\.internal|\.corp|intranet|api[_-]?key|secret|password|/Users/|/home/)'

# 3. every external link resolves
git ls-files -z | xargs -0 grep -rhoE 'https?://[^"'"'"' )]+' | sort -u \
  | xargs -P8 -I{} sh -c 'printf "%s %s\n" "$(curl -s -o /dev/null -w "%{http_code}" -m 20 -A Mozilla/5.0 "{}")" "{}"'
```

Note: `.gitignore` is the stock Python template and contains benign strings such as
`secrets.toml` and package-manager URLs. Those are expected; anything else is not.

---

## House style

Prose follows `microprediction/style`. The rules that bite most often here:

- **Banned outright, every register:** "load-bearing" (and variants such as "load-carrying"),
  "earns its keep". Name the specific thing depended on instead.
- **No phantom context.** Never write a sentence that only makes sense to someone who saw a
  previous draft — "this page previously said", "the site now does X", "an earlier version".
  State the timeless fact. A dated changelog entry is fine; narrating revisions in prose is not.
- **No significance announcements** — "the key point is", "what matters here is", "the
  interesting thing is", "worth noting". Put the two facts side by side and let the contrast work.
- **No reading instructions** — "this is where I explain X". Trust the title.
- **No self-grading prose** — "to be precise", "strictly speaking" as self-audit.
- **Factual, not conversational.** Avoid second-person address and tour-guide scaffolding; it
  reads as lecturing.
- Avoid the assistant lexicon: delve, leverage (verb), showcase, underscore, realm, tapestry,
  pivotal, robust/powerful/elegant as praise, "Think of it as", "boils down to", "Enter X".
- Semicolons are rare in the corpus (3.3 per 10k). Prefer a full stop.

Verify before pushing: extract page text and run the corpus audit from the style repo
(`analysis/ai_pattern_audit.py`) against its baseline (`analysis/audit_results.txt`). Anything in
its AVOID table appearing in a draft is a bug.

## Editorial standards

- Cite the result that cuts against a method next to the one that favours it.
- Treat rebuttals as first-class; several headline mechanisms in this field failed replication.
- State complexity-theory claims with their preconditions, or not at all.
- Mark anything resting on news reporting rather than a primary document.
- Dashed edges on the literature map are claims about absences — each carries a confidence level.

## Build

Plain static HTML, one shared `style.css`, no build step. KaTeX and d3 from CDN. GitHub Pages
serves from the default branch, root directory; `CNAME` sets the domain.
