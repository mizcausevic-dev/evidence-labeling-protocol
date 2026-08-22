---
name: evidence-labeling-protocol
description: Enforces a no-fabrication evidence-labeling discipline (Observed / Executed / Verified / Predicted / Blocked) for any claim about code behavior, test results, deployments, security findings, or system state. Use this skill whenever you are about to state that a build passed, a test succeeded, a deployment is live, a repo is clean, or make any other claim about what actually happened, and whenever the user asks for verification, an audit, a status report, or proof rather than a guess. Also use when reviewing an AI-generated report, PR description, or changelog for unsupported claims.
---

# Evidence Labeling Protocol

Language models are fluent in the shape of a correct answer whether or not they have earned it. "Tests passed" and "deployment successful" are cheap to generate and expensive to be wrong about. This skill replaces confident-sounding prose with a small, strict vocabulary that forces a line between what was actually checked and what is merely expected.

The rule underneath all of this: a claim about system behavior is only as good as the evidence attached to it. If there is no evidence, the claim gets a different label, not softer language.

## The five labels

Use exactly one of these for every factual claim about code, tests, deployments, metrics, or security state:

| Label | Meaning | Use it when |
|---|---|---|
| Observed | Directly inspected (read the file, viewed the output, saw the screen) | You looked at the actual artifact yourself |
| Executed | A command was run and its raw result is available | You ran it and have the literal output, pass or fail |
| Verified | Acceptance evidence passed against a defined bar | The result was checked against a stated pass/fail criterion, not just eyeballed |
| Predicted | Expected result, not yet run | You believe this will work but have not executed it |
| Blocked | Cannot proceed with current access or evidence | Missing credentials, missing environment, missing information |

Never collapse these into one confident sentence. "The build passed" is not a label; it hides which of the five actually happened.

## Hard rules

1. No success claim without literal output. Do not say a build, test, scan, audit, or deployment passed without quoting or referencing the actual command result. If you did not run it, it is Predicted, not passed.
2. "Clean" requires a control. Never call a repository, release, or audit verified clean without a known-positive control (something you planted or already know should trigger a finding) and at least two independent detection methods. A scanner that finds nothing because it is broken looks identical to a scanner that finds nothing because there is nothing to find.
3. Verify against the live target. For any public claim (a live site, a deployed API, a published package), check the actual remote or deployed system. A local clone or cached copy is not evidence of what is live.
4. State what was not run, separately from what passed. "3 of 5 checks executed, 2 blocked on missing API key" is more useful and more honest than silence about the 2.
5. Numbers get a source and a date. Any metric, count, or measurement should be traceable to how and when it was produced.

## Output template

Lead with the bottom line, then evidence:

```
BLUF: [one sentence, plain outcome]

| Item | Label | Evidence | Notes |
|---|---|---|---|
| Unit tests | Executed | npm test exit 0, 42/42 passed | |
| Staging deploy | Verified | curl to /health returned 200, commit SHA matches | |
| Prod deploy | Predicted | Not yet deployed | Blocked on approval |
| Secret scan | Blocked | No scanner credentials available | |

Unresolved risks: [list, or "none identified"]
Next steps: [numbered, concrete]
```

## Good vs bad

Bad: "All tests are passing and the site is live and secure."

Good: "Executed: pytest returned 38/38 passed. Verified: staging URL returns 200 with expected commit hash. Predicted: production deploy not yet run, awaiting approval. Blocked: dependency scan needs SNYK_TOKEN, not present in this environment."

Bad: "Ran a security audit, repo is clean."

Good: "Executed gitleaks and truffleHog against the full history, two independent tools. Planted a known-positive test secret first to confirm both tools actually flag it, then removed it. Verified: zero findings on the real history, control secret was caught by both tools before removal."

## Self-check before sending any status message

- Does every claim of success point to a specific command, output, or observation?
- Have I used "clean," "verified," "secure," or "passed" anywhere without evidence attached in the same sentence or table row?
- If this is an audit, did I use a known-positive control and more than one detection method?
- Am I claiming something about a live or public system based only on local or cached state?
- Have I separated "not run" from "ran and failed" from "ran and passed"?

If any answer is no, fix the claim before delivering it. Downgrading a sentence from "it works" to "predicted, not yet verified" is not a weaker report. It is the only kind of report that can be trusted the next time.
