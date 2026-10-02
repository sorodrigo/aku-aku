---
name: paranoid-bunch
description: Review code, a change, a plan, or a spec for complexity caused by being too careful, keeping backwards compatibility, or being too thorough. Writes one list of issues to docs/review.md, then asks the user to decide on each one. Use when the user asks for the paranoid bunch, a paranoid review, or a complexity review.
---

# paranoid bunch

## 0. Target

If the user has not said what to review, ask first. The target can be the project code, a diff, a branch, a plan, a spec, or any file. In the steps below, "the project code" means the target.

## 1. Review

Can you review the project code from a softwate architecture standpoint and write your findings to `docs/review.md`? I want you to analyze it and look for paranoid, backwards compatibility, obsesive thoroughness complexity. I want to be able to understand how data flows thru the system in no more than 4 paragraphs. Complexity makes things fragile, simplicity is antifragile.

Hint: Should be run twice in adversarial mode with different models.

## 2. Blend

Review the findings in that file. Group recommendations about the same issue into a single entry. I want a single blended list of issues. If views differ or conflict, group them and list both.

## 3. Decide

Ask me for a decision on each issue, one at a time. I will give you my thoughts, and you will record them. Record each decision as accepted, with or without comments; deferred; or rejected.

Assume I have not read the target. Tell me two stories, so I can decide without asking for more:

- **The target's story**, once, before the first issue: in 4 lines at most, what it does and how it does it.
- **Each issue's story**, before its question: the part of the target it touches, what goes wrong there, and what the recommendations would change. Keep it short, in plain words, and in the order things happen.

When showing an issue, give its number and the total, for example "Issue 2 of 12".

A decision can also be an **open question**, left for the team to read and discuss. Ask me for my recommendation and record it with the question. Never write the recommendation yourself. An open question is not complete without one.

## Addenda

Put a long explanation, or a decision that others may need to review, in an addendum at the end of `docs/review.md`. Name addenda A, B, C, and so on, and refer to them by letter from the issue.

## Drill-down (optional)

If the target is a feature plan, ask me after step 3 whether to drill down. Only drill down if I say yes.

Turn the plan and the decisions in `docs/review.md` into a spec:

- `docs/spec/index.md`: lists the PRs in order, with a link to each.
- `docs/spec/pr-<n>-<name>.md`: one file per PR, where `<name>` is a short name in lowercase words joined by hyphens, for example `pr-1-add-user-table.md`.

Each PR must be atomic: it can be reviewed and merged alone, and the system still works after it. Each PR file has these sections, in this order:

1. **Before**: how the system behaves before the PR.
2. **After**: how the system behaves after the PR.
3. **Implementation details**
4. **Acceptance criteria**

The spec may refer to the plan, or to an addendum by letter, instead of repeating it.

Then review the spec: run steps 1 to 3 with the spec as the target, and write to `docs/spec-review.md` instead of `docs/review.md`. Record decisions only, with no addenda. Do not offer another drill-down.

When the spec review ends, update the spec, the plan, and the addenda to match the latest decisions, so no file keeps information that is out of date.

## Writing

Check every file this skill writes against the writing rules in `AGENTS.md` or `CLAUDE.md`.
