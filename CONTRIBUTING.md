# Contributing

This repo holds a single Claude Skill: the Evidence Labeling Protocol. It is small and maintained by one person, so the bar for contributing is low but specific.

## Reporting a problem

Open an issue describing where the labeling discipline broke down: what claim slipped through unlabeled, or which label was misapplied, and what you expected instead. A concrete before/after example is more useful than a general complaint.

## Proposing a change

Pull requests are welcome for:

- Fixing wording or examples in SKILL.md
- Adding a good-vs-bad example you have hit in real work
- Correcting or tightening the README

Keep SKILL.md's frontmatter (`name`, `description`) intact and accurate. Claude Code and Claude.ai key off those fields to decide when to load the skill. Keep the five labels (Observed, Executed, Verified, Predicted, Blocked) as the core vocabulary; do not add a sixth label without opening an issue first to discuss why the existing five do not cover the case.

## Style

Match the existing tone: direct, no filler, no claim without the evidence that earned it.
