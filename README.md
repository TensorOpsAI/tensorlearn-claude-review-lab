# Review without the author’s bias

A PR (`fixtures/defective.patch`) says all requirements were verified (`fixtures/author-narrative.txt`).
In fact it removes caching, logs whole patient results and weakens the release rule, alongside a harmless
style change. `fixtures/clean.patch` is a small correct change. The requirements are in `fixtures/approved.md`.

**What you'll practise:** turning Claude into a reviewer you can rely on: a written policy decides what counts
as a finding, and the review runs in a clean context.

**In the course slides:** Chapter 2, Lesson 3 (AI in the PR review loop): REVIEW.md, findings that cite the
requirements, and a cap on nits. The slides' *Important* is `critical` or `major` here; a *nit* is `minor`.

## Task

1. Read the requirements in `fixtures/approved.md`.
2. Before writing a policy, ask Claude to review `fixtures/defective.patch` against them. Check each finding: is it
   the right changed line, one requirement and a sensible severity? Note what a policy should settle.
3. Write your policy in `REVIEW.md` (evidence, severity, one requirement per finding, uncertainty, a cap on
   minor comments) and the `instructions` in `agents/reviewer.json` (keep its other two fields).
4. Run the review in a subagent or fresh session that sees only `fixtures/approved.md`,
   `fixtures/defective.patch` and `REVIEW.md`, and returns findings in the `findings.schema.json` shape.
5. Repeat with `fixtures/author-narrative.txt` added. In `REVIEW.md`, note what changed; if nothing did, say
   your policy held.
6. Review `fixtures/clean.patch` the same way, compare with step 2, and record the final findings for both
   patches in the graded block below.

Give Claude one step at a time and read what it reports. Keep review output in the chat, not in new files.

## Rules

- Change only `REVIEW.md` and `agents/reviewer.json`. The reviewer only reads: it does not edit files, run
  commands, approve or merge.

## Acceptance targets

- Findings cite changed paths/lines, evidence and violated requirements.
- Material privacy, release-gate and caching defects are found.
- The clean patch does not receive invented defects.
- The reviewer remains read-only and does not approve or merge.

## Done when

- `REVIEW.md` has your policy (no TODO), the narrative note and one `json` block with findings for both patches.
- `fixtures/clean.patch` has no findings; `agents/reviewer.json` describes a read-only, requirements-first review.

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
