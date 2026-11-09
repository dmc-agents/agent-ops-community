# agent-ops-community Agent Instructions

<!-- dmc-review-policy:start -->
## Pull requests and review

This block is the DMC review standard. Every DMC repository carries it at the top of `AGENTS.md`; the shared PR proof check refuses a pull request whose target branch lacks it. The source is https://github.com/dmc-technologies/agent-ops/blob/main/configs/global/review-policy.md; change it there.

- Open pull requests under the GitHub login this machine is configured with, and never switch to another person's login. DMC-managed machines use the agent account `dmc-agents`; `.github/agent-accounts` lists the agent accounts. Open the pull request as a draft with `Proof: pending`; mark it ready when CI is green and the proof is recorded.
- The description starts with `Proof: clip`, `Proof: screenshots`, or `Proof: checks - <why nothing a person runs changed>`, then the headings What problem this solves, User impact, Why this change was made, Evidence, and Excluded. Evidence holds the exact commands with their results and exit status. Write no `Criticality:` line. Set a body with `gh api -X PATCH repos/<owner>/<repo>/pulls/<n> -F body=@file`.
- CodeRabbit applies one tier label: `routine`, `moderate`, or `critical`. Never add or remove a tier label. A person may add `critical`. A change under `.github/workflows/` or of more than 40 files is critical whatever the label says.
- Every pull request follows five steps:
  1. CodeRabbit reviews when the pull request leaves draft, and again only when you post `@coderabbitai review`. Fix every Critical and Major finding with a failing test first, reply on its thread, and post `@coderabbitai review` once per round. Never resolve a CodeRabbit thread yourself. CodeRabbit closes what it verifies and withdraws its change request; it approves a low-risk pull request itself.
  2. CI is green on the head commit.
  3. For a `critical` pull request, PR proof requests the review once, from the `dmc-developers` team, when that approval is the only thing left. The author then changes nothing. Only a point CodeRabbit still disputes after a second round goes through the command PR proof prints, listed in one pull request comment.
  4. An approval of the head from a person who did not do the work is final: not the author, not a commit author, and not an agent account. Anyone may add another reviewer. For `routine` and `moderate`, CodeRabbit's approval is the approval.
  5. When you mark the pull request ready, run `gh pr merge <n> --auto --merge` as yourself. Without write access, ask someone who has it to turn auto-merge on. GitHub merges it once `CI`, `PR proof`, and `CodeRabbit Security` pass on the head and step 4's approval exists. If GitHub reports that auto-merge is not allowed, ask a repository admin to turn on **Allow auto-merge**. A person may merge any pull request that is green and approved. Agents never merge by hand, and nobody uses `gh pr merge --admin`.
- PR proof's refusals name each next step. Full guide: https://github.com/dmc-technologies/coderabbit/blob/main/docs/coderabbit-for-dummies.md. The check: https://github.com/dmc-technologies/agent-ops/blob/main/.github/workflows/pr-proof.yml.
<!-- dmc-review-policy:end -->

## Project

`agent-ops-community` is the public Agent Ops package. It should provide the
same generic Agent Ops workflow across common agent frameworks while excluding
only proprietary runner/verifier implementations and organization-owned
operational workflows.

## Handoff

- GitHub pull requests and the tracker are the handoff. This repository has no Agent Ops harness, so there is no repository progress file and no clock-in or clock-out step.
- Record durable architecture and workflow decisions in `docs/DECISIONS.md`.
- Use shared-memory tooling only for distilled cross-agent memory.
- Keep public-facing docs free of proprietary runner names and organization-specific references.

## Verification

- `ruff check .`
- `pytest`

## Stack and migration

Do not introduce a new implementation language, runtime, or framework unless an existing supported boundary strictly requires it or Dan has approved a material product or operational benefit. Prefer the repository's current stack, preserve prototypes and history, and require source-backed migration plus rollback proof before replacement.
