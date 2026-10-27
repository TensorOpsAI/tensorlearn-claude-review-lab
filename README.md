# Review without the author’s bias

A PR description says all requirements and performance were verified. The patch removes caching, logs whole responses and changes approved behavior. Generated-file style changes distract from the defects.

The PR under review is `fixtures/defective.patch`; its description is `fixtures/author-narrative.txt`. `fixtures/clean.patch` is a second, small change to check that your review does not invent problems. The approved requirements, with their IDs, are in `fixtures/approved.md`; there is no separate spec or plan file. The evals and incident labs tell the same incident from other angles.

Terms used below:

- **Blind review:** a review that sees only the approved requirements and the patch, not the author's description.
- **Supported finding:** one that points to a changed line and ties it to a requirement in `fixtures/approved.md`.
- **Reviewer instructions:** the `instructions` in `agents/reviewer.json`: how a read-only reviewer reads the requirements and the patch and reports findings in the `findings.schema.json` shape.

**What you'll practise:** reviewing a change with Claude Code without being steered by the author. Write a review policy, run it in a fresh context that sees only the requirements and the diff, then compare that with a run that also reads the author's claims.

**In the course slides:** Chapter 2, Lesson 3 (AI in the PR review loop): REVIEW.md, findings that cite the requirements, and a cap on nits. The slides rank findings as Important or nit; here `critical` and `major` are the slides' Important, and `minor` is a nit.

## Task

1. Read the approved intent, spec and plan in `fixtures/approved.md`.
2. Write `REVIEW.md` and the reviewer’s instructions. In `REVIEW.md`, replace the TODO line with your policy: what counts as evidence, how you choose a severity, how a finding maps to a requirement, how you handle uncertainty, and a cap on minor comments. In `agents/reviewer.json`, fill in `instructions` and keep the other two fields.
3. Run a blind review using only requirements and the patch: in a fresh Claude Code session, or a subagent, give the reviewer your instructions and let it read only `fixtures/approved.md`, `fixtures/defective.patch` and `REVIEW.md`. Ask for findings in the `findings.schema.json` shape.
4. Repeat with the author narrative (`fixtures/author-narrative.txt`) added and compare supported findings. In `REVIEW.md`, note what changed between the two runs and how your policy keeps the narrative from replacing evidence.
5. Test the clean patch (`fixtures/clean.patch`) the same way; tighten the policy until it reduces unsupported findings and minor comments. Record the final findings for both patches in `REVIEW.md`, in the graded block below.

A good way to work: draft the policy with Claude, then run each review in a context that has not seen this conversation, so neither the author's claims nor your own discussion steer it. Compare the two runs before you change the policy. Keep the reviewer's output in the chat (do not save it to new files); you copy the final findings into `REVIEW.md` yourself.

## Working rules

Edit only REVIEW.md and agents/reviewer.json.

The reviewer only reads: it does not edit files, run commands, approve or merge. The grader does not run your reviewer. It reads `REVIEW.md` and `agents/reviewer.json` and checks the findings you record with fixed rules.

## Acceptance targets

- Findings cite changed paths/lines, evidence and violated requirements.
- Material privacy, release-gate and caching defects are found.
- The clean patch does not receive invented defects.
- The reviewer remains read-only and does not approve or merge.

## Done when

- `REVIEW.md` has your policy in prose (no TODO), a note on how the author narrative changed the review, and one fenced `json` block with findings for both patches.
- Each finding for `fixtures/defective.patch` names a changed path and line, a requirement ID from `fixtures/approved.md`, the evidence and a recommendation; `fixtures/clean.patch` has no findings.
- `agents/reviewer.json` describes a read-only, requirements-first review.
- Only `REVIEW.md` and `agents/reviewer.json` changed.

## Graded format

The grader checks these rules; a correct answer must follow them:

- `REVIEW.md` has at least 300 characters of policy prose outside code blocks that covers evidence, severity and requirement mapping, with no `TODO` and no HTML comments.
- `agents/reviewer.json` keeps exactly `purpose`, `instructions` (at least 80 characters, no `TODO`) and `"output": "findings.schema.json"`. Do not add tool, model or permission fields.
- Report only supported findings: each must point to a changed line and a requirement it breaks. Unsupported findings lose points.
- The whitespace change in `generated/styles.css` breaks no requirement. Either leave it out or report it as one `minor` finding.

## Your findings (graded)

Use your policy to review `fixtures/defective.patch` and `fixtures/clean.patch`, then
record the findings in `REVIEW.md` as exactly one JSON block (no other `json` blocks in
the file). Each patch gets an object that follows `findings.schema.json`:

- `path`: the file path without the `a/` or `b/` prefix, for example `src/status.mjs`.
- `line`: the line number in the new version of the file (the `+` line).
- `requirement`: one ID from `fixtures/approved.md` per finding.
- `severity`: `critical` breaks a patient-safety, release-gate or privacy requirement;
  `major` breaks another requirement; `minor` is style only.

```json
{
  "fixtures/defective.patch": {"findings": []},
  "fixtures/clean.patch": {"findings": []}
}
```

## How to submit

1. Fork this repository to your personal GitHub account. Use fictional information
   only, keep the fork public, and keep `main` as its default branch. In your fork's
   **Actions** tab, click **I understand my workflows, go ahead and enable them** if shown.
2. On the lab page, use **Connect GitHub for scoring** to install the read-only grading
   App on your fork, then **Recheck connection**.
3. Download the TensorLearn connector from the lab page and extract it outside your fork.
   From your fork folder, run the page's `--code` connection command and paste your token
   only into the hidden terminal prompt. This opens a Claude Code session connected to the lab.
4. In that session, run `/load`. Run `/load` again after your instructor launches the round.
5. Create a branch such as `review/first-draft` and complete the task above. Change only the
   files the task allows: any other change, including `CLAUDE.md`, `.claude/` settings or
   agents, or notes files, makes the submission invalid.
6. Commit and push your branch. Open a PR **inside your own fork**, targeting its `main`
   branch (GitHub may suggest the upstream repository: select your fork instead). Check
   that **Check review submission** starts in the PR's checks. Keep the PR open; no merge
   is required.
7. Run **`/submit`** in the connected Claude Code session, then `/check` (or refresh the
   lab page) to read your saved score. The Actions run is useful feedback, but it does not
   submit your work.

A change to your answer needs a new commit, push and `/submit`. Keep `main` unchanged:
the grader compares your PR with the released starter.

## What is graded

The pinned grader in `.github/workflows/grade.yml` reads your files as data and runs your
code only in an isolated, offline container. Every check is a fixed rule; no AI model
judges your work. Your score is out of 100. Your facilitator separately reviews
clarity and reasoning.

- REVIEW.md is a written policy covering evidence, severity and requirement mapping.
- agents/reviewer.json keeps exactly `purpose`, `instructions` and `output` (still `findings.schema.json`), with written instructions.
- Your recorded findings match the schema, find the three real defects with the right requirement and severity, and report nothing unsupported.
- The style change stays minor and the clean patch has no findings.
