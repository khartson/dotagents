---
name: change-summary
description: >-
  Summarize the current git changes into a plain commit subject, a short
  changelog, and a one-sentence release note. Use this whenever the user asks
  for a commit message, pull-request description, changelog entry, release
  notes, or a summary of what changed, including "summarize my diff", "write
  the commit", "what did I change", "draft the PR", or "release notes", even
  if they do not name this skill.
---

# Change summary

Write a paste-ready account of the current git changes. The reader is about to put it in a commit, a pull request, or a changelog, so every claim has to be visible in the diff or the commit list.

## What to summarize

Prefer uncommitted work. Staged and unstaged changes both count. Say which set you used, because `git commit` without `-a` only includes the staged set.

When the working tree is clean, summarize the current branch against the branch it diverged from. Use the upstream if it exists; otherwise use the repo's default branch. Compare with the merge-base, not with the latest commit alone.

If there is no repository, or there is nothing to summarize, say so and stop. Do not invent a summary from filenames or from earlier conversation.

## How to read it

Start with `git status`, then the relevant `git diff` (`--cached` when anything is staged). For a clean branch, read `git log` and `git diff` against the merge-base.

Read the hunks that change behavior. File names are a map, not the summary. Skip generated files, lockfile churn, and formatting-only edits unless that is the whole change; if they ride along, mention them in one line.

For a very large diff, use the stat and read the hunks that change behavior. Say when the summary is based on a sample.

If the diff contains a secret, token, or credential, do not copy the value. Name the file and say a secret is present.

## What to write

Use this shape and stop. Do not stage, commit, push, or edit files.

```markdown
## Commit message
<one imperative line>

## Summary
- <change, and why it matters>
- <change, and why it matters>

## Release note
<one sentence>
```

The commit subject is plain imperative language, with no type prefix such as `feat:` or `fix:`. Prefer the reason the change exists when the diff supports it.

Write three to six summary bullets. Group related edits. Do not walk the diff file by file.

The release note is one sentence a user of the project could read. Describe the outcome, not the files.

If the diff mixes unrelated work, aim the subject at the dominant change and say in the summary that unrelated work is included too. Offer a second subject only when one message would hide a separate change.

## Example

Input: a diff that fixes password reset by checking the token expiry, plus an unrelated formatting pass on the login page.

```markdown
## Commit message
Reject expired password-reset tokens

## Summary
- Check reset-token expiry before accepting the new password, so old links stop working.
- The login page also has a formatting-only edit that is unrelated to the fix.

## Release note
Expired password-reset links are no longer accepted.
```
