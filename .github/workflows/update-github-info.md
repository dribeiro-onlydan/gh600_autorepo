---
name: update-github-info
description: Refresh the GitHub info page with current GitHub Blog and changelog updates and propose the change as a PR for Mona review.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  pull-requests: read
  issues: read
strict: true
network:
  allowed:
    - defaults
    - github.com
    - github.blog
tools:
  github:
    mode: gh-proxy
    toolsets: [default]
  edit:
  web-fetch:
safe-outputs:
  create-pull-request:
    title-prefix: "[mona] "
    draft: true
    labels: [documentation, automation]
    allowed-files:
      - "site/content/github-info.md"
    allowed-branches:
      - "main"
      - "feature/*"
    protected-files:
      policy: request_review
    if-no-changes: "warn"
    base-branch: main
---

# Update GitHub info

## Task

Review the notes in `notes/mona-notes.md`, then fetch and review:

- https://github.blog/latest/
- https://github.blog/changelog/
- https://awesome-copilot.github.com/workflows/

Use those sources to refresh `site/content/github-info.md` so it reflects current GitHub themes, blog coverage, changelog highlights, and notable Copilot workflow inspiration. Keep the content aligned with Mona's editorial angle based on official GitHub references:

- docs.github.com
- github.blog
- github.blog/changelog

Focus on concise, valuable updates that improve the page without broad edits or unrelated changes.

## Requirements

- Read `notes/mona-notes.md` before drafting a change.
- Use the web-fetch tool to read both GitHub blog URLs above.
- Update only `site/content/github-info.md`.
- Prefer small, high-signal editorial improvements over a full rewrite.
- If no meaningful update is needed, call `noop` with a brief reason instead of creating a PR.
- When an update is warranted, use the configured safe output to open a pull request for Mona to review.
- Keep the PR draft-only and scope it to the documentation update.
- Mention in the PR summary that Mona should review wording, editorial direction, and tone.

## Safe outputs

- Use `create-pull-request` to propose the edit without writing directly to `main`.
- Keep the patch limited to `site/content/github-info.md`.
- Do not alter unrelated files.
- If there are no meaningful, source-backed changes, use `noop` rather than creating a PR.
