# wrap-framwork-sample

This repository is a **workshop template** for practicing **WRAP (W/R/A/P)** with **GitHub Copilot coding agent**.

The goal is to start from an **almost-empty repository**, then repeatedly run the loop:

**Issue → Copilot PR → human review → merge**, in small, controlled steps.

> **Workshop operating policy**  
> - The `main` branch is intentionally kept as a **blank workshop baseline** (mainly `.github/**` + `README.md`).  
> - All hands-on work happens on dated branches like `workshop/YYYY-MM-DD[-suffix]`.  
> - Keep a reference implementation on a separate branch such as `solution` (optional).

---

## What you can do with this repo

- Learn WRAP: meaning, value, and how to apply it with Copilot coding agent
- Create a workshop branch (e.g., `workshop/2026-01-08`) from an Issue (Actions automation)
- Start with **R-first (Pattern A)** to reduce ambiguity and improve later agent results
- Keep work reviewable by enforcing **Atomic tasks** (1 Issue ≈ 1 PR)
- Pair with the agent in PRs: iterate via reviews and `@copilot`, and feed recurring feedback back into R

---

## What is WRAP?

WRAP is a practical operating pattern for getting reliable, reviewable outputs from Copilot coding agent.

### W — Write effective issues
For Copilot, an Issue is the “work order.”  
Ambiguous Issues produce ambiguous PRs. W is about removing ambiguity.

This repo’s Issue Forms emphasize:
- **Change surface** (what to touch / avoid)
- **Acceptance criteria** (verifiable)
- **Test plan** (commands + steps)
- **Edge cases / failure modes** (pitfalls up front)

### R — Refine your instructions
R defines shared repo guidance for both the agent and human reviewers:
- Standard commands (build/test/lint/run)
- Guardrails (no secrets, no unrelated refactors, minimal deps)
- Directory conventions and ownership boundaries
- PR expectations (summary, how to test, risks/assumptions)

For workshops starting from empty repos, **Pattern A (first PR is R-only)** is the most stable.

### A — Atomic tasks
A is the splitting strategy:
- 1 Issue = 1 PR
- One PR should be reviewable and independently mergeable
- Don’t mix design exploration and implementation in a single PR

### P — Pair with the coding agent
Treat agent PRs as drafts and complete them through review:
- Review the PR and give concrete, actionable feedback
- Use PR comments with `@copilot` to request fixes
- If the same feedback repeats, update **R** to prevent recurrence

---

## What this repo provides

### 1) Issue Forms (to support W and R)
Under `.github/ISSUE_TEMPLATE/`, you can include forms such as:
- **Copilot Task (Ultra-detailed)** — general implementation tasks
- **WRAP R Task (R-only)** — instructions/setup changes
- **Atomic R forms (optional)** — instructions-only / AGENTS-only / setup-steps-only
- **Workshop: Create a dated branch** — create `workshop/YYYY-MM-DD` via an Issue

> Note: Branch creation is always a **copy of the base ref**.  
> If you want a “blank workshop branch,” the base ref (typically `main`) must already be blank (this README’s policy),  
> or you must use a “create-and-blank” workflow variant that adds a blanking commit.

### 2) GitHub Actions (workshop automation)
Under `.github/workflows/`, you can keep:
- Issue → dated branch creation workflow
- (Optional) reset-to-blank workflow (creates a PR that deletes everything except `.github/**` + `README.md`)
- (Later) `copilot-setup-steps.yml` (created as part of the R-first exercise)

---

## Workshop flow (from blank baseline to a working project)

This section is the core operating procedure.

### Step 0 — Create a workshop branch (Issue → Actions)
1. Open a GitHub Issue
   - Template: **Workshop: Create a dated branch**
2. Enter a date (YYYY-MM-DD) and a base ref
   - Typically `base_ref=main` (blank baseline)
3. Actions runs and comments back with:
   - branch name
   - checkout commands
4. Checkout locally:

```bash
git fetch origin workshop/2026-01-08
git checkout workshop/2026-01-08
```

### Step 1 — First PR is R-only (Pattern A)
**Goal**: lock down guidance and guardrails before doing any app work.

1. Create an Issue (template: **WRAP R Task**)
2. Specify the deliverables, e.g.:
   - `.github/copilot-instructions.md`
   - `AGENTS.md`
   - `.github/workflows/copilot-setup-steps.yml`
3. Assign the Issue to Copilot coding agent (it opens a PR)
4. Review and merge (reject out-of-scope changes)

**R-PR review checklist**
- Only R artifacts are touched (minimal diff)
- “TBD” policy is explicit (no silent tech choices)
- Non-negotiables and PR expectations are documented

### Step 2 — Implement with atomic Issues (A)
Once R is in place, start implementation — but split it.

Suggested stack-agnostic backlog:
1. **Stack decision** (choose language/framework)
2. **Scaffold** (minimal project skeleton)
3. **Build/Test/Lint** (standard commands exist)
4. **Hello/Health** (a minimal “it runs” endpoint)
5. **CI** (lint/test/build on PR)
6. **Docs** (README and operating procedures)

### Step 3 — Pair in PRs (P)
- Consolidate feedback in PR reviews (bullet list)
- Use `@copilot` to request changes
- If feedback repeats, update R to prevent recurrence

---

## Workshop modules you can run (menu)

Pick modules based on your audience and goals.

### Module 1: WRAP fundamentals
- WRAP meaning and roles (Issue as the prompt)
- Writing effective Issues (change surface, acceptance criteria, test plan)
- PR pairing practices (`@copilot` in PR discussions)

### Module 2: R-first bootstrap (first PR is R-only)
- Create/refine `.github/copilot-instructions.md`
- Create/refine `AGENTS.md`
- Create `copilot-setup-steps.yml` so future agent sessions are more reliable

### Module 3: Atomic implementation loop
- Scaffold → build/test → health endpoint
- Test strategy (minimum unit/integration)
- Dependency policy and justification in PRs

### Module 4: Quality and operations
- Add CI workflows (lint/test/build)
- Security and secrets handling
- Convert repeating review comments into R updates

---

## Troubleshooting

### Q. The workshop branch is not blank
A. Branch creation is a **copy of the base ref**. If `base_ref=main`, then `main` must be the blank baseline.  
Alternative: use a “create-and-blank” workflow variant that adds a blanking commit.

### Q. I updated the Issue but the agent behavior didn’t change
A. In practice, treat the Issue body at assignment time as the input. Send changes via PR comments and reviews.

### Q. The workflow fails with permission errors
A. Check workflow `permissions:` and the repository/organization Actions settings.  
For safety, restrict branch creation to users with `write/maintain/admin`.

---

## Optional: reference implementation (`solution` branch)
If you keep a “finished example,” store it on a separate branch like `solution`.  
See that branch’s README for run instructions.

---

## License
MIT

