---
name: design-split
description: Splits one large, exploratory UI branch (built to get the design right, not to ship) into a handful of small branches a developer can actually review — each rewritten to the project's coding standards, checked with a before/after screenshot so the visual result doesn't drift, and opened as a draft PR.
---

# Design Split

> **Before you adapt this:** every `[LIKE_THIS]` placeholder below names a skill, tool, or convention this was built against at a specific company — your own task-tracking skill, your own coding-standards skill, your own design-system reference. Swap each one for your own equivalent (or cut the step if you don't have one) before running this for real. The mechanism — snapshot branch, dependency-ordered slices, subagent rewrite against a hard "don't change the rendered UI" constraint, before/after screenshot check, draft PR — is the part meant to transfer as-is.

Turns one large, exploratory branch — designed and built without applying the project's own coding rules — into a handful of small branches a developer can actually review: clean code, real tests, real lint, while keeping the UI the user sees the same, or closer to the design system, never further from it.

Runs autonomously once the plan is confirmed. No per-slice human sign-off during execution, no ticket filed in your tracker, no PM approval loop — that's a deliberately different job from `[your-task-scoping-skill]`/`[your-task-implementation-skill]` (new work a PM scopes and prioritizes). This skill's job is finishing UI that's already been designed and approved, to a shippable standard. There are two routine human checkpoints: confirming the slice plan before anything gets pushed (see below), and reviewing/merging each PR afterward — the reviewer also runs `/[your-coding-rules-skill]` again before merging, see below. Execution can still pause outside those two for a safety or ambiguity blocker the playbook calls out explicitly — that's the named exception, not a contradiction.

## The hard constraint

Implementation can be rewritten however cleanly a slice needs. The rendered page cannot change beyond what's needed to fit the design system properly — a difference that comes from correctly aligning to the real design system is fine (confirm it, note it in the PR), but never contort the code to force pixel-parity at the cost of a hand-rolled/one-off design-system violation. Every slice with visual surface gets a before/after screenshot comparison, driving the browser tools directly, before its PR opens — a visual diff with no such explanation is a blocker, not a nice-to-have.

## Two phases

**Plan** — read the full diff/working-tree state against its real base, decide how many slices and what each contains. See [playbook.md](playbook.md#planning) for the sizing heuristic (no fixed slice count) and the stack-only-when-needed rule.

**Execute** — first, review every change that would enter the snapshot — `git diff`, `git diff --cached`, and each untracked file — and exclude anything that looks like a secret, credential, or personal data (never a blanket add-everything), then commit and push the branch's remaining current state to a dedicated snapshot branch (never opened as a PR, never merged) — this is both the safety net against losing exploratory work and the reference every slice's UI gets diffed against. Then, per slice, in dependency order: branch from the real base, isolate that slice's files, rewrite for coding-standards compliance via a subagent, run real checks, verify the UI against the snapshot, commit/push/open a draft PR. See [playbook.md](playbook.md#execution) for the full checklist.

Confirm the slice plan with the user before pushing anything — this is a multi-branch action, worth a sanity check even though each branch alone is reversible.

## Tools

Use whichever skill actually helps, one you built specifically for this project or a generic one — the test is objective vs subjective. A tool that checks for bugs, redundant code, or security vulnerabilities (`code-review`, `simplify`) is fair game regardless of origin: a bug is a bug everywhere. A tool that makes a UX/copy/design judgment call isn't safe to call generically — a generic skill can't know which of your project's choices were deliberate, which is why a UX-copy or accessibility-review skill, if you have one, stays excluded specifically — not generic skills as a whole. Your own frontend dev/test skills and design-system-reference skill (`[your-frontend-dev-skill]`, `[your-frontend-test-skill]`, `[your-design-system-review-skill]`) are all fair game too. While rewriting a frontend slice, also spawn a fresh subagent with no context on why the original choice was made and give it only the touched components plus your design-system skill's Reviewer-mode instructions — an unbiased second opinion catches drift the rewrite subagent won't flag in itself. For a backend slice, run `[your-db-migration-skill]` instead of letting a hand-edited migration survive the rewrite — security is covered by this repo's own automated checks, not this skill.

The rewrite subagent runs `[your-coding-rules-skill]` on its own diff at the end of playbook step 3, while its session still holds full context of what it just wrote and this file's hard constraint. The human still runs `/[your-coding-rules-skill]` again before merging, as an independent second pass — that step doesn't go away. Tell the human to run `/[your-task-scoping-skill]` themselves if a slice turns out to need PM-level tracking instead of a plain PR.

For the before/after UI check, drive the browser tools directly rather than through a generic app-launcher skill.

## QA branch — only when slices don't already end there

A real, pushed, never-merged branch stacking every slice for one whole-page preview. Skip building a separate one when slices are already stacked and the last slice's branch tip *is* the complete feature — that branch already serves as the QA branch. Only build a distinct one when slices are independent (not stacked) and need combining to preview together, or someone needs to preview before every slice has landed.

Never merge any slice or the QA branch. Pushing branches and opening draft PRs is the deliverable; a human merges.
