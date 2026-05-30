# mrm-mdd-template-generator

**The MRM-owned repo for reviewing and approving new MDD templates.**

This repo is the governance bridge between model development teams and Model Risk Management. It receives candidate MDD templates automatically from [`mrm-mdd-kit`](https://github.com/o-perro/mrm-mdd-kit) when a data scientist's project uses a pattern and algorithm combination that has no approved template yet. MRM reviews, refines, and approves templates here before they are promoted back into the kit for use across all ML projects.

---

## How It Works

1. A data scientist confirms their go-forward model in Claude Code
2. Claude Code generates a first-pass candidate template and opens a PR into this repo via `gh pr create`
3. A GitHub Actions workflow fires on PR open — labels the PR, requests review from the MRM team, and generates a structured review summary
4. MRM reviews only the flagged sections (AMENDED and NET NEW); inherited sections require no action
5. Two MRM reviewers approve the PR
6. On merge, a second Actions workflow opens a promotion PR into `mrm-mdd-kit`
7. Two MRM reviewers approve the promotion PR — the template is now live for all projects

---

## Repo Structure

```
mrm-mdd-template-generator/
├── CLAUDE.md          ← Instructions for Claude Code in this repo
├── incoming/          ← PRs from mrm-mdd-kit land here
├── drafts/            ← Active working space: LLM draft + human-touched revisions
├── approved/          ← Finalized templates staged for promotion to mrm-mdd-kit
└── archive/           ← Permanent record of all requests, drafts, and decisions
```

---

## For MRM Reviewers

When a PR arrives:
- Check the **MRM Review Summary** at the top of the PR description for scope and tollgate date
- Review only sections flagged `⚠️ AMENDED` or `⚠️ NET NEW` — inherited sections are pre-approved
- Edit inline directly in the PR; leave comments where decisions need to be on record
- Two approvals required before merge — no exceptions

SLA targets are auto-labeled on each PR based on tollgate date.

---

## Dependencies

- [`mrm-mdd-kit`](https://github.com/o-perro/mrm-mdd-kit) — the developer-facing kit that generates handoff PRs into this repo
- GitHub Actions — handles incoming PR processing and outbound promotion PRs
- CODEOWNERS — routes all PRs to the `@mrm-reviewers` team automatically
