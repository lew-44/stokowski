# Implementation Stage

You are implementing the solution for **{{ issue.identifier }}**: {{ issue.title }}

**Current status:** {{ issue.state }}
**Labels:** {{ issue.labels }}
**URL:** {{ issue.url }}

## Issue description

{% if issue.description %}
{{ issue.description }}
{% else %}
No description provided.
{% endif %}

## Objective

Implement the solution, create an MR, and ensure it passes all quality checks.

## First run

1. Read the investigation summary from the Linear comments.
2. Read the relevant source files identified in the investigation.
3. Create a feature branch from `main`:
   ```
   git checkout -b {{ issue.identifier | lower }}-<short-description>
   ```
4. Implement the changes with clean, logical commits.
5. Run the project's real quality suite — the exact commands it documents,
   not a generic approximation. Check `CLAUDE.md` for the pre-MR checklist.
6. Fix any failures before proceeding.
7. Review your own diff, and run the project's review command if it has one
   (e.g. `/review-changes`) — slash commands work in this environment.
8. Capture before/after screenshots into `$STOKOWSKI_ARTIFACTS` for anything
   visible to a user.
9. Push the branch and create an MR:
   ```
   git push -u origin HEAD
   glab mr create --title "{{ issue.identifier }}: <concise title>" --description "<description>"
   ```
10. Link the MR to the Linear issue.
11. Write `.stokowski/report.json`: what changed and why, the exact
    verification commands and their real results, assumptions, and known
    limitations. Set `verdict` to `complete` or `blocked`, put the reviewer's
    summary in `next`, and anything they must check in `next_steps`. Add 3-5
    bullets to `key_points`: what changed, what you actually verified, and
    anything the reviewer should be suspicious of. Those four fields render at
    the top of the Linear comment and are what the gate reads first — someone
    who reads only them should know whether this is safe to merge. Stokowski
    posts it.

## Rework run

If this is a rework run (a branch and MR already exist):

1. Find the existing MR:
   ```
   glab mr list --source-branch <branch-name>
   ```
2. Read review comments and requested changes:
   ```
   glab mr view <number> --comments
   ```
3. Address each piece of feedback specifically.
4. Run the full quality suite again.
5. Push new commits to the existing branch (do not force-push).
6. Post a comment on the GitLab MR summarising the rework:
   - Which review comments were addressed
   - What was modified
   - Any decisions or trade-offs
7. Write a fresh `.stokowski/report.json` covering the rework.

## Quality bar

Before finishing, verify:

- [ ] All tests pass
- [ ] No type errors
- [ ] No lint errors
- [ ] All acceptance criteria from the ticket description met
- [ ] MR created (or updated) and linked to Linear issue
- [ ] Evidence captured to `$STOKOWSKI_ARTIFACTS` for any visible change
- [ ] `.stokowski/report.json` written, every claim sourced
