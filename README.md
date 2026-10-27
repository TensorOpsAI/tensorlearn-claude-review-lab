# Review without the author’s bias

A PR description says all requirements and performance were verified. The patch removes caching, logs whole responses and changes approved behavior. Generated-file style changes distract from the defects.

## Task

1. Read the approved intent, spec and plan.
2. Write `REVIEW.md` and the reviewer’s instructions.
3. Run a blind review using only requirements and the patch.
4. Repeat with the author narrative and compare supported findings.
5. Test the clean patch; reduce unsupported findings and minor comments.

## Working rules

Edit only REVIEW.md and agents/reviewer.json. Assessed reviewer processes have no shell or write tools. The instructor MCP exposes only immutable requirements, diff and file reads. Candidate reports are not accepted as evidence that a review actually ran.

## Acceptance targets

- Findings cite changed paths/lines, evidence and violated requirements.
- Material privacy, release-gate and caching defects are found.
- The clean patch does not receive invented defects.
- The reviewer remains read-only and does not approve or merge.

## Calibration findings (graded)

Use your policy to review `fixtures/defective.patch` and `fixtures/clean.patch`, then
record the findings in `REVIEW.md` as exactly one JSON block. Each patch gets an
object that follows `findings.schema.json`; use the requirement IDs from
`fixtures/approved.md` and the line number in the changed file.

```json
{
  "fixtures/defective.patch": {"findings": []},
  "fixtures/clean.patch": {"findings": []}
}
```

## How to submit

1. Fork this repository to your personal GitHub account. Use fictional information
   only, keep the fork public, and keep `main` as its default branch.
2. Clone your fork and enable GitHub Actions in its **Actions** tab. Use **Connect
   GitHub for scoring** on the lab page to install the read-only grading App on your
   fork. Return to the lab and use **Recheck connection** if needed.
3. Download the TensorLearn client from the lab page and extract it outside your fork.
   Follow the page's connection command from your fork folder. Paste your learner
   token only into the hidden terminal prompt. Run `/load` from your fork folder to
   connect the repository. Run `/load` again after the facilitator launches the round.
4. Create a branch such as `review/first-draft` and complete the task above. Change
   only the files the task allows; any other change makes the submission invalid.
5. Commit and push your branch. Open a PR **inside your own fork**, targeting its
   `main` branch. GitHub may suggest the upstream repository: select your fork instead.
   Opening or updating this PR starts the checks automatically.
6. Keep the PR open. When ready, run **`/submit`** in the connected Claude Code session
   to submit that exact pushed revision for the current round. **No merge is required.**
7. Use `/check` or refresh the lab page to read the saved result. The Actions run and
   `lab-score/score.json` are useful feedback, but an Actions check alone does not
   submit your work or prove that the server saved a score.

A change to your answer requires a new commit, push, and explicit `/submit`. Keep
`main` unchanged: the grader compares your PR with the released starter.

## What is graded

The pinned grader in `.github/workflows/grade.yml` reads your files as data and runs
your code only in an isolated, offline container. Every check is a fixed rule; no AI
model judges your work. Your facilitator separately reviews clarity and reasoning.

- REVIEW.md is a written policy covering evidence, severity and requirement mapping.
- agents/reviewer.json keeps its three fields and writes findings.schema.json output.
- Your recorded findings match the schema, find the three real defects with the right requirement and severity, and report nothing unsupported.
- The style change stays minor and the clean patch has no findings.
